---
title: "实现checkbox全选和PHP删除多个数据"
date: 2011-11-24T16:31:00+08:00
tags: ["php"] 
draft: false
toc: true
---

首先定义表单中checkbox的name为一个数组range[]

```html
<input type="checkbox" name="range[]" value="你设定的值" />
```

示例：

```html
<input type="checkbox" name="range[]" value="<?php echo $row['id'];?>"/>
```

下面是“全选”、“反选”、“删除”。

```html
<input type="button" value="全选" id="selectAll">

<input type="button" value="反选" id="unSelect">

<input type="submit" value="删除" onclick="return queren()" />
```

JS代码如下：

```javascript
<script type="text/javascript">
function $(id){
    return document.getElementById(id);
}
window.onload=function(){
    var selectAll = $("selectAll"),
    unSelect = $("unSelect"),
    inputs=document.getElementsByName('range[]'),
    len = inputs.length;
    selectAll.onclick=function(){
        for(var i=0; i<len;i++){
            inputs[i].checked=true;
        }
    }
    unSelect.onclick=function(){
        for(var i=0; i<len;i++){
            var o = inputs[i];
            o.checked?o.checked=false:o.checked=true;
        }
    }
}
</script>

<script type="text/javascript">
function queren(){
    var info=confirm('确认删除么？');
    if(info==true){
        return true;
    }
    return false;
}
</script>
```

JS代码如下：

```javascript
<script type="text/javascript">
function $(id){
return document.getElementById(id);
}
window.onload=function(){
	var pan;
	var conf;
	var selectAll = $("selectAll"),
	unSelect = $("unSelect"),
	del = $("del"),
	inputs=document.getElementsByName('range[]'),
	len = inputs.length;
	selectAll.onclick=function(){
		for(var i=0; i<len;i++){
			inputs[i].checked=true;
		}
	}
	unSelect.onclick=function(){
		for(var i=0; i<len;i++){
			var o = inputs[i];
			o.checked?o.checked=false:o.checked=true;
		}
	}

	$("form1").onsubmit=function(){
		for(var i=0; i<len;i++){
			var o = inputs[i];
			if(o.checked){
				pan=1;
				break;
			}else{
				pan=0;
			}
		}

		if(!pan){
			alert("请选择");
			return false;
		}else{
			conf=confirm("确定删除");
		}

		if(conf){
		  return true;
		}else{
			return false;
		}
	}
}
</script>
```

删除按钮：

```html
<input type="submit" value="删除" id="del" />
```

form表单id="form1" 并且还要调用一个jquery。