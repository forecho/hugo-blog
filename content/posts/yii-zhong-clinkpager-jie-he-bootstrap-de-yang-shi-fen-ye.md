---
title: "Yii中CLinkPager结合Bootstrap的样式分页"
date: 2013-08-09T10:38:00+08:00
categories: 
draft: false
toc: true
---

$this->widget('zii.widgets.CListView', array(
        'dataProvider'=>$model->search(),
        'itemView'=>'_post', 
        'emptyText'=>'暂时没有数据',  
        'template'=>'{items}{pager}',
        'pager' => array(
                'header'=>false,
                'htmlOptions'=>array('class'=>'pagination'),
                'selectedPageCssClass' => 'active',        
                'hiddenPageCssClass' => 'disabled',
                // 'cssFile'=>false,
                // 'maxButtonCount'=>25,
                // 'selectedPageCssClass'=>'active',
                // 'hiddenPageCssClass'=>'disabled',
                // 'firstPageCssClass'=>'previous',
                // 'lastPageCssClass'=>'next',
                // 'firstPageLabel'=>'<<',
                // 'lastPageLabel'=>'>>',
                // 'prevPageLabel'=>'<',
                // 'nextPageLabel'=>'>',
            ),
        'htmlOptions'=>array('class'=>'list-group'),
        'itemsTagName'=>'ol',
        'itemsCssClass'=>'box-cell',
        'pagerCssClass'=>'',
    ));
    ?>

yii默认分页默认第一页是不显示首页和前一页的，加上上面的两行加亮代码就能解决这个问题。这样首页和上一页会总是显示。