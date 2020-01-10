---
title: "Yii 表添加字段后数据更新不成功"
date: 2013-07-28T22:48:00+08:00
categories: 
draft: false
toc: true
---

情况是这样的，我开始已经用Gii生成了Model文件了，后来又手动添加一个字段，这个字段是info，TEXT类型，不是必填项。 刚开始我是直接在已经生成的Model文件里的rules最后一行代码添加info的，添加之后的代码如下： 
    
    
    public function rules()
    {
    	// NOTE: you should only define rules for those attributes that
    	// will receive user inputs.
    	return array(
    		array('email', 'required'),
    		array('email', 'length', 'max'=>255),
    		array('name', 'length', 'max'=>25),
    		array('password', 'length', 'max'=>100),
    		// The following rule is used by search().
    		// Please remove those attributes that should not be searched.
    		array('id, email, name, password, info', 'safe', 'on'=>'search'),
    	);
    }

后来调试了半天，数据就是添加不进去数据库，在Controller的actionUpade时打印数据$_POST['User']，能获取到更新后的数据，但是就是添加不了数据库。 最后Google和调试了几次，发现可能是我的info字段没有设置验证规则，虽然不是必填项。后来就试着添加了下面这行代码： 
    
    
    array('info','type','type'=>'string'),

感觉上面这行代码挺别扭的，但是最后结果是成功更新数据到数据库了，事实证明就是这个字段的验证有问题。 后来我又发现了这个rules的safe貌似只是针对search场景的，而我需要的更新数据。 于是代码改成下面这样的了： 
    
    
    public function rules()
    {
    	// NOTE: you should only define rules for those attributes that
    	// will receive user inputs.
    	return array(
    		array('email', 'required'),
    		array('email', 'length', 'max'=>255),
    		array('name', 'length', 'max'=>25),
    		array('password', 'length', 'max'=>100),
    		array('info', 'safe'),
    		// The following rule is used by search().
    		// Please remove those attributes that should not be searched.
    		array('id, email, name, password', 'safe', 'on'=>'search'),
    	);
    }

测试一下，果然成功了。 最后为了进一步确认一下，我去Gii又生成一个这个表的Model，只是预览一下代码，不生成。对比rules，果然就是改这里。早知道直接用这个方法了。 **最后总结一下**，yii的表每个字段在rules都要有个验证规则，如果没有验证规则，那就必须要写一个safe。