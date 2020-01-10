---
title: "用辅助函数来取代复杂的表达式"
date: 2016-12-19T12:09:00+08:00
tags: ["Yii2", "编程思想"] 
draft: false
toc: true
---
## 引言

上上上个月在图书馆淘到一本书叫[《Effective Python - 编写高质量Python代码的59个有效方法》](https://book.douban.com/subject/26709315/)，虽然我不用 Python 写代码，但是好歹以前写过一点 Python 的皮毛。
豆瓣看了一下评分，就果断借了这本书。当看到第四条方法《第4条：用辅助函数来取代复杂的表达式 》结合自己最近几年编程经验，深有感触，于是就有了这篇文章。

## 为什么要用辅助函数？

在编程的时候，你肯定会遇到类似这样的事情：数据库中保存的商品单价单位是分，商品详情页需要你的商品价格，但是单位肯定是元。

那么我们可以怎么样实现呢？一般的做法肯定这样的，直接在视图页面要展示价格的地方这样写：

```php
<?= round(($price / 100), 2) ?>
```

这样写虽然能实现效果，但是如果需求有变化，需要使用强制保留两位小数，那么你需要这样改：

```php
<?= number_format(round(($price / 100), 2), 2, '.', ''); ?>
```

看上去虽然不多的代码，但是

- 阅读起来很困难，而且很上去也很乱。
- 如果涉及到很多东西的话，需要找到每个相应的地方然后做修改。同理，下次改需求的时候，修改也将会是一件很痛苦的事情。

<!--more-->

如果使用辅助函数来实践的话，代码将会是这样的：

```php
/**		
* 分转元		
* @param int $price 分		
* @return float 元		
*/
function fenToYuan($price)
{
    $price = round(($price / 100), 2);
    return number_format($price, 2, '.', '');
}
// 视图层使用方式
echo fenToYuan($price);
```

一旦这样使用之后，代码将变得非常清晰，而且以后重构起来也非常方便。以上是为了说明辅助函数的作用特意列举的一个小示例，实际项目中，可能会有大量需要你使用这种方式编程的地方，你需要养成这种思维模式才是最重要的。

PS：以上代码可以在[这里](http://ideone.com/miLLM3)查看并且运行。

## Yii2 中是如何实践的？

Yii2 中有一个 [helpers](https://github.com/yiisoft/yii2/tree/master/framework/helpers) 文件夹，里面的代码推荐各位 PHP 工程师都应该去看看，注释都给了示例，非常的友好。

下面我来简单分享一些常用的 helpers：

**[ArrayHelper](https://github.com/yiisoft/yii2/blob/master/framework/helpers/BaseArrayHelper.php)**

```php
// 最基本的用法，获取数组中的某个键对应的值。好处是不必判断 username 是否存在
$username = \yii\helpers\ArrayHelper::getValue($_POST, 'username');
// 也可以获取对象中的某个值
$username = \yii\helpers\ArrayHelper::getValue($user, 'username');
// 也可以使用匿名函数
$fullName = \yii\helpers\ArrayHelper::getValue($user, function ($user, $defaultValue) {
    return $user->firstName . ' ' . $user->lastName;
});
// 使用「.」获取关联对象的属性
$street = \yii\helpers\ArrayHelper::getValue($users, 'address.street');
// 获取数组键值的数组键值
$versions = ['date' => '2016年12月19日', '1.0' => ['date' => '2016年12月18日']];
$value = \yii\helpers\ArrayHelper::getValue($versions, ['1.0', 'date']); // $value 输出为 2016年12月18日
```
PS：以上代码可以在[这里](http://ideone.com/T9d3Qb)查看并且运行。

有点太花时间，以后再补充……


## 使用其他 helpers

据我了解，Yii2 的 helpers 在其他 PHP 框架中也是可以使用的，但是本人没有亲测，不作保证。

类似这种 helpers 是可以在网上找到的，比方说我就找到了[phpfunct/funct](https://github.com/phpfunct/funct)

当然也可以自己根据需要，收集使用，比方说根据这篇文章收集到 UUID 的生成方法：


```php
class Sequence
{
    const EPOCH = 1000000000000;

    const TIME_BITS  = 41;
    const NODE_BITS  = 10;
    const COUNT_BITS = 10;

    private $node = 0;

    private $ttl = 10;

    public function __construct($node)
    {
        $max = $this->max(self::NODE_BITS);

        if (is_int($node) === false || $node > $max || $node < 0) {
            throw new \InvalidArgumentException('node');
        }

        $this->node = $node;
    }

    public function generate($time = null)
    {
        if ($time === null) {
            $time = (int)(microtime(true) * 1000);
        }

        return ($this->time($time) << (self::NODE_BITS + self::COUNT_BITS)) |
               ($this->node << self::COUNT_BITS) |
               ($this->count($time));
    }

    public function restore($id)
    {
        $binary = decbin($id);

        $position = -(self::NODE_BITS + self::COUNT_BITS);

        return array(
            'time'  => bindec(substr($binary, 0, $position)) + self::EPOCH,
            'node'  => bindec(substr($binary, $position, - self::COUNT_BITS)),
            'count' => bindec(substr($binary, - self::COUNT_BITS)),
        );
    }

    public function setTTL($ttl)
    {
        $this->ttl = $ttl;
    }

    private function time($time)
    {
        $time -= self::EPOCH;

        $max = $this->max(self::TIME_BITS);

        if (is_int($time) === false || $time > $max || $time < 0) {
            throw new \InvalidArgumentException('time');
        }

        return $time;
    }

    private function count($time)
    {
        $key = "seq:count:{$time}";

        while (!$count = apcu_inc($key)) {
            apcu_add($key, mt_rand(0, 9), $this->ttl);
        }

        $max = $this->max(self::COUNT_BITS);

        if ($count > $max) {
            throw new \UnexpectedValueException('count');
        }

        return $count;
    }

    private function max($bits)
    {
        return -1 ^ (-1 << $bits);
    }
}
```

## 最后总结

编写可读性，可维护性，可扩展性的代码应该是每个开发工程师去追求的目标。程序员可以说是80%的时间都是在维护代码和阅读代码（包括别人的和自己的），所以一定要重视这些小细节。

但实际上是很多人连最基本的[单一职责原则](http://baike.baidu.com/view/4779987.htm)都做不到，虽然有可能他们知道这个原则，但是不实践又有什么用呢？！
