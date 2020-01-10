---
title: "CI框架学习之上传文件"
date: 2011-11-26T15:57:00+08:00
tags: ["php"] 
draft: false
toc: true
---

视图代码：

```php
<?php echo form_open_multipart('chome/do_upload');?>

<dl>
    <dt>文件名称：</dt>
    <dd>
        <input type="text" name="filename">
    </dd>
    <dt>选择文件：</dt>
    <dd>
        <input type="file" name="userfile">
    </dd>
    <dt></dt>
    <dd>
        <input type="submit" value="上传">
    </dd>
</dl>
```

CI模型中的代码如下： 之前要加载两个CI自带的类：


```php
$this->load->model('mhome');
$this->load->helper(array('form', 'url'));
```

下面是方法：

```php
function do_upload(){
	$config['upload_path'] = './uploads/';//绝对路径
	$config['allowed_types'] = 'txt|php|cdr|gif|jpg|png';//文件支持类型
	$config['max_size'] = '0';
	$config['encrypt_name'] = true;//重命名文件
	$this->load->library('upload',$config);

	if ($this->upload->do_upload()) {
		$upload_data = $this->upload->data();
		$query = 1;
		//调用模型，写入数据库
		$this->mhome->upload($upload_data['file_name']);
	}
	else {
		$this->upload->display_errors();
		$query = 0;
	}
	//提示
	$data['succ'] = $query;
	$data['su1'] = "提交成功";
	$data['su0'] = "文件上传失败,请检查文件再重新上传";
	$this->load->view('admin/success', $data);
}
```

**值得注意**：有图片上传的时候表单必须用form_open_multipart；还有用于上传的input:file的name如果不是userfile,则必须改成 $this->upload->do_upload('你自己命名的name')