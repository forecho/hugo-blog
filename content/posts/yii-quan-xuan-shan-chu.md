---
title: "Yii全选删除"
date: 2013-05-30T20:10:00+08:00
tags: ["yii1"] 
draft: false
toc: true
---

目前功能还是能用，但是删除不是Ajax操作的。等待下次完善。 **Views：**

```php
<form action="/member/msglog/deleteordmsg" method="post" id="deleteForm">
	<?php
	    $this->widget('zii.widgets.CListView', array(
	        'dataProvider'=>$dataProvider,
	        'itemView'=>'_view',
	        'emptyText'=>'暂时没有数据',
	       	'template'=>'{items}{pager}',
	        'pager'=>array('class'=>'CLinksPager'),
	        'itemsTagName'=>'table',
	        'itemsOptions'=>array('class'=>'list_table', 'width'=>'100%'),
	        'buttonCssClass'=>'bg btn_page',
	        'batchItemOptions'=>array('class'=>"filter f_l"),
	        'batchItem'=>array(
	            //删除
	            '<input id="allcheckbox1" onchange="checkAll(\'select-on-check\',\'allcheckbox1\');" class="chk select-on-check" type="checkbox" name="">全选
                 <button class="mem_bgx link_btn mr5" onclick="$(\'#deleteForm\').submit();" title="">删除选中</button>'			        ),
	    ));
	?>
</form>
```

**_view.php文件：**

```html
<tr>
	<td width="6%"><input class="select-on-check chk f_l" type="checkbox" name="ids[]" value="<?php echo $data->id; ?>" /></td>
    <td width="15%"><?php echo $data->fromMember->name;?></td>
    <td width="58%"><p class="pub_tit02"><?php echo CHtml::link($data->title, array('/member/msgLog/inboxDetail', 'id'=>$data->id));?></p></td>
    <td><?php echo date("Y-m-d H:i:m", $data->inputtime);?></td>
</tr>
```

*jQuery验证是否有选择：**

```javascript
<script type="text/javascript">
//表单验证
$('#deleteForm').bind('submit',function(){
	var data=new Array();
	$("input:checkbox[name='ids[]']").each(function ()
    	{
		if($(this).attr("checked")=='checked')
		{
			data.push($(this).val());
		}
	});
	if(data.length > 0)
	{
	return true;
    }else{
        return false;
    }
})
</script>
```

**jQuery全选：**

```javascript
/*全选
classs:被选框class
selfid：全选框id
*/
function checkAll(classs,selfid){
	$('.'+classs).attr('checked',$('#'+selfid).attr('checked')?'checked':false);
}
```

**Controller：**

```php
//删除选中
public function actionDeleteordmsg()
{
	if (Yii::app()->request->isPostRequest)
	{
		$criteria= new CDbCriteria;
		$criteria->addInCondition('id', $_POST['ids']);
		MsgLog::model()->deleteAll($criteria);

		if(isset(Yii::app()->request->isAjaxRequest)) {
			$message = '删除成功。';
			$path = '../index/success';
		} else{
			$message = '删除失败。';
			$path = '../index/error';
		}
	}
	$this->render($path,array(
			'message'=>$message,
		));
}
```