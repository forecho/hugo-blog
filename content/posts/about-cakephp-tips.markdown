---
title: "CakePHP 使用小技巧"
date: 2014-11-19T22:00:00+08:00
tags: ["CakePHP"] 
draft: false
toc: true
---

### 知道主键 ID 更新一条数据，代码示例：

```php
$this->Order->id = $id;
$this->Order->saveField('status', $status);
```

### 点赞的时候需要+1，如何更新数据库？

```php
$this->Widget->updateAll(
    array('Widget.numberfield' => 'Widget.numberfield + 1'),
    array('Widget.id' => 1)
);
```

### 如何通过主键最简单的方式获取到一条数据？

```php
// 只获取name字段信息
$this->User->read('name', $id);
// 获取所有信息
$this->User->read(null, $id);
```

<!--more-->

### CakePHP控制器如何返回上一页？

```php
$this->redirect($this->referer());
```

### CakePHP A控制器调用B控制器？

```php
$this->requestAction(
    array('controller'=>'Wx','action'=>'aa'),
    array('data'=>
        array('xing'=>'A1','ming'=>'A2')
    )
);
```

这样可以在A控制器调用B控制器方法，而且在后面传参过去，用$this->request->data获取参数值。

### 输出单个页面执行的 SQL 语句

```php
$log = $this->Model->getDataSource()->getLog(false, false);
debug($log);
```

Model要改一下名字才能用。

### 模糊和 OR 搜索示例：

```php
$this->User->find('all', array(
   'conditions' => array(
   		'OR' =>array(
			array('nickname like ' => "%$keyword%"),
			array('User.id' => $keyword),
		)
	),
	'fields' => 'User.id,User.nickname'
));
```

### find 的 语法糖

```php
#findAllBy<fieldName>(string $value, array $fields, array $order, int $limit, int $page, int $recursive)
$this->Product->findAllByOrderStatus('3');
$this->User->findAllByUsernameAndPassword('jhon', '123');
$this->User->findAllByEmailOrUsername('jhon', 'jhon');
$this->User->findAllByLastName('psychic', array(),array('User.user_name' => 'asc'));

#findBy<fieldName>(string $value[, mixed $fields[, mixed $order]]);
$this->Recipe->findByType('Cookie');
$this->User->findByEmailOrUsername('jhon','jhon');
$this->User->findByUsernameAndPassword('jhon','123');
```

### CakePHP saveAll 的用法:

```php
for ($i=0; $i < count($data['product_id']); $i++) {
	$item[$i]['DiscountProduct']['discount_id'] = $this->Discount->id;
	$item[$i]['DiscountProduct']['discount'] = $data['discount'][$i];
}
$this->DiscountProduct->saveAll($item);
```

### 如果要是有 CakePHP 自带和 HTML 结合的 FORM
必须在 Controller 的 action 里面使用这个：`$this->request->data = $data;` 修改的时候才能读取数据，并且view里面的 form 要使用 CakePHP 的

```php
<?php echo $this->Form->create('PrintAd', array('type'=>'post')); ?>
```

### CakePHP 中表有以下字段名，则自动更新时间

```sql
`created` datetime NOT NULL,
`modified` datetime NOT NULL,
```

### CakePHP 自带图片+链接

```php
echo $this->Html->link(
     $this->Html->image($value['PrintClient']['weixin_code_img'], array('width'=>'60px')),
     $value['PrintClient']['weixin_code_img'],
     array('escape' => false)
);
```

### CakePHP 查询的时候表联接

```php
$options['joins'] = array(
	array(
		'table' => 'channels',
		'alias' => 'Channel',
		'type' => 'LEFT',
		'conditions' => array(
			'Channel.id = Item.channel_id',
		)
	));
$Item->find('all', $options);
```

### CakePHP 获取当前域名

```php
Router::fullbaseUrl()
```

### CakePHP 控制器构造函数的用法：

```php
public function__construct($request = null, $response = null)
{
	parent::__construct($request, $response);
	# code...
}
```

### CakePHP 视图获取 URL 的参数值

```php
#array():
$this->params->pass
#第一个值：
$this->params->pass[0]
```

### CakePHP 联表分页

```php
$this->loadModel('WifiList');
$this->SearchPagination->setup('WifiList');
$this->request->data['WifiList']['seller_id'] = SELLER_ID;
$this->paginate = array(
	'fields' => array('WifiList.*', 'WxPersistentQrcodes.ticket'),
	'conditions' => $this->WifiList->parseCriteria($this->request->data['WifiList']),
	'order' => 'WifiList.id desc',
	'joins' => array(
		array(
			'table'=>'wx_persistent_qrcodes',
			'alias'=>'WxPersistentQrcodes',
			'type'=>'LEFT',
			'conditions'=>array(
				'WifiList.wx_p_qrcode_id=WxPersistentQrcodes.scene_id and WxPersistentQrcodes.seller_id='.SELLER_ID
			)
		)
	),
	'limit' => 10
);
$data = $this->paginate('WifiList');
$this->set(compact('data'));
```

### CakePHP 抛出异常

```php
if(!$id){
	throw new NotFoundException();
}
```

### CakePHP 跳转链接

```php
$this->redirect(array(
	'controller'=>'dist',
	'action'=>'result',
	$status,
	'?'=>array('sid'=>SELLER_ID,)
));
```

### CakePHP Model 使用其他模型

```php
// the other model to load & use
App::uses('AnotherModel', 'Model');
class MyModel extends AppModel {
	public $name = 'MyModel';

	public function test() {
		// load the Model
		$anotherModel = new AnotherModel();
		// use the Model
		$anotherModel->save($data);
	}
}
```