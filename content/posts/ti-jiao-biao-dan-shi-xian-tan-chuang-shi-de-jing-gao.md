---
title: "提交表单实现，弹窗试的警告"
date: 2011-11-25T14:41:00+08:00
tags: ["javascript"] 
draft: false
toc: true
---

适合使用编辑器的时候的验证。JS代码如下(第二个为UEditor的特殊判断用法)：


```javascript
<script language="javascript">
function chkinput(form)
{
    if (form.title.value=="") {
        alert("请输入新闻标题!");
        form.title.select();
        return(false);
    }

    var content=editor.hasContents()

    if (!content) {
        alert("你的输入为空");
        return(false);
    }

    return(true);
}
</script>
```


html代码如下：


```html
<form action="<?php echo site_url('chome/news_ok')?>" method="post" onSubmit="return chkinput(this)">
    <dl>
        <dt>标题：</dt>
        <dd><input type="text" name="title" /></dd>
        <dt>分类：</dt>
        <dd>
            <select name="navid">
            <?php foreach ($nav as $row){
                if ($row['nav_id']!=1 && $row['nav_type']==$nav_type) {
                    echo '<option value='.$row['nav_name'].'>'.$row['nav_name'].'</option>';
                };
            }?>
            </select>
        </dd>
        <dt>时间：</dt>
        <dd><input id="d11" type="text" onClick="WdatePicker()" name="addtime" value="<?php echo date("Y-m-d");?>" autocomplete="off"/></dd>
        <dt></dt>
        <dd>
            <script type="text/plain" id="myEditor"></script>
            <script type="text/javascript">
                var editor = new baidu.editor.ui.Editor();
                editor.render("myEditor");
            </script>
        </dd>
        <dt></dt>
        <dd class="submit"><input type="submit" name="submit" value="&nbsp;" /></dd>
    </dl>
</form>
```


这个表单里面有两个效果，一个是[My97日期控件](http://www.my97.net/dp/demo/index.htm)JS，还有一个[百度Ueditor编辑器](http://ueditor.baidu.com/teach.html)。 使用百度编辑器的时候注意editor_config.js 配置文件里面的URL路径。 注：autocomplete="off"是关闭记忆功能。