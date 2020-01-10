---
title: "PHP一次写入上万条数据到数据库"
date: 2013-12-05T16:36:00+08:00
categories: 
draft: false
toc: true
---

场景：商家生成1W条以上的优惠券，随机生成的优惠券写入数据库，也就是1W多条。 按平常的写法我们肯定是这样写的： 
    
    
    for ($i=0; $i <10000 ; $i++) { 
    	SQL语句
    	执行写入数据库操作
    }

但是这样效率太低，如果是1W条数据库就需要操作1W次数据库，这个成本是很大的，一般情况下，这样就是报错的，说需要执行的时间太长了。 解决办法就是，把上面代码改成如下： 
    
    
    for ($i=0; $i <10000 ; $i++) { 
    	SQL语句
    }
    执行写入数据库操作

这样只需要执行一次写入数据库操作就行了，效率立刻体现出来了。 CakePHP自带有一个方法是saveAll就是用来做这件事的，使用方法如下： 
    
    
    for ($i=0; $i <10000 ; $i++) { 
    	$data[] = array('字段名' => '值'),array('字段名' => '值');
    }
    $this->ModelData->saveAll($data);

  参考链接：[PHP技巧：写入大量数据到mysql](http://www.tiandiyoyo.com/2013/08/php-insert-large-records-to-mysql/)