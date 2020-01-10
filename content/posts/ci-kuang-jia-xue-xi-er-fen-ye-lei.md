---
title: "CI框架学习（二）——分页类"
date: 2011-11-23T09:54:00+08:00
tags: ["CodeIgniter"] 
draft: false
toc: true
---

手册写的很简单，看了无从下手，不过后来看了一个国外的视频教程。立马就懂了，国内的不行。 先看一下效果图：

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160806.png)

下面是控制器中的代码：

```php
function list_news($offset='') {
    $this->load->model('mhome');//加载模型
    $this->load->library('pagination'); // 加载分页类
    $limit = 1;// 每页显示数量
    $total = $this->mhome->count_news();// 统计数量
    $data['sel_news'] = $this->mhome->sel_news($limit,$offset);//调用模型，查询数据库

    $config['base_url'] = base_url().'chome/list_news/';// 分页的基础 URL
    $config['total_rows'] = $total;//记录总数
    $config['per_page'] = $limit; //每页条数

    //几行可选设置
    $config['full_tag_open'] = '<div class="pagination">'; // 分页开始样式
    $config['full_tag_close'] = '</div>'; // 分页结束样式
    $config['first_link'] = '首页'; // 第一页显示
    $config['last_link'] = '末页'; // 最后一页显示
    $config['next_link'] = '下一页 >'; // 下一页显示
    $config['prev_link'] = '< 上一页'; // 上一页显示
    $config['cur_tag_open'] = ' <a class="current">'; // 当前页开始样式
    $config['cur_tag_close'] = '</a>'; // 当前页结束样式
    $config['num_links'] = 2;// 当前连接前后显示页码个数

    $this->pagination->initialize($config); // 配置分页

    $data['pag_links'] = $this->pagination->create_links();//显示分页

    $this->load->view('admin/list_news',$data);
}
```

模型中是代码：

```php
//查询数据

function sel_news($limit,$offset) {
    $this->db->limit($limit,$offset);
    $query = $this->db->get('news');
    $row = $query->row('navid');
    return $query->result_array();
}

//查询数量
function count_news() {
    $query = $this->db->get_where('news');
    return $query->num_rows();
}
```

视图显示代码：

```php
<div id="page">
<?php echo $pag_links; ?>
</div>
```

CSS样式如下：

```css
/*************** Pagination for MeMo Blog ***************/
#page .pagination {
    border-top:1px solid #dfdfdf;
    padding-top: 10px;
    text-align: left;
    margin-bottom: 10px;
    font-size: 10px;
}
.pagination a ,.pagination a.number {
    margin: 0 5px 0 0;
    padding: 3px 6px;
    border: 1px solid #d0d0d0;
}
.pagination a:hover,.pagination a.current {
    border-color: #000 !important;
    color: #000 !important;
}

#page{
    float:right;
}
```

说实话，不是很喜欢CI自带的分页类，默认第4页才显示 【首页】、倒数第4页显示【尾页】。

下次有机会自己研究一个分页。

 

**注：**

```php
$config['uri_segment']= 3;
```

分页方法自动测定你 URI 的哪个部分包含页数。如果你需要一些不一样的，你可以明确指定它。默认是3。 $offset 默认是：$this->uri->segment(3)