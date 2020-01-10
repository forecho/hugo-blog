---
title: "CakePHP belongsTo不是主键关联"
date: 2014-01-02T17:06:00+08:00
tags: ["cakephp"] 
draft: false
toc: true
---

belongsTo是`属于`的关系。


    var $belongsTo = array(

        'Inventory' => array(
            'className'    => 'Inventory',
            'foreignKey' => false,
            'conditions' => array(' `RentalLineitem`.`i_num` = `Inventory`.`i_num`'),
            'dependent'    => false //是否级联删除
        )

    );

参考文章：<http://stackoverflow.com/questions/6267804/cakephp-mapping-belongsto-association-to-a-non-primary-key>