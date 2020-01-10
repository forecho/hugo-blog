---
title: "面向对象编程思想(一)"
date: 2013-02-17T15:48:00+08:00
tags: ["php"] 
draft: false
toc: true
---

```
<?php
类{
成员属性（申明变量）
成员方法（函数）
}

实例化类的过程，我们会产生一个对象
?>
```

例如：

```php
<?php
//声明一个笔记本的类
class NoteBook{
    //成员属性
    public $name;
    public $type;
    public $os;
    public $work;
    //构造函数:创建和初始化对象成员属性
    function __construct($name,$type,$os,$work){
        $this->name = $name;
        $this->type = $type;
        $this->os = $os;
        $this->work = $work;
    }
    //析造函数:回收内存种的垃圾变量
    function __destruct(){
        echo "销毁内存种使用过的垃圾变量".$this->name."<br/>";
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
$macbook = null;

$sony = new NoteBook("SONY","ZT288","Windows 7","娱乐");
echo $sony->say();
$sony = null;

$samsung = new NoteBook("SAMSUNG","B002","Ubuntu Linux 10.04","编程");
echo $samsung->say();

?>
```