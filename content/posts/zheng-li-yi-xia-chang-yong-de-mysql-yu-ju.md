---
title: "整理一下常用的MySQL语句"
date: 2013-03-07T11:10:00+08:00
tags: ["MySQL"] 
draft: false
toc: true
---

### INNER JOIN ... ON用法 查询数据库多个表

```sql
select * from table1 inner join table2 on table1.id=table2.id
```

概括为： FROM (((表1 INNER JOIN 表2 ON 表1.字段号=表2.字段号) INNER JOIN 表3 ON 表1.字段号=表3.字段号) INNER JOIN 表4 ON Member.字段号=表4.字段号) INNER JOIN 表X ON Member.字段号=表X.字段号 

**连接两个数据表的用法：**

```sql
FROM Member INNER JOIN MemberSort ON Member.MemberSort=MemberSort.MemberSort
```

语法格式可以概括为：

FROM 表1 INNER JOIN 表2 ON 表1.字段号=表2.字段号

**连接三个数据表的用法：**

语法格式可以概括为：

FROM (表1 INNER JOIN 表2 ON 表1.字段号=表2.字段号) INNER JOIN 表3 ON 表1.字段号=表3.字段号  

### DISTINCT

使用 DISTINCT 关键字可以去掉查询中某个字段的重复记录。 用法：

```
SELECT DISTINCT(column) FROM tb_name
```

语法格式可以概括为：

SELECT DISTINCT(字段号) FROM 表  

### GROUP BY 语句

GROUP BY 语句用于结合合计函数，根据一个或多个列对结果集进行分组。 用法： 

```sql
SELECT column_name, aggregate_function(column_name) FROM table_name GROUP BY column_name
```

