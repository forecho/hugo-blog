---
title: "PHP MySQL 表非主键自增长"
date: 2014-11-13T20:34:00+08:00
tags: ["MySQL"] 
---
##场景
一个活动表，一个活动记录表，活动记录表要根据活动 ID，记录每个人的活动分数和时间等用户参与信息，其中活动记录表有一个字段是用户的编号，每个活动的记录都要从1开始自增。现在的问题是怎么实现这个自增的效果？


## 表结构如下：
主要说一下活动记录表 activity_records 的 主要字段结构：

| 字段               | 注释
| :-----------------|-------------:
| id                |  主键自增
| activity_id       | 活动ID
| user_id           | 用户 ID
| number            |活动记录编号


##解决方案
<!--more-->

### 方案一（失败）
开始我用的是最方便的方法：使用 MySQL 的 count 一下活动记录表某个活动的数量，再+1就等于这个活动活动记录的编号。但是这个方案在并发的时候很容易出现编号重复的 BUG，显然这不是我们想要的。
因为要执行两条分开的 MySQL 语句（下面的语句只是提供大概思路，具体代码要根据你的项目使用的框架写法不一样）：

```php
$count = mysql_query("SELECT COUNT(*) FROM activity_records WHERE `activity_id` = activityId");
mysql_query("INSERT INTO activity_records (number, user_id, activity_id) VALUES ($count+1, userId, activityId)");
```

因为 MySQL 是单进程多线程架构的数据库。

后来有朋友介绍用 MySQL 的**联合唯一索引**的方法把 number 字段和 activityId 字段联合一下。执行语句就可以了：

```sql
ALTER TABLE activity_records add unique index(number, activityId);
```

结果就是确实能阻止 number 字段的重复出现，但是这种方法是以提示插入数据库不成功并且抛出错误信息为方式的，显然是不是我们想要的。

### 方案二（成功）
使用 MySQL 的自定义函数功能，使用 MySQL 的存储过程。

**（一）配置 MySQL** 允许function的同步：

```sql
SET GLOBAL log_bin_trust_function_creators = 1;
```

**（二）写自定义函数**（这里我写了俩个，其实写成一个也可以）：

```sql
-- ----------------------------
-- Function structure for `currval`
-- ----------------------------
DROP FUNCTION IF EXISTS `currval`;
DELIMITER ;;
CREATE DEFINER=`root`@`%` FUNCTION `currval`(`value` int(11), `userId` int(11), `activityId` int(11)) RETURNS int(11)
BEGIN
  INSERT INTO activity_records (number, user_id, activity_id)
  VALUES(value+1, userId, activityId);
  RETURN value+1;
END
;;
DELIMITER ;

-- ----------------------------
-- Function structure for `nextval`
-- -------------------------draft: false
toc: true
---
DROP FUNCTION IF EXISTS `nextval`;
DELIMITER ;;
CREATE DEFINER=`root`@`%` FUNCTION `nextval`(`userId` int(11), `activityId` int(11)) RETURNS int(11)
BEGIN
  DECLARE value INTEGER;

  SET value = 0;

  SELECT COUNT(*) INTO value

  FROM activity_records WHERE `activity_id` = activityId;

  RETURN currval(value, userId, activityId);
END
;;
DELIMITER ;
```

**（三）使用自定义函数**

```php
mysql_query("SELECT nextval($userId, $activityId)");
```


**（四）值得注意的地方**

1. 自定义函数过程中变量不要和表字段名一样。
2. 其中的 root 为项目中的使用数据库的连接用户。