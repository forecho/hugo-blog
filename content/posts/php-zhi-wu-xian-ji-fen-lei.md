---
title: "PHP之无限极分类"
date: 2012-10-16T14:12:00+08:00
categories: 
draft: false
toc: true
---

用到今天发现一个严重的错误，在id超过两位数的时候，排序是有问题的。如下图错误的排序： ![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160845.png) 你会发现如果按照as出来的bpath排序的话，cid为55的排序错误，正确的情况是cid为55的应该在101前面，出错的原因是bpath是一个字符串，它会安装字符串来排序，以 “-”  为分节符第一位是0一位数，55的第二位为55两位数，101的第三位数为101三位数。位数不同导致排序不正确的BUG，解决办法就是让cid的位数固定下来，不够的前面填充0，这里我们就用到了MySQL的[ZEROFILL](http://www.2cto.com/database/201206/135566.html)。

> # Zerofill
> 
> 用于数字类型的定长显示是最适合不过了， 长度不够时，用0填充。

那么我们在MySQL里面执行下面的命令，改变表结构： 
    
    
    ALTER TABLE `category` CHANGE COLUMN `id` `id` INT(9) UNSIGNED ZEROFILL NOT NULL AUTO_INCREMENT

现在你再执行查询，结果如下： ![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160901.png) 现在的补救方法要么就是把子类删除掉，要么就是手动添加path的0保证位数相同。（不要太在意上图的cid，其实就是等同于本文的id。只是换了个名字而已。） ============以上信息update 于2014年1月10日============ 目前写无限极分类的大致分为三种情况： 

  1. 递归（效率太慢）
  2. ajax（不会的话，就写不出来）
  3. 继承关系（方法简单，实用）
数据库结构如下图： ![](http://m2.img.libdd.com/farm5/2012/0913/15/2A76C895BDF8890113334C98FDE5BAEFC40F2905049E_491_205.PNG)   主要思想是通过path这个字段通过MySQL 的concat 函数查询实现。示例代码如下： 
    
    
    SELECT id, path, name, CONCAT( path,  '-', id ) AS bpath FROM  `category` ORDER BY bpath

bpath:虚拟字段 内容字段如下： ![](http://m1.img.libdd.com/farm4/2012/0913/15/8AA38893F111837AA9473C3B36B86B939CBBE405049E_495_201.PNG)   视图主要代码如下： 
    
    
    <select name="id" class="small-input">
        <option value="0">根目录</option>
        <?php foreach($category as $row):?>
        <option value="<?php echo $row->id;?>">
            <?php
              $count = count(explode('-',$row->bpath));
              echo '|';
              for($i=1;$i<$count;$i++){
                   echo '—';
               }   
              echo $row->name;?>
         </option>
        <?php endforeach;?>
    </select>

  添加新栏目的话，主要思想是的把父级的path 和pid链接起来，组成新的path。 详细代码请参考我的[gitbub](https://github.com/forecho) 的Fecms项目，敬请期待。