---
title: "面向对象编程思想(二)"
date: 2013-02-17T15:51:00+08:00
tags: ["php"] 
draft: false
toc: true
---

![](http://m1.img.libdd.com/farm3/249/21243226FD2AFBC179CB7EAD91BA96F9_510_160.JPEG) ![](http://m1.img.libdd.com/farm3/175/8D8406554044B6F02299D3D4F66D00AF_604_179.JPEG) 示例：

```php
<?php
//声明一个笔记本的类
class NoteBook{
    //成员属性
    private $name;
    private $type;
    private $os;
    private $work;
   
    //构造函数:创建和初始化对象成员属性
    function __construct($name,$type,$os,$work){
        $this->name = $name;
        $this->type = $type;
        $this->os = $os;
        $this->work = $work;
    }
   
    function __get($proName){
        return "======>".$this->$proName."<======<br/>";
    }
   
    function __set($proName,$proValue){
        echo "======>".$this->$proName = $proValue."<======<br/>";
    }
   
    function __isset($proName){
        return isset($this->$proName);
    }
   
    function __unset($proName){
        unset($this->$proName);
    }
   
    //成员方法
    function say(){
        return "<p>应用于".$this->work." - ".
               $this->name." - ".
               $this->type." - ".
               $this->os."</p>";
    }
}
   
$macbook = new NoteBook("MacBook Pro","MC374","Mac OS X Snow Leopard","设计");
echo $macbook->say();
echo $macbook->name;
echo $macbook->type;
echo $macbook->os;
echo $macbook->work;
   
echo "<hr/>";
   
$macbook->name = "SONY";
$macbook->type = "ZT288";
$macbook->os = "Windows 7";
$macbook->work = "娱乐";
   
unset($macbook->name);
   
echo "<hr/>";
   
if(isset($macbook->name)){
    echo "成员属性name存在于NoteBook类中";
}else{
    echo "成员属性name不存在于NoteBook类中";
}
   
?>
```