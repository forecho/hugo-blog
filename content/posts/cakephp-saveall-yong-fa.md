---
title: "CakePHP saveAll用法"
date: 2013-12-26T21:17:00+08:00
categories: 
draft: false
toc: true
---

视图表单代码如下： 
    
    
    <tr id="research-items">
    	<td>
    		<select name="type[]">
    			<option value="text">单行文字</option>
    			<option value="textarea">多行文字</option>
    			<option value="radio">单项选择</option>
    			<option value="checkbox">多项选择</option>
    		</select>
    	</td>
    	<td><input type="text" name="question[]" value="" placeholder="这里是问题"></td>
    	<td><input type="text" name="items[]" value="" placeholder="答案1|答案2"></td>
    	<td>
    		<input type="checkbox" name="is_answer[]" value="1" <?php echo (isset($data) && $data['ResearchSetting']['is_answer']==1) ? "checked" : '' ; ?>>是否必填
    	</td>
    	<td><a class="btn_delete" title="删除" href="javascript:;">删除</a></td>
    </tr>

控制器的写法如下： 
    
    
    for ($i=0; $i < count($_POST['type']) ; $i++) {
         $post[$i]['ResearchOption']['type']                = $_POST['type'][$i];
         $post[$i]['ResearchOption']['question']            = $_POST['question'][$i];
         $post[$i]['ResearchOption']['items']               = $_POST['items'][$i];
         $post[$i]['ResearchOption']['is_answer']           = ($_POST['is_answer'][$i])?$_POST['is_answer'][$i]:0;
         $post[$i]['ResearchOption']['research_setting_id'] = $this->ResearchSetting->id;
    }
    $this->ResearchOption->saveAll($post);

参考链接：<http://book.cakephp.org/2.0/en/models/saving-your-data.html>