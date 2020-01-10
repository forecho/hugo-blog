---
title: "jQuery 二级联动"
date: 2011-10-14T13:44:00+08:00
tags: ["jQuery"] 
draft: false
toc: true
---

公司JS做的一个效果，二级联动 获取第二个value的值。 下面是js代码。

```javascript
<script type="text/javascript" src="js/jquery-1.6.2.min.js">// <![CDATA[
<script type="text/javascript">
var currentShowCity=0;
$(document).ready(function(){
   $("#province").change(function(){
       $("#content").text("");
	   $("#province option").each(function(i,o){
		   if($(this).attr("selected")){
			   $(".city").hide();
			   $(".city").eq(i).show();
			   currentShowCity=i;
			   $(".city:eq("+i+")").change(function(){
			     $(".city:eq("+i+") option").each(function(i,o){
				    if($(this).attr("selected")){
					  $("#content").text($(this).text());
                       $("#value").val($(this).val());
					}
				 })
			   })
		   }
	   });
   });
   $("#province").change();
});
function getSelectValue(){
	alert("1级="+$("#province").val());
	$(".city").each(function(i,o){
		 if(i == currentShowCity){
			alert("2级="+$(".city").eq(i).val());
		 }
    });
}
// ]]></script>
```

下面是html代码。

```html
<select id="province">
	<option selected="selected">----请选择省份----</option>
	<option>北京</option>
	<option>上海</option>
	<option>江苏</option>
</select>
<select class="city" style="display: inline-block;">
	<option>----请选择----</option>
</select>
<select class="city" style="display: none;">
	<option>----请选择----</option>
	<option value="20">预定流程</option>
	<option value="21">预定演示</option>
	<option value="22">服务时间</option>
	<option value="23">注意事项</option>
</select>
<select class="city" style="display: none;">
	<option>----请选择----</option>
	<option value="24">会员章程</option>
	<option value="25">入会资格</option>
	<option value="26">会员优惠</option>
	<option value="27">会员申请</option>
</select>
<select class="city" style="display: none;">
	<option>----请选择----</option>
	<option value="28">儿童座椅</option>
	<option value="29">GPS</option>
	<option value="30">送车上门</option>
	<option value="31">异店还车</option>
</select>
<input id="value" type="text" name="" value="" /> <input onclick="getSelectValue();" type="button" value="点我" />
```

效果演示代码请猛击 [这里~~~~](http://www.nowhisky.com/demo/2liandong.html)