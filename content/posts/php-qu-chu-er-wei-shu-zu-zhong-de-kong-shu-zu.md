---
title: "PHP去除二维数组中的空数组"
date: 2013-07-03T17:10:00+08:00
categories: 
draft: false
toc: true
---

**关键是[array_filter](http://www.w3school.com.cn/php/func_array_filter.asp)这个函数；例子：** 原数组： 
    
    
    Array
    (
        [0] => Array
            (
                [ExchangeNo] => ECR000033
                [CardNo] => Array
                    (
                    )
    
                [QTY] => 1
                [ExchangeDate] => 2013-06-13T18:55:23.84
                [Remark] => Array
                    (
                    )
    
            )
    
        [1] => Array
            (
                [ExchangeNo] => ECR000036
                [CardNo] => Array
                    (
                    )
    
                [QTY] => 20
                [ExchangeDate] => 2013-06-20T18:24:32.53
                [Remark] => Array
                    (
                    )
    
            )
    
        [2] => Array
            (
                [ExchangeNo] => ECR000037
                [CardNo] => Array
                    (
                    )
    
                [QTY] => 20
                [ExchangeDate] => 2013-06-20T18:28:23.14
                [Remark] => Array
                    (
                    )
    
            )
    
        [3] => Array
            (
                [ExchangeNo] => ECR000038
                [CardNo] => Array
                    (
                    )
    
                [QTY] => 20
                [ExchangeDate] => 2013-06-20T18:30:53.71
                [Remark] => Array
                    (
                    )
    
            )
    
    )

去除空数组： 
    
    
    foreach ($array as $key=>$value){
    	 $array[$key] = array_filter($value);
    }

输出结果： 
    
    
    Array
    (
        [0] => Array
            (
                [ExchangeNo] => ECR000033
                [QTY] => 1
                [ExchangeDate] => 2013-06-13T18:55:23.84
            )
    
        [1] => Array
            (
                [ExchangeNo] => ECR000036
                [QTY] => 20
                [ExchangeDate] => 2013-06-20T18:24:32.53
            )
    
        [2] => Array
            (
                [ExchangeNo] => ECR000037
                [QTY] => 20
                [ExchangeDate] => 2013-06-20T18:28:23.14
            )
    
        [3] => Array
            (
                [ExchangeNo] => ECR000038
                [QTY] => 20
                [ExchangeDate] => 2013-06-20T18:30:53.71
            )
    )