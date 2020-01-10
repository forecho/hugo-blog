---
title: "Octopress 迁移到 Hugo"
date: 2020-01-02T18:48:00+08:00
tags: ["Octopress"] 
draft: false
toc: true
---

## 引言

终于还是受不了 Octopress 了，准备迁移到 [Hugo](https://gohugo.io/)，受不了 Octopress 是因为：

- markdown code 语法支持不好，很多时候莫名其妙的 Build 失败，让人抓狂。（最主要原因）
- 生成速度太慢，目前生成一次要2分多钟了
- Octopress 项目已经没人维护了

<!--more-->

## 安装 Hugo

如果是 Mac 的话直接：

```sh
brew install hugo
hugo new site blog
cd blog
git init
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
echo 'theme = "ananke"' >> config.toml
```

然后就可以用下面命令生成文章了：

```sh
hugo new posts/my-first-post.md
```

开启服务

```sh
hugo server -D
```

然后访问 <http://localhost:1313/> 就可以看到你的 Hugo 博客了。

## 迁移文章

官方有[迁移指南](https://gohugo.io/tools/migrations/)，Octopress 迁移到 Hugo 官方推荐使用 [octohug](https://github.com/codebrane/octohug)，但是我使用下来发现不好用，迁移完之后有小问题。于是我找了一篇文章，结合文章给的代码，自己完善了功能，终于达到我要的效果，参考代码如下：

```go
package main

import (
	"flag"
	"fmt"
	"io/ioutil"
	"os"
	"regexp"
	"strings"
)

func main() {
	var src, dst string
	flag.StringVar(&src, "src", "", "source")
	flag.StringVar(&dst, "dst", "", "destination")
	flag.Parse()

	if _, err := os.Stat(src); os.IsNotExist(err) {
		panic("source directory does not exist")
	}

	if _, err := os.Stat(dst); os.IsNotExist(err) {
		if err := os.Mkdir(dst, os.ModePerm); err != nil {
			panic(err)
		}
	}

	files, err := ioutil.ReadDir(src)
	if err != nil {
		panic(err)
	}

	for _, file := range files {
		filename := file.Name()
		re := regexp.MustCompile(`^\d{4}-\d{2}-\d{2}-(.*).m(arkdown|d)`)
		// Ignore non-matching filenames (i.e. do no dereference nil)
		if matches := re.FindStringSubmatch(filename); matches == nil {
			continue
		}

		in := fmt.Sprintf("%s%s", src, filename)
		data, err := ioutil.ReadFile(in)
		if err != nil {
			panic(err)
		}

		re = regexp.MustCompile(`(\d{4}-\d{2}-\d{2})-(.*)`)
		newFilename := re.ReplaceAllString(filename, "$2")

		re = regexp.MustCompile(`---\n([\s\S]*)---\n([\s\S]*)`)
		matches := re.FindSubmatch(data)
		header := string(matches[1])
		body := string(matches[2])
		draftline := "draft: false\n"

		// date
		re = regexp.MustCompile(`date: (\d{4}-\d{2}-\d{2}) (\d{2}:\d{2}).*\n`)
		header = re.ReplaceAllString(header, `date: ${1}T$2:00+08:00`+"\n")

		// layout
		re = regexp.MustCompile(`layout: .*\n`)
		header = re.ReplaceAllString(header, "")

		// comments
		re = regexp.MustCompile(`comments: .*\n`)
		header = re.ReplaceAllString(header, "")

		// categories
		re = regexp.MustCompile(`categories: (.*)\n`)
		matches = re.FindSubmatch([]byte(header))
		if categories := string(matches[1]); categories != "" {
			tags := strings.ReplaceAll(categories, " ", "\", \"")
			header = re.ReplaceAllString(header, fmt.Sprintf("tags: [\"%s\"] \n", tags))
		}

		more := "toc: true\n"
		header = fmt.Sprintf("%s%s%s", header, draftline, more)

		content := fmt.Sprintf("---\n%s---\n%s", header, body)

		// toc
		toc := "* list element with functor item\n{:toc}\n"
		content = strings.ReplaceAll(content, toc, "")

		content = strings.ReplaceAll(content, "{{ .Site.BaseURL }}", "{{ .Site.BaseURL }}")

		out := dst + newFilename
		fmt.Printf("%s -> %s\n", in, out)

		if err := ioutil.WriteFile(out, []byte(content), 0644); err != nil {
			panic(err)
		}
	}
}
```


### 使用方式

在 Octopress 博客根目录下新建一个 `main.go` 文件，然后把上面代码拷贝进去，然后执行下面代码完成迁移：

```sh
go run main.go -src octopress-blog/blog/_posts/ -dst hugo-blog/content/posts/
```

## 最后

这篇文章主要是分享 Octopress 迁移 Hugo 文章的经验，上面的迁移代码不负责，有一点点 Golang 经验应该是看得懂的，你可以根据自己的需求修改代码。


## 参考链接

- [Migrating from Jekyll to Hugo](https://miguelmota.com/blog/migrating-from-jekyll-to-hugo/)