---
title: "CI使用PHP-ExcelReader类"
date: 2012-11-27T16:37:00+08:00
categories: 
draft: false
toc: true
---

关于PHP怎么使用`PHP-ExcelReader`，请参考[这篇文章~](http://haimingyoung.iteye.com/blog/1442544)，这个demo，你可以在这[下载~](http://pan.baidu.com/share/link?shareid=134586&uk=2684558169)。修改一下数据库资料就能使用。 **下面转入正题，CI如何使用PHP-ExcelReader？** 先把下载好的PHP-ExcelReader文件中的`oleread.php`和`reader.php`文件拷贝出来（reader.php文件可能需要把第 261行 “`=&`”  改为 “`=`” ）放在`phpexcelreader`文件夹（这个文件夹可以放在项目更目录下），然后我们最好写一个函数文件命名为：`my_excel_helper.php`，放在`application/helpers`文件夹里面，代码如下： 
    
    
    <?php  if ( ! defined('BASEPATH')) exit('No direct script access allowed');
    
    function excel()
    {
    	$CI = &get_instance();
    	if(!isset($CI->excel))
    	{
    		require_once '/resources/phpexcelreader/reader.php';//此处是reader.php文件的相对路径，根据项目自行修改
    		$CI->excel = new Spreadsheet_Excel_Reader();
    		$CI->excel->setOutputEncoding('utf-8');
    	}
    	return $CI->excel;
    }

`ExcelController`控制器代码如下： 
    
    
    <?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');
    class Excel extends CI_Controller {
    
        function __construct() {
            parent::__construct();
    		$this->load->library('session');
        }
    	
    	function excelList()
    	{
    		// 读取 excel 内容
    		$this->load->helper('my_excel');
    		$excel = excel();
    		$excel->read('themes/default/demo.xls');
    		$data['excel_datas'] = $excel->sheets[0]['cells'];
    		//$data['excel_rows'] = $excel->sheets[0]['numRows'];//表的行数
    		$data['excel_cols'] = $excel->sheets[0]['numCols'];//表的列数
    
    		$data['title_for_layout'] = "Excel表格";
    		// 加载视图输出
    		$this->layout->view('admin/excelList', $data);
    	}
    	
    
    }

`View`视图文件主要代码如下： 
    
    
    <h2>读取 excel 内容</h2>
    <table cellpadding="0" cellspacing="0">
    	<?php foreach($excel_datas as $row): ?>
        <tr>
    		<?php for ($i = 1; $i <= $excel_cols; $i++):?>
        	<td width="80"><?php echo $row[$i]; ?></td>
    		<?php endfor;?>
        </tr>
        <?php endforeach; ?>
    </table>

然后就完成了，如果你能正确的浏览Excel内容，那么说明你成功了，**注意本教程CI使用PHP-ExcelReader时没有涉及到数据库方面，需要自行扩充。**