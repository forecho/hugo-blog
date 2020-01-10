---
title: "Yii CListView使用方法"
date: 2013-05-22T10:42:00+08:00
categories: 
draft: false
toc: true
---

<?php   
        $this->widget('zii.widgets.CListView', array(  
            'dataProvider'=>$model->search(20),  
            'itemView'=>'_view',    
            'emptyText'=>'暂时没有数据',  
           //'template'=>'{items}{pager}',
            'pager'=>array('class'=>'CLinksPager'),
            'htmlOptions'=>array('class'=>'list-view pro_list bd'),
            'itemsTagName'=>'ul',
            'itemsOptions'=>array('class'=>'list c_f'),
            'summaryText'=>'第 {start}-{end} 条, 共<strong class="c_red2">{count}</strong> 条   第{page}页/共{pages}页',
            'pagerTemplate'=>'onlyPager',
            'buttonCssClass'=>'bg_x btn_page',
            'batchItemOptions'=>array('class'=>"f_l batchItem"),
            'batchItem'=>array(  
                //移动到分类  
                '<span class="c_666">排序：</span>',  
                //添加到分类  
                CHtml::link('默认排序',array('list','id' => $model->catgory_id,'att' =>$g_att,'sort'=>''),array('class'=>empty($model->list_order)?"default price":"default")),
                CHtml::link(($model->list_order==4)?'价格<b class="ico ico_up"></b>':'价格<b class="ico ico_down"></b>',array('list','id' => $model->catgory_id,'att' =>$g_att,'sort'=>($model->list_order==4)?6:4),array('class'=>($model->list_order==4||$model->list_order==6)?"bg_x price":"bg_x")),  
                CHtml::link(($model->list_order==5)?'销量<b class="ico ico_up"></b>':'销量<b class="ico ico_down"></b>',array('list','id' => $model->catgory_id,'att' =>$g_att,'sort'=>($model->list_order==2)?5:2),array('class'=>($model->list_order==2||$model->list_order==5)?"bg_x price":"bg_x")),
                CHtml::link(($model->list_order==7)?'上架时间<b class="ico ico_up"></b>':'上架时间<b class="ico ico_down"></b>',array('list','id' => $model->catgory_id,'att' =>$g_att,'sort'=>($model->list_order==1)?7:1),array('class'=>($model->list_order==1||$model->list_order==7)?"bg_x price":"bg_x")),
             ),  
            ));  
    ?>

简单说明： template是整个CListView的模板；