---
title: "MWS 开发 - 产品上传篇"
date: 2019-08-08T22:58:00+08:00
tags: ["MWS 开发"] 
draft: false
toc: true
---

## 引言

上次我们讲了 [MWS 与订单相关的开发](https://blog.forecho.com/mws-order.html)，这次我们就来讲如何通过 MWS 接口上传产品。


<!--more-->

## 上传产品

上传产品有几种方式，此处我们采用的是提交类型为 [`_POST_FLAT_FILE_LISTINGS_DATA_`](https://docs.developer.amazonservices.com/en_US/feeds/Feeds_FeedType.html) 的 Feed 方式来上传产品。因为这种方式兼容性比较强。

### 思路

思路就是先生成一个包含产品信息的 csv 文件，再去请求 MWS 接口。

### 实现

[《订单篇》](https://blog.forecho.com/mws-order.html)我有提到一个 MWS PHP 的库，但是再开发产品上传功能的时候，被这个库坑惨了，由于作者不维护了，即使提了 PR 也没人合并，所以我 Fork 了一份，自己直接改了代码，修复了 bug，欢迎你们使用我的这个库 [forecho/amazon-mws](https://github.com/forecho/amazon-mws)。

**安装库**

使用方式，先修改你项目的 `composer.json` 文件，添加如下代码

```
"require": {
    // ...
    "mcs/amazon-mws": "*",
    // ...
},
"repositories": [
    {
        "name": "mcs/amazon-mws",
        "type": "git",
        "url": "git@github.com:forecho/amazon-mws.git"
    }
]
```

然后再执行如下命令：

```
composer require mcs/amazon-mws
```

> 如果在执行命令行的时候提示需要输入 `Token` 的话，解决办法就是去 GitHub 的 [Personal access tokens](https://github.com/settings/tokens) 页面，点击「Generate new token」新建一个 Token，选择 `public_repo` ，然后就会得到一个 Token，然后去终端输入这个值就可以继续了。

原来的库上传产品非常简单，更是没有提供 `_POST_FLAT_FILE_LISTINGS_DATA_` 的方式上传产品，我在这个 issue#30 - [Add Product Error](https://github.com/meertensm/amazon-mws/issues/30) 的基础上加了此功能。

**上传产品的伪代码**

下面是我上传产品的伪代码，可以拿来参考：

```php
<?php
public function uploadAmazon(string $productType, Product $product, int $productNode, $otherAttributes = []) 
{
    $client = new MCS\MWSClient([
        'Marketplace_Id' => '',
        'Seller_Id' => '',
        'Access_Key_ID' => '',
        'Secret_Access_Key' => '',
        'MWSAuthToken' => '' // Optional. Only use this key if you are a third party user/developer
    ]);
    $amazonProducts = [];
    $postItems = [];
    $amazonProduct = new AmazonMarketPlaceProduct();
    if ($product->productSku) {
        $amazonProduct->setSku($product->parent_sku)
            ->setFeedProductType($productType)
            ->setBrand($product->brand)
            ->setTitle($product->title)
            ->setManufacturer($product->manufacturer)
            ->setRecommendedBrowseNodes($productNode)
            ->setParentChild('Parent')
            ->setOtherAttributes(\app\core\helpers\ArrayHelper::clearValue($otherAttributes))
            ->setVariationTheme($product->variation_theme);
        array_push($amazonProducts, $amazonProduct);
        array_push($postItems, $amazonProduct->toArray(false));

        foreach ($product->productSku as $productSku) {
            $retailPrice = $productSku->retail_price ?: 0;
            $salePrice = $productSku->sale_price ?: 0;
            $saleDate = $salePrice ? explode('~', $productSku->sale_date) : [];

            $_amazonProduct = clone $amazonProduct;
            $_amazonProduct->setSku($productSku->product_sku)
                ->setFeedProductType($productType)
                ->setBrand($product->brand)
                ->setTitle($product->title)
                ->setManufacturer($product->manufacturer)
                ->setPrice($retailPrice > 0 ? CurrencyConverter::CNYConverter($currency, $retailPrice) : '')
                ->setSalePrice($salePrice > 0 ? CurrencyConverter::CNYConverter($currency, $salePrice) : '')
                ->setProductId($amazonProductId)
                ->setSizeName($productSku->size ?: '')
                ->setColorName($productSku->color ?: '')
                ->setProductIdType('EAN')
                ->setCurrency($currency)
                ->setConditionType('New')
                ->setWeight($product->weight)
                ->setQuantity($product->quantity)
                ->setParentChild('Child')
                ->setParentSku($parentSku)
                ->setSaleFromDate(count($saleDate) == 2 ? $saleDate[0] : '')
                ->setSaleEndDate(count($saleDate) == 2 ? $saleDate[1] : '')
                ->setVariationTheme($product->variation_theme)
                ->setKeywords($product->search_keyword)
                ->setRecommendedBrowseNodes($productNode)
                ->setBulletPoint($features)
                ->setDescription($product->description)
                ->setOtherAttributes($otherAttributes)
                ->setImage($productSku->images);
            array_push($amazonProducts, $_amazonProduct);
            array_push($postItems, $_amazonProduct->toArray(false));
        }
    } else {
        $retailPrice = $product->price ?: 0;
        $salePrice = $product->sale_price ?: 0;
        $saleDate = $salePrice ? explode('~', $product->sale_date) : [];

        $amazonProduct->setSku($parentSku)
            ->setFeedProductType($productType)
            ->setBrand($product->brand)
            ->setTitle($product->{$titleAttribute})
            ->setManufacturer($product->manufacturer)
            ->setPrice($retailPrice > 0 ? CurrencyConverter::CNYConverter($currency, $retailPrice) : '')
            ->setSalePrice($salePrice > 0 ? CurrencyConverter::CNYConverter($currency, $salePrice) : '')
            ->setProductId($amazonProductId)
            ->setProductIdType('EAN')
            ->setCurrency($currency)
            ->setConditionType('New')
            ->setWeight($product->weight)
            ->setQuantity($product->quantity)
            ->setSaleFromDate(count($saleDate) == 2 ? $saleDate[0] : '')
            ->setSaleEndDate(count($saleDate) == 2 ? $saleDate[1] : '')
            ->setKeywords($product->search_keyword)
            ->setRecommendedBrowseNodes($productNode)
            ->setBulletPoint($features)
            ->setDescription($product->description)
            ->setOtherAttributes($otherAttributes)
            ->setImage($product->images);
        array_push($amazonProducts, $amazonProduct);
        array_push($postItems, $amazonProduct->toArray(false));
    }

    // You can also submit an array of MWSProduct objects
    $feed = $client->postProduct(
        $amazonProducts,
        'fptcustomcustom',
        '2019.0501',
        'SE9NRV9MSUdIVElOR19BTkRfTEFNUFM='
    );
    Yii::info($postItems, $product->id . '上传产品数据');
    return $feed;
}
```

上传产品是一个异步接口，提交成功会返回如下数据：

```php
[
  'FeedSubmissionId' => 'xxxxxxx', // 一串数字
  'FeedType' => '_POST_FLAT_FILE_LISTINGS_DATA_',
  'SubmittedDate' => '2019-08-08T10:15:29+00:00',
  'FeedProcessingStatus' => '_SUBMITTED_',
]
```

要想获取结果，需要过两分钟之后再去请求另外一个接口，你可以用下面方法实现：

```php
$client->GetFeedSubmissionResult($FeedSubmissionId);
```

如果成功返回的信息是了类似这样的：

```
Feed Processing Summary:
	Number of records processed		2
	Number of records successful		2


```


如果失败就类似这样的：

```
Feed Processing Summary:
	Number of records processed		4
	Number of records successful		3

original-record-number	sku	error-code	error-type	error-message
14	3uC000670-Random combination-Large*2+Sma	90118	Error	The item_sku field contains an invalid value: 3uC000670-Random combination-Large*2+Small*2. The value exceeds the maximum number of bytes allowed: 40.

```

## 注意事项

### 编码问题

我最开始上传产品的时候，产品上传成功了，但是去亚马逊后台看，产品是乱码的，解决这个问题花了我几天时间。导致这个问题出现的原因就是编码问题。

[meertensm/amazon-mws](https://github.com/meertensm/amazon-mws/blob/master/src/MWSClient.php#L1221) 提交接口的时候使用的是 `iso-8859-1` 编码，这意味着上传 CSV 文件的时候，要先把内容转成对应的格式。问题是**并非所有的亚马逊站点都是用这个编码，日本就是用 `UTF-8` 编码的**，所以之前上传产品到日本站点总算乱码。听说中国站点也是 `UTF-8` 编码，不过这已经不重要了，因为亚马逊这部分业务已经退出中国了。**此问题我已经在 [forecho/amazon-mws](https://github.com/forecho/amazon-mws) 解决**，你可以不用关心此细节。

另外一个需要注意的编码问题就是，在使用 `GetFeedSubmissionResult()` 方法获取刊登结果时候，日本站返回的数据是 `Shift-JIS` 的，这个需要你手动转一下：

```php
if ($isJp) {
    $string = mb_convert_encoding($string, 'UTF-8', 'Shift-JIS')
}
```

### 产品图片

上传产品的时候图片我们是给的图片地址链接，如果图片放在国内，很容易出现亚马逊下载产品图片超时导致的刊登失败。所以我们要想办法把我们的图片存到海外，比方说使用阿里的 OSS。

如果因为用了阿里 OSS 的海外 Bucket，导致我们在使用的时候上传图片很慢，可以了解一下这个[《OSS 全球传输加速开启公测，助力企业业务全地域覆盖》](https://yq.aliyun.com/articles/688039)。

### UPC/EAN

这个可以通过购买生成器生成 UPC 和 EAN，也可以购买合法 UPC/EAN。生成器生成的容易导致上传产品失败，换一个重新上传产品即可。有时候发现你上传的产品出现别人的图片，不要慌，这个就是因为 UPC/EAN 跟别人重复导致的，换一个重新上传产品即可。

### 关于产品描述

产品描述除了不能大于 2000 字符串之外，还不能使用除了 `<br>`（换行）和 `<b></b>`（加粗）之外的 HTML 标签。

### SKU 异常

出现上传产品失败，错误信息 SKU 异常的，如 8008 错误，这种情况一般也是因为 UPC/EAN 有问题导致的，我们可以把异常的 SKU 的 UPC/EAN 在 [美国统一代码委员会网站
](http://gepir.gs1.org/index.php/search-by-gtin) 查询，如果查询到有结果，说明你就不能使用此 UPC/EAN，换一个 UPC/EAN 重新上传即可。

## 总结

上传产品也是比较核心的功能，开发此功能比我开发同步订单花了更多时间，踩了不少坑，现在分享出来，让大家少走一点弯路。

## 相关文章

- [《MWS 开发 - 订单篇》](https://blog.forecho.com/mws-order.html)
- [《MWS 开发 - 产品上传篇》](https://blog.forecho.com/mws-product.html)

