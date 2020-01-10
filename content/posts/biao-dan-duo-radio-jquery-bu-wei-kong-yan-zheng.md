---
title: "表单多radio jQuery 不为空验证"
date: 2014-01-24T14:02:00+08:00
tags: ["jquery"] 
draft: false
toc: true
---

一、如果表单是 POST 或者 GET 提交的话，可以直接这样写 jQuery：


    $("form").submit(function() {
        var submitme = true;
        $(':radio').each(function() { // loop through each radio button
            nam = $(this).attr('name'); // get the name of its set
            if (submitme && !$(':radio[name="'+nam+'"]:checked').length) {
            // see if any button in the set is checked
                alert(nam+' group not checked');
                submitme = false;
            }
        });
        return submitme; // cancel the form submit
    });

二、如果表单用的 Ajax 提交的话，我们还需要修改一下：

1. 把提交按钮 type 由 submit 改成 button 类型，然后加一个 id="submit "。

2. jQuery 代码如下：


    $("#button").click(function(){
        var submitme = true;
        $(':radio').each(function() {
            nam = $(this).attr('name');
            if (submitme && !$(':radio[name="'+nam+'"]:checked').length) {
                alert(nam+' group not checked');
                submitme = false;
            }
        });

        if (submitme) {
            // 防止重复提交
            $(this).attr({
                disabled: 'disabled',
                value: '正在提交...'
            });
            $.post("/admin/saveUserData",$("#user-form").serialize(),function(data){
                #do something
                ....
            });
        };
    })

参考文章：<http://stackoverflow.com/questions/10706925/jquery-ensuring-all-radio-groups-are-checked>