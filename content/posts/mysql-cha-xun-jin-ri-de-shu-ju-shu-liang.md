---
title: "MySQL 查询今日的数据数量"
date: 2014-04-22T10:39:00+08:00
tags: ["MySQL"] 
draft: false
toc: true
---

数据库我们存放的**时间戳**格式。

**方法一：**

先用PHP计算出时间戳。

```php
// 获取今天开始的时间戳
$today = strtotime(date('Y-m-d 00:00:00', time()));
```

这个值就是今天凌晨的时间戳。 然后查询的时间大于这个值就可以了。

**方法二：**

直接用MySQ语句查询当天的记录。

```sql
SELECT * FROM message WHERE DATE_FORMAT(FROM_UNIXTIME(chattime),'%Y-%m-%d') = DATE_FORMAT(NOW(),'%Y-%m-%d') ORDER BY id DESC
```