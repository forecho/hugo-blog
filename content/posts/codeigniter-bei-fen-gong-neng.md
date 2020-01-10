---
title: "CodeIgniter备份功能"
date: 2012-10-23T12:53:00+08:00
categories: 
draft: false
toc: true
---

首先我犯了一个错误，就是表命名的时候使用了，特殊的数据字符，所以以后的项目请使用**表前缀**，可以尽量避免犯没必要而且很蛋疼的错误。 CI的database.php中的 $db['default']['dbprefix'] = ' '; 可以设置表前缀。 但是要注意一点：此功能只支持CI自带的Active Record 类，也就是说自己的写的sql语句是不支持的。   下面是根据手册写出的备份方法： 
    
    
    // 备份
    function backup(){
    
    	$this->load->dbutil();
    
    	$this->load->helper('file');
    
    	$prefs = array(
    			'tables'      => array(),  // 包含了需备份的表名的数组.如果留空将备份所有数据表
    			'ignore'      => array(),           // 备份时需要被忽略的表
    			'format'      => 'zip',             // gzip, zip, txt
    			'filename'    => 'mybackup'.date('Ymd').'.sql',    // 文件名 - 如果选择了ZIP压缩,此项就是必需的
    			'add_drop'    => TRUE,              // 是否要在备份文件中添加 DROP TABLE 语句
    			'add_insert'  => TRUE,              // 是否要在备份文件中添加 INSERT 语句
    			'newline'     => "\n"               // 备份文件中的换行符
    				  );
    
    	$backup = $this->dbutil->backup($prefs);
    
    	write_file('./mybackup'.date('Ymd').'.sql', $backup); 
    
    }

为了让程序自动执行，每天备份一次，我在后台控制器的index方法中写了下面的代码： 
    
    
    //检查是否存在文件，即是否备份，如果文件不存在则执行备份			
    $filename = './mybackup'.date('Ymd').'.sql';
    if (!file_exists($filename)){
    	$this->backup();
    }
    //删除一个星期之前的备份文件
    $oldfilename = './mybackup'.date('Ymd',(time()-3600*24*7)).'.sql';
    if (file_exists($oldfilename)){
    	@unlink($oldfilename);
    }

备份完成。至于还原数据库，未完待续————