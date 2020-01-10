---
title: "jQuery Ajax 点击修改"
date: 2014-01-23T16:20:00+08:00
tags: ["jquery", "ajax"] 
draft: false
toc: true
---

先来一张效果图： ![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160945.png) view HTML页面代码如下：


    <td align="center" onclick="changeLike(<?php echo $id ?>,'<?php echo $like_count; ?>',this)">
        <?php echo $value['like']; ?>
    </td>

JavaScript代码如下：


    function changeLike (id, like, obj) {
        $(obj).html('<input type="num" name="" size="5" value="'+like+'">');
        $(obj).find('input').focus();
        // 键盘Enter键提交
        $(obj).find('input').keydown(function(event) {
            if(event.which==13){
                $(obj).find('input').blur();
            }
        });
        // 更新数据
        $(obj).find('input').blur(function(event) {
            $(obj).html(this.value);
            $.get('/admin/ajaxLike/'+id+'/'+this.value, function(data) {
                like = data;
            });
        });
    }

至于控制器代码就很简单了，用获取到的参数，更新记录，返回新纪录。下面是我用CakePHP写的action，请结合自身情况写代码：


    public function ajaxLike($id, $like)
    {
        $this->layout = 'ajax';
        $this->Article->id = $id;
        $this->Article->saveField('like', $like);
        exit($like);
    }