---
title: "CI自定义分页类"
date: 2012-10-29T16:13:00+08:00
categories: 
draft: false
toc: true
---

之前我写过一篇文章，介绍了CI如何分页，[请猛击~](/?p=286) 那么考虑到一个项目要分页的地方比较多，所以后来我们写了一个单独的分页类，这样一个项目的效率会大大提升。 1、首先，我们在  application / libraries /下面创建page.php（扩展分页类）文件，代码如下： 
    
    
    <?php
    
    class Page
    { 
    	protected $url;
    	protected $total;
    	protected $size;
    	protected $segment;
    	protected $CI;
    
    	function __construct($value){
    
    		$this->url=$value['url'];
    		$this->size=$value['size'];
    		$this->total=$value['total'];
    		//$this->segment=$value['uri'];
    
    		$this->CI=& get_instance();
    
    		$this->CI->load->library('pagination');
    
    	}
    
    	function fy(){
    
    		return $this->page();
    
    	}
    
    	protected function page(){
    
    		$config['base_url']=base_url($this->url);
    		$config['total_rows']=$this->total;
    		$config['per_page']=$this->size;
    		//$config['uri_segment']=$this->segment;
    		//GET分页 传参
    		$config['page_query_string'] = TRUE;
    
    		$config['num_links']=3;
    		$config['first_link']='首页';
    		$config['last_link']='末页';
    		$config['prev_link']='上一页';
    		$config['next_link']='下一页';
    		$config['cur_tag_open'] = ' <a class="current">'; // 当前页开始样式  
    		$config['cur_tag_close'] = '</a>'; // 当前页结束样式  
    
    //      $this->CI->load->library('pagination', $config);
    
    		$this->CI->pagination->initialize($config);
    
    		return $this->CI->pagination->create_links();
    
    	}
    
    }
    
    ?>

2、那分页类写好了，如何使用呢？在控制器里面 controllers / feadmin.php文件（此处要结合你自己的项目，feadmin.php是我的控制器文件名）。下面是我们文章列表的示例代码： 
    
    
    function postsList() {
    		$data['posts'] = $this->fe_model->page('posts', 'feadmin/postsList?',  @$_GET['per_page'], 2, 'id desc','category','posts.category = category.cid ');
    		//print_r($data['posts']);
    		$data['category'] = $this->fe_model->selectCate();
    		//$data['category1'] = $this->fe_model->selectFormWhere('category',);
    		//print_r($data['category1']);
            $data['title_for_layout'] = '文章列表';
    
            $this->load->view('admin/postsView', $data);
        }

此处我使用的是CI** GET分页**方法，具体需要看你的项目是否需要，此处可参考手册开启GET翻页功能。 fe_model是我项目的模型文件，那么要实现翻页，还需要在模型中写下面的方法： 
    
    
    //分页
    	function page($form, $url, $offset, $size, $order, $join, $joinArray){
    
    		$fy['url'] = $url;
    		$fy['total'] = $data['total'] = $this->fy_n($form);
    		$fy['size'] = $data['size'] = $info['size'] = $size;
    		//$fy['uri'] = $offset;
    		$this->load->library('page', $fy);
    		$data['fy'] = $this->page->fy();
    		//print_r($data['fy']);
    		//$info['start'] = $data['start'] = $this->uri->segment($offset, 0);
    		$info['start'] = $data['start'] = $offset;
    		$info['order'] = $order;
    		$data['admin'] = $this->fy_info($form, $info, $join, $joinArray);
    
    		return $data;
    
    	}
    
    	function fy_n($form){
    
    		return $this->db->get($form)->num_rows();
    
    	}
    
    	function fy_info($form,$value,$join,$joinArray){
    
    		$this->db->order_by($value['order']);
    		$this->db->limit($value['size'],$value['start']);
    		if($join != ""){
    			$this->db->join($join,$joinArray);
    		}
    		return $this->db->get($form)->result();
    
    	}
    
    	function pageWhere($form, $url, $offset, $size, $where, $order, $join, $joinArray){
    
    			$fy['url'] = $url;
    			$fy['total'] = $data['total'] = $this->p_numWhere($form, $where);
    			$fy['size'] = $data['size'] =$info['size'] = $size;
    			//$fy['uri'] = $offset;
    			$this->load->library('page', $fy);
    			$data['fy'] = $this->page->fy();
    			//print_r($data['fy']);
    			//$info['start'] = $data['start'] = $this->uri->segment(offset, 0);
    			$info['start'] = $data['start'] = $data['start'] = $offset;
    			$info['order'] = $order;
    			$data['admin'] = $this->fy_infoWhere($form, $where, $info, $join, $joinArray);
    
    			return $data;
    
    	}
    	function p_numWhere($form, $where){
    
    		if(isset($where['where'])){
    			$this->db->where($where['where']);
    		}
    		if(isset($where['like'])){
    			$this->db->like($where['like'])	;
    		}
    
    		return $this->db->get($form)->num_rows();
    
    	}
    
    	function fy_infoWhere($form, $where, $value, $join, $joinArray){
    
    		if(isset($where['where'])){
    			$this->db->where($where['where']);
    		}
    		if(isset($where['like'])){
    			$this->db->like($where['like']);
    		}
    
    		$this->db->order_by($value['order']);
    		$this->db->limit($value['size'],$value['start']);
    		if($join != ""){
    			$this->db->join($join,$joinArray);
    		}
    
    		return $this->db->get($form)->result();
    
    	}

3、下面的视图文件的代码： 
    
    
    <tbody>
    	<?php foreach ($posts['admin'] as $post):
    		?>
    	<tr>
    		<td>
    		  <input type="checkbox" name="checkbox[]" value="<?php echo $post->id; ?>" />
    		</td>
    		<td><a href="feadmin/posts/<?php echo $post->id;?>" title="修改文章"><?php echo $post->title;?></a></td>
    		<td><a href="feadmin/postsSearch/?category=<?php echo $post->category;?>" title="查询“<?php echo $post->name;?>”分类"><?php echo $post->name;?></a></td>
    		<!-- <td><?php //echo date('Y-m-d', strtotime($post->addtime));?></td> -->
    		<td><?php echo $post->addtime;?></td>
    		<td>
    			<a href="feadmin/posts/<?php echo $post->id;?>" title="修改"><img src="resources/images/icons/pencil.png" alt="修改" /></a> 
    			<?php if($post->type != 1){?>
    				<a href="feadmin/postsDelete/<?php echo $post->id;?>" title="删除" onclick="return(confirm('确定删除?'))"><img src="resources/images/icons/cross.png" alt="删除" /></a> 
    				<!-- <a href="#" title="Edit Meta"><img src="resources/images/icons/hammer_screwdriver.png" alt="Edit Meta" /></a> -->
    			<?php }?>
    		</td>
    	</tr>
      <?php 
      endforeach;?>
    </tbody>

翻页的按钮代码如下： 
    
    
    <div class="pagination">
    	<?php echo $this->pagination->create_links(); ?>
    </div>

此处值得注意的是<?php foreach (**$posts['admin']** as $post):?>   完整项目代码请参考[这里~](https://github.com/forecho/Fecms/)