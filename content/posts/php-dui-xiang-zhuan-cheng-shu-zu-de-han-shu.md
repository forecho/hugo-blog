---
title: "PHP对象转成数组的函数"
date: 2013-07-16T17:47:00+08:00
categories: 
draft: false
toc: true
---

**对象和数组的区别：**

  * 数组表示有序数据的集合，而对象表示无序数据的集合。如果数据的顺序很重要，就用数组，否则就用对象。
  * 数组的数据没有"名称"（name），对象的数据有"名称"（name）。（例外：在Javascript语言中，关联数组就是对象，对象就是关联数组。）
    
    
    /**
     * 对象转数组
     * @author 佚名
     * @param object $obj
     * @return array
     */
    function object_to_array($obj){
      $_arr = is_object($obj) ? get_object_vars($obj) : $obj;
    	foreach ($_arr as $key => $val){
    		$val = (is_array($val) || is_object($val)) ? $this->object_to_array($val) : $val;
    		$arr[$key] = $val;
    	}
    	return $arr;
    }

`[is_object` ](http://www.php.net/manual/zh/function.is-object.php): 检测变量是否是一个对象。 `[get_object_vars`](http://php.net/manual/zh/function.get-object-vars.php) : 返回由对象属性组成的关联数组。 参考资料: <http://www.ruanyifeng.com/blog/2009/05/data_types_and_json.html> <https://gist.github.com/suziewong/4316491>