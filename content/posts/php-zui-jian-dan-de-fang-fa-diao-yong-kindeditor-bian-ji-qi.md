---
title: "PHP最简单的方法调用kindeditor编辑器"
date: 2012-10-16T13:46:00+08:00
categories: 
draft: false
toc: true
---

先在[这里](http://pan.baidu.com/share/link?shareid=65430&uk=2684558169)直接下载编辑器代码。 插入javascript代码如下， 
    
    
    <script charset="utf-8" src="kindeditor/kindeditor.js"></script>
    <script charset="utf-8" src="kindeditor/lang/zh_CN.js"></script>
    <script>
            var editor;
            KindEditor.ready(function(K) {
                    //editor = K.create('textarea[name="content"]');
                    editor = K.create('#editor_id', {
                        allowFileManager : true  //开启浏览服务器文件功能，比方上传图片的图片空间
                    });
            });
    </script>

form表单代码如下： 
    
    
    <textarea id="editor_id" name="content" style="width:90%;height:450px;"></textarea>

**Kindeditor内容区默认字体和字体的大小如何设置呢?** 打开themes/default/default.css文件找到：`.ke-dialog-loading-content` 把`font-size: 14px;`改成`font-size: 12px;`保存即可。