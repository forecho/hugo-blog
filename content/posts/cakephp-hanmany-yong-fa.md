---
title: "CakePHP hanMany用法"
date: 2013-12-18T11:02:00+08:00
categories: 
draft: false
toc: true
---

hanMany就是一对多的关系，比方说我们现在一篇文章有多条评论，那么这个就是一对多的关系。 我们在文章的模型里面写hasMany就可以关联评论了。使用方法如下： 
    
    
    var $hasMany = array('ProductsSku' =>
    		array('className'     => 'ProductsSku',//关联对象类名
    				'conditions'    => '',//关联对象限定条件
    				'order'         => '',//关联对象排序子句
    				'limit'         => '',//检索的关联对象数量
    				'foreignKey'    => 'prod_id',//外键字段名
    				'dependent'     => true,//是否级联删除
    				'exclusive'     => false,//：如果设为true，所有的关联对象将在一句sql中删除，model的beforeDelete回调函数不会被执行。但是如果没有复杂的逻辑在级联删除中，这样的设定会带来性能上的优势。（译注：Cake的确方便，但是使用时一定要记住控制sql语句发送数量）
    				'finderQuery'   => ''//定义一句完整的sql语句来检索关联对象，能够对关联规则进行最大程度上的控制。当关联关系特别复杂的时候，比如one table - many model one model - many table的情况下，Cake无法准确的替你完成映射动作，需要你自己来完成这个艰巨的任务。
    		)
    );

在CakePHP命名规范里面规定每一张表都有ID为主键的字段，作为外键foreignKey的字段命名：其默认值为当前模型的单数模型名缀以 ‘_id’。如果你遵守这个使用起来将非常简单。 下面我们来说个实例。 文章的表名为Articles，文章评论表名为Comments。那么文章的模型文件为Article.php，文章评论的模型文件为Comment.php。（单复数命名规则） 那么在Article.php文件中这样写就可以实现简单的关联评论表了。 
    
    
    class Article extends AppModel
    {
        public $name = 'Article';
    
        public $hasMany = array(
            'Comment' => array(
                'className'  => 'Comment',
            )
        );
    }

参考资料： <http://www.cnblogs.com/matchless/archive/2013/02/01/2889134.html> <http://www.21haolou.com/articles/show/88>