---
title: "CakePHP ajax分页"
date: 2014-01-23T16:09:00+08:00
tags: ["cakephp"] 
draft: false
toc: true
---

Controller文件头部调用CakePHP自带分页并且配置分页条数代码：


    public $components = array('RequestHandler', 'Paginator');
    public $paginate = array('limit' => 2 );  //2是为了方便测试

调用的action方法如下：


    public function list()
    {
        $this->Paginator->settings = $this->paginate;
        $data = $this->Paginator->paginate('Notice');
        $this->set('data', $data);
    }

list.ctp文件，代码如下：


    <?php echo $this->Js->writeBuffer(array('inline'=>false));?>
    <div id="search_result">
        <?php foreach ($data as $key => $value): ?>
            <?php echo $value['Notice']['title'] ?>
        <?php endforeach ?>
        <?php $this->Paginator->options(array(
                    'update' => '#search_result',
                    'evalScripts' => true,
        ));?>
        <?php echo $this->Paginator->prev('上一页' . __('', true), array(), null, array('class'=>'disabled'));?>
        <?php echo $this->Paginator->numbers(array('class' => 'numbers', 'first' => false, 'last' => false));?>
        <?php echo $this->Paginator->next(__('下一页', true) . ' >>', array(), null, array('class' => 'disabled'));?>
        <?php echo $this->Js->writeBuffer(array('inline'=>true));?>
    </div>

参考文章： <http://endoyuta.com/2013/05/06/cakephp-2-3-ajax%E3%81%AApagination/> <http://caky.de/en/core-libraries/helpers/js.html#ajax-pagination>