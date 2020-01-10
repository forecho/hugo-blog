---
title: "MWS 开发 - 订单篇"
date: 2019-07-08T22:58:00+08:00
tags: ["MWS开发"] 
draft: false
toc: true
---

## 引言

最近这几个月都在帮一个朋友在开发亚马逊 ERP 系统，使用了 [MWS 接口](http://docs.developer.amazonservices.com/en_US/dev_guide/index.html)，自己一路摸索，花了不少时间和精力，现在想把这些经验记录分享出来，让大家少走一些弯路。

这将是一系列文章，本篇主要讲与订单相关的接口。

<!--more-->

## SDK

MWS 的接口文档在[这里](http://docs.developer.amazonservices.com/en_US/orders-2013-09-01/Orders_Overview.html)（中文文档不是最新的）查看，但是官方给的 PHP SDK 非常的老了，而且每个功能模块还是分散的，于是我就去 Github 找到了 Star 最多的一个，即 [meertensm/amazon-mws](https://github.com/meertensm/amazon-mws)，好用是好用，但是我使用之前没发现作者好像没维护了，有一些坑在里面。

好在这些坑我都帮你踩过了，你接着往下看就可以了。

## 订单相关的功能

### 同步订单

**订单列表**

文档地址：[ListOrders](http://docs.developer.amazonservices.com/zh_CN/orders/2013-09-01/Orders_ListOrders.html)

```php
$client = new MCS\MWSClient([
    'Marketplace_Id' => '',
    'Seller_Id' => '',
    'Access_Key_ID' => '',
    'Secret_Access_Key' => '',
    'MWSAuthToken' => '' // Optional. Only use this key if you are a third party user/developer
]);
$fromDate = new DateTime('2016-01-01');
$states = ['Unshipped','PartiallyShipped','Shipped','PendingAvailability','Pending','InvoiceUnconfirmed','Canceled','Unfulfillable'];
$client->ListOrders($fromDate, false, $states);
```

SDK 的 [ListOrders()](https://github.com/meertensm/amazon-mws/blob/master/src/MWSClient.php#L383) 默认的 `states` 只有两个状态。

**订单商品**

要想获取订单里的商品信息要调用 [ListOrderItems()](https://github.com/meertensm/amazon-mws/blob/master/src/MWSClient.php#L493) 

**订单商品的属性和图片**

要想获取订单商品里的属性和图片信息得调用 [GetMatchingProductForId()](https://github.com/meertensm/amazon-mws/blob/master/src/MWSClient.php#L553)，商品属性目前应该只有 `Size` 和 `Color`。

注意：如果接口返回 `Request is throttled`，那你就要考虑接口频率问题了，详情就得查看文档[《限制：针对提交请求频率的限制》](http://docs.developer.amazonservices.com/zh_CN/dev_guide/DG_Throttling.html)了。

### 发货

**思路**

发货是一个异步接口，流程就是先创建一个类型为 `_POST_ORDER_FULFILLMENT_DATA_` 的 `SubmitFeed`，获取到一个 `FeedSubmissionId`，然后稍候通过另外一个接口传 `FeedSubmissionId` 查询发货结果。

**实践**

坑的是前面提到的 [meertensm/amazon-mws](https://github.com/meertensm/amazon-mws) 库作者没有实现发货方法，提了 PR 作者也没时间管了，所以我自己实现了。

- 先创建一个 `core/replace/MWSClient.php` 文件:

```php
<?php
/**
 * author     : forecho <caizhenghai@gmail.com>
 * createTime : 2019/6/11 9:53 PM
 * description:
 */

namespace app\core\helpers;

use Exception;

class MWSClient extends \MCS\MWSClient
{
    /**
     * Sets the shipping status of an order
     * @param array $data required data
     * @return array feed submission result
     * @throws Exception
     */
    public function setDeliveryState(array $data)
    {
        if (!isset($data["shippingDate"])) {
            $data["shippingDate"] = date("c");
        }

        if (!isset($data["carrierCode"]) && !isset($data["carrierName"])) {
            throw new Exception('Missing required carrier data');
        }

        $feed = [
            'MessageType' => 'OrderFulfillment',
            'Message' => [
                'MessageID' => rand(),
                "OrderFulfillment" => [
                    "AmazonOrderID" => $data["orderId"],
                    "FulfillmentDate" => $data["shippingDate"]
                ]
            ]
        ];
        $fulfillmentData = [];


        if (isset($data["carrierCode"])) {
            $fulfillmentData["CarrierCode"] = $data["carrierCode"];
        } elseif (isset($data["carrierName"])) {
            $fulfillmentData["CarrierName"] = $data["carrierName"];
        }

        if (isset($data["shippingMethod"])) {
            $fulfillmentData["ShippingMethod"] = $data["shippingMethod"];
        }


        if (isset($data["trackingCode"])) {
            $fulfillmentData["ShipperTrackingNumber"] = $data["trackingCode"];
        }

        if (sizeof($fulfillmentData) > 0) {
            $feed["Message"]["OrderFulfillment"]["FulfillmentData"] = $fulfillmentData;
        }
        $feed = $this->SubmitFeed('_POST_ORDER_FULFILLMENT_DATA_', $feed);

        return $feed;
    }
}
```

- 然后修改第一步调用 `MWSClient` 类的时候使用我们自己的类：

```php
$client = new app\core\helpers\MWSClient([
    'Marketplace_Id' => '',
    'Seller_Id' => '',
    'Access_Key_ID' => '',
    'Secret_Access_Key' => '',
    'MWSAuthToken' => '' // Optional. Only use this key if you are a third party user/developer
]);
```


- 这样之后我们就可以这样发货了：

```php
$client->setDeliveryState([
    "orderId" => 3456342,
    "carrierCode" => "UPS",
    "trackingCode" => "34JD943T6",
    "shippingMethod" => "1 Day",
]);

$client->setDeliveryState([
    "orderId" => 3456342,
    "carrierName" => "Mini transporters A.C.",
    "trackingCode" => "34JD943T6",
]);

$client->setDeliveryState([
    "orderId" => 3456342,
    "carrierName" => "Mini transporters A.C.",
    "trackingCode" => "34JD943T6",
    "shippingDate" => "2018-06-21T12:32:24+02:00"
]);
```

以上实践代码我是根据 [Update order - set delivery state #55](https://github.com/meertensm/amazon-mws/issues/55) 这个 Issues 采坑而来的。

- 查询发货结果

无论成功或者失败都可以通过 [GetFeedSubmissionResult()](https://github.com/meertensm/amazon-mws/blob/master/src/MWSClient.php#L956) 方法获取结果。

PS: 可以分享一下我发货采坑的过程：

MWS 对应的接口都会有对应的参数 XMLSchema，是专门用来校验接口传过去的参数，我们可以找一个在线 XML 校验的网站，校验我们要传过去的值是否正确。异步接口调试非常的麻烦，而且亚马逊返回的错误信息又不足够详细。

- [OrderFulfillment 的 XMLSchema](https://images-na.ssl-images-amazon.com/images/G/01/rainier/help/xsd/release_1_9/OrderFulfillment.xsd)
- [在线 XML 校验](https://www.liquid-technologies.com/online-xsd-validator)

## 总结

发现国内使用 MWS 的文档和资料比较少，自己辛辛苦苦踩的坑，当然要分享出来，希望对你有用。

## 相关文章

- [《MWS 开发 - 订单篇》](https://blog.forecho.com/mws-order.html)
- [《MWS 开发 - 产品上传篇》](https://blog.forecho.com/mws-product.html)