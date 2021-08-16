---
title: "我是如何用 Golang 做接口测试的"
date: 2021-08-16T10:59:00+08:00
tags: ["技术", "Golang"]
draft: false
toc: true
---

## 引言

最近公司项目用 Golang 重写，总算是有机会实战了。这篇文章主要分享与记录我是怎么在 Golang 的项目中写接口测试的。

因为是换语言重写 API 项目，所以一定要保证新旧项目对外输出的结果一致性。由于我使用 Golang 的时间不算太久，本篇文章如有错误、不足之处，还请大家多多指教。

<!--more-->

## 接口测试

接口测试的代码有参考 [qiangxue/go-rest-api](https://github.com/qiangxue/go-rest-api/blob/master/internal/auth/api_test.go#L21) 的代码，但是因为使用的包和结构也不太一样，所以会有调整，但是思路是差不多的。

### tests 包

先建一个 tests 包，写一个 `Endpoint` 方法：


```go
package tests

import (
	"bytes"
	"fmt"
	"github.com/julienschmidt/httprouter"
	"github.com/kinbiko/jsonassert"
	"github.com/stretchr/testify/assert"
	"net/http"
	"net/http/httptest"
	"testing"
)

// APITestCase represents the data needed to describe an API test case.
type APITestCase struct {
	Name         string
	Method       string
	URL          string
	Body         string
	Header       http.Header
	WantStatus   int
	WantResponse string
}


// Endpoint tests an HTTP endpoint using the given APITestCase spec.
func Endpoint(t *testing.T, router *httprouter.Router, tc APITestCase) {
	t.Run(tc.Name, func(t *testing.T) {
		req, _ := http.NewRequest(tc.Method, tc.URL, bytes.NewBufferString(tc.Body))
		if tc.Header != nil {
			req.Header = tc.Header
		}
		if req.Header.Get("Content-Type") == "" {
			req.Header.Set("Content-Type", "application/json")
		}
		res := httptest.NewRecorder()
		router.ServeHTTP(res, req)
		assert.Equal(t, tc.WantStatus, res.Code, "status mismatch")
		if tc.WantResponse != "" {
			ja := jsonassert.New(t)
			ja.Assertf(res.Body.String(), tc.WantResponse)
		}
	})
}


func MockAuthHeader(JWT string) http.Header {
	header := http.Header{}
	header.Set("Authorization", fmt.Sprintf("Bearer %v", JWT))
	return header
}
```

几点需要说明：
- 我们路由用的是 `julienschmidt/httprouter` 此处你可以更改适用你们的
- `stretchr/testify` 包是 Go 用的最多的断言包，非常好用，出现断言失败，输出结果清晰明了，推荐。
- `kinbiko/jsonassert` 这个包可以断言两个 `JSON` 的值是否相等，使用这个包是因为 `stretchr/testify` 不满足我们的需求，我们每个接口都会返回 `request_id` 这个值是唯一且随机的，所以我们断言的时候不必考虑这个值，使用的 `kinbiko/jsonassert` 的 `<<PRESENCE>>` 就能很好的满足我们的需求。另外由于 WantResponse 值我是直接拿的老项目返回的值做比对的， `kinbiko/jsonassert` 断言 `JSON` 时不会考虑顺序，这个也正是我们需要的功能。

### 接口测试

一个示例：

```go
func mockNewServer() *Server {
	return NewServer()
}

func TestAccount(t *testing.T) {
	c := mockNewServer()
	JWT, _ := jwt.MakeJWT(tests.ClientId, tests.ClientSecret)
	header := tests.MockAuthHeader(JWT)

	items := []tests.APITestCase{
		{
			"test_unauthorized",
			"GET",
			"/api/v1/account/1155501",
			"",
			nil,
			http.StatusUnauthorized,
			`{"request_id":"<<PRESENCE>>","code":50000,"status_code":401,"message":"token 验证失败：未识别的 payload"}`,
		},
		{
			"get_account_info",
			"GET",
			"/api/v1/account/1155501",
			"",
			header,
			http.StatusOK,
			`{"request_id":"<<PRESENCE>>","message":"success","code":0,"data":{"union_id":1155501,"status":"valid","created_at":"2021-06-18T14:57:18+08:00","updated_at":"2021-06-22T10:44:26+08:00"}}`,
		},
		{
			"account_not_found",
			"GET",
			"/api/v1/points/account/1551734",
			"",
			header,
			http.StatusOK,
			`{"request_id":"<<PRESENCE>>","code":42003,"status_code":400,"message":"parameter union_id is invalid."}`,
		},
	}
	for _, tc := range items {
		tests.Endpoint(t, c.router, tc)
	}
}
```

- `mockNewServer` 就是起一个 Server，具体每个项目都不太一样
- 接口如果需要鉴权的话，要传入 `token` 数据。 我们使用的是在 `header` 里面传入 `JWT`

## Mock 第三方接口

有时候我们的项目会依赖另外一个项目的接口，就比分说发通知的功能，我们会单独请求一个通知系统的接口给用户发通知，这种情况如何写测试代码呢？

首先要明确的是我们不会测试第三方接口的服务，所以这里我们就断言第三方接口一定会返回我们需要的数据。使用 Mock 方式就能达到这个需求。

我们使用的是 [jarcoal/httpmock](https://github.com/jarcoal/httpmock) 这个包，可以很好的满足我们的需求。

使用方法我们分两步：

### 先定义 Mock 数据

```go
type mockServiceAPI struct {
	Method       string
	URL          string
	Body         string
	WantStatus   int
	WantResponse string
}

func MockNotificationServiceAPI() {
    // 读配置文件
	domain := config.GetString("notification_service_api")
	items := []mockServiceAPI{
		{
			http.MethodPost,
			"/api/v1/template/send",
			`{"app_key":"website","topic":"points","topic_sub_key":"login","types":["mail"],"data":[{"union_id":1551630,"template_data":{"url":"https://blog.forecho.com/"}}]}`,
			http.StatusOK,
			`{"code": 200,"msg": "请求成功","data": []}`,
		},
	}
	for _, item := range items {
		httpmock.Activate()
		httpmock.RegisterResponder(
			"POST",
			fmt.Sprintf("%s%s", domain, item.URL),
			httpmock.NewStringResponder(item.WantStatus, item.WantResponse),
		)
	}
}
```

### 使用 Mock

```go
func TestSendNotification(t *testing.T) {
	tests.MockNotificationServiceAPI()
	defer httpmock.DeactivateAndReset()
    // something test code
}
```

加入上面两行代码之后，跑测试时请求的第三方接口（URL 一致的话）会直接返回我们的 Mock 数据，非常方便。


## 总结

测试是非常有必要写的，特别是核心代码的测试一定要覆盖到，为以后重构或者修改需求都带来更多保障。

回顾一下今天主要就是分享了一下接口功能测试以及如何 Mock API 返回的数据，帮忙更好的写测试。

希望本篇文章分享对你有帮助。