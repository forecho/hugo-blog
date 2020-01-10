---
title: "整理一下我经历的PHP面试题"
date: 2013-10-08T23:20:00+08:00
categories: 
draft: false
toc: true
---

1、用css、html编写一个两列布局的网页，右侧固定宽度200px，左侧自适应。（答案如下，IE下没测试，Chrome下正常） 
    
    
    <!DOCTYPE html>
    <html>
    <head>
    	<meta http-equiv="content-type" content="text/html; charset=utf-8" />
    	<title></title>
    </head>
    <style type="text/css">
    div{
    	height: 200px;
    }
    #left{
    	float: left;
    	width: 100%;
    	margin-left: -210px;
    }
    #content{
    	margin-left: 210px;
    	border: 1px solid #ccc;
    }
    #right{
    	float: right;
    	width: 200px;
    	border: 1px solid #ccc;
    }
    
    </style>
    <body>
    <div id="left">
    	<div id="content">
    		我在左边 可以自动适应
    	</div>
    </div>
    
    <div id="right">
    	我在右边 固定宽度200px
    </div>
    
    </body>
    </html>

2、简单描述mysql中，索引，主键，唯一索引，联合索引的区别，对数据库的性能有什么影响（从读写两方面） 

> 索引是一种特殊的文件(InnoDB数据表上的索引是表空间的一个组成部分)，它们包含着对数据表里所有记录的引用指针。 普通索引(由关键字KEY或INDEX定义的索引)的唯一任务是加快对数据的访问速度。普通索引允许被索引的数据列包含重复的值。如果能确定某个数据列将只包含彼此各不相同的值，在为这个数据列创建索引的时候就应该用关键字UNIQUE把它定义为一个唯一索引。也就是说，唯一索引可以保证数据记录的唯一性。 主键，是一种特殊的唯一索引，在一张表中只能定义一个主键索引，主键用于唯一标识一条记录，使用关键字PRIMARYKEY来创建。 索引可以覆盖多个数据列，如像INDEX(columnA,columnB)索引，这就是联合索引。索引可以极大的提高数据的查询速度，但是会降低插入、删除、更新表的速度，因为在执行这些写操作时，还要操作索引文件。

3、JS 表单弹出对话框函数是 ?获得输入焦点函数是 ? 

> 弹出对话框函数:alert(), prompt(), confirm() 获得输入焦点函数: focus()

4、JS 的转向函数是?怎么引入一个外部 JS 文件? 

> 转向使用 window.location.href="" 引入外部 js 使用 <script src=""/>

5、include 和 require 都能把另外一个文件包含到当前文件中 ,他们有什么区别? Include 和 include_once 又有什么区别? 

> 二者区别只有一个,那就是对包含文件的需求程度, include就是包含,如果被包含的 文件不存在的话,那么则会提示一个错误,但是程序会继续执行下去。而 require 意思是需要,如果被包含文件不存在或者无法打开的时候,则会提示错误, 并且会终止程序的执行。这两种结构除了在如何处理失败之外完全一样 。once 的意思是一次,那么 include_once 和 require_once 表示只包含一次,避免重复包含。

6、echo(),print(),print_r()的区别? 

> echo 是一个语言结构,输出一个或多个字符串; print() 实际上不是一个函数(它是一个语言结构),因此你可以不必使用圆括号来括起 它的参数列表 ,它输出一个字符串 ; print_r () 是一个函数,打印变量的信息,基本类型,数组,对象。

7、用 PHP 打印出前一天的时间格式是 2006-5-10 22:21:21。 
    
    
    <?php echo date("Y-m-d H:i:s", time()-3600*24); ?>
    <?php echo date("Y-m-d H:i:s", strtotime("-1 days")); ?>

8、求两个日期的差数,例如 2009-3-1 ~ 2009-4-4 的日期差数？ 
    
    
    <?php echo (strtotime("2009-4-4")-strtotime("2009-3-1"))/3600*24;?>

9、用 PHP 写出显示客户端 IP 与服务器 IP 的代码。 

> 客户端 IP:$_SERVER["REMOTE_ADDR"] 服务器端 IP:$_SERVER["SERVER_ADDR"]

10、sql 语句应该考虑哪些安全性? 

> 防止 Sql 注入,对特殊字符进行转义、过滤或者使用预编译的 sql 语句绑定变量。 最小权限原则,特别是不要用 root 账户,为不同的类型的动作或者组建使用不同的账户。当 sql 运行出错时,不要把数据库返回的错误信息全部显示给用户 ,以防止泄露服务器和数据库相关信息。

11、MYSQL 取得当前时间的函数是?,格式化日期的函数是? 

> current_time() 用于取得当前时间 date_format(datetime, format)用于格式化日期,如:select date_format(now(),'%Y%m%d');

12、请简述项目中优化 sql 语句执行效率的方法,从哪些方面,sql 语句性能如何分析? 

> 1) 尽量选择较小的列 2) 将where中用的比较频繁的字段建立索引 3) select子句中避免使用‘*’ 4) 避免在索引列上使用计算 、not in 和<>等操作 5) 当只需要一行数据的时候使用 limit 1 6) 保证单表数据不超过 200W,适时分割表。 针对查询较慢的语句,可以使用explain 来分析该语句具体的执行情况。

13、简述在 MySQL 数据库中 MyISAM 和 InnoDB 的区别？ 

> 区别主要有以下几个: 构成上,MyISAM 的表在磁盘中有三个文件组成,分别是表定义文件( .frm)、数据文 件(.MYD)、索引文件(.MYI),而 InnoDB 的表由表定义文件(.frm)、表空间数据和日志文 件组成。 安全方面, MyISAM 强调的是性能,其 查询 效率较高,但不支持事务和外键等安全性 方面的功能,而 InnoDB 支持事务和外键等高级功能 ,查询效率稍低。 对锁的支持,MyISAM 支持表锁,而 InnoDB 支持行锁。