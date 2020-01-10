---
title: "PHP jQuery JSON 二级联动"
date: 2013-12-12T11:18:00+08:00
categories: 
draft: false
toc: true
---

HTML代码如下： 
    
    
    <select name="category_f" id="category_f" class="valid">
        <option value="1">美食</option>
        <option value="2">休闲娱乐</option>
        <option value="3">其他</option>
    </select>
    <select name="category_s" id="category_s" class="valid">
    	<option value="11">本帮江浙菜</option>
    	<option value="12">川菜</option>
    </select>

JavaScript代码如下： 
    
    
    <script type="text/javascript">		
        $("#category_f").click(function() {
            var html = '';
        	$.ajax({
        		url: '/getcategory', //PHP方法
        		type: 'POST',
                dataType: 'json',
        		data: {f: $(this).val()},
                success:function(msg){
                    $.each(msg,function(key,val){
                        html += '<option value="' + key + '">' + val + '</option>';
                    });
                    $("#category_s").html(html);
                }
        	})
        });
    </script>

 PHP action处理页面： 
    
    
    public function getcategory()
    {
    	$json[1]  = '{"11":"本帮江浙菜","12":"川菜"}';
    	$json[2]  = '{"30":"密室","31":"咖啡厅","32":"酒吧"}';
    	$json[3] = '{"136":"其他"}';
    
    	exit($json[$_POST['f']]);
    }

上面代码已经可以用了，**但是修改页面如何去实现呢？所以我要重构上面的代码。** JavaScript代码被改成这样了(重在理解代码，我这个代码有可能在你的场景中不适用): 
    
    
    <script type="text/javascript">
        // 自动加载 用于修改时
        $(document).ready(function() {
            ajaxSelect(<?php echo ($data) ? $data['category_f'] : 1 ; ?>, <?php echo ($data) ? $data['category_s'] : 11 ; ?>);
        });
        // 手动修改 二级联动
        $("#category_f").click(function() {
            ajaxSelect($(this).val(), 11);
        });
        // 二级联动
        function ajaxSelect (argument,id) {
            var html = '';
            $.ajax({
                url: '/getcategory',  //PHP方法
                type: 'POST',
                dataType: 'json',
                data: {f: argument},
                success:function(msg){
                    $.each(msg,function(key,val){
                        if(id == key) {
                            html += '<option value="' + key + '" selected>' + val + '</option>';
                        }else{
                            html += '<option value="' + key + '">' + val + '</option>';
                        }
                    });
                    $("#category_s").html(html);
                }
            })
        }
    </script>

HTML简化后如下： 
    
    
    <select name="data[category_f]" id="category_f" class="valid">
         <option value="1" <?php if($data && $data["category_f"]==1) echo "selected"; ?>>美食</option>
         <option value="2" <?php if($data && $data["category_f"]==2) echo "selected"; ?>>休闲娱乐</option>
         <option value="3" <?php if($data && $data["category_f"]==3) echo "selected"; ?>>其他</option>
    </select>
    <select name="data[category_s]" id="category_s" class="valid"></select>

PHP action代码不变。