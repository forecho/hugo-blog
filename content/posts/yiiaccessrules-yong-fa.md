---
title: "Yii—accessRules用法"
date: 2012-12-12T11:58:00+08:00
categories: 
draft: false
toc: true
---

**一、访问控制过滤器（Access Control Filter）** 访问控制过滤器是检查当前用户是否能执行访问的controller action的初步授权模式。这种授权模式基于用户名，客户IP地址和访问类型。访问控制过滤器适用于简单的验证。需要复杂的访问控制，需要使用将要讲解到的基于角色访问控制（role-based access (RBAC)）. 在控制器（controller）里重载`CController::filters`方法设置访问过滤器来控制访问动作(看 Filter 了解更多过滤器设置信息)。 
    
    
    class PostController extends CController
    {
    ......
    public function filters()
        {
            return array(
                'accessControl',
            );
        }
    }

在上面，设置的[access control](http://www.yiiframework.com/doc/api/1.1/CController#filterAccessControl)过滤器将应用于`PostController`里每个动作。过滤器具体的授权规则通过重载控制器的[CController::accessRules](http://www.yiiframework.com/doc/api/1.1/CController#accessRules)方法来指定。 
    
    
    class PostController extends CController
    {
        ......
        public function accessRules()
        {
            return array(
                array('deny',
                    'actions'=>array('create', 'edit'),
                    'users'=>array('?'),
                ),
                array('allow',
                    'actions'=>array('delete'),
                    'roles'=>array('admin'),
                ),
                array('deny',
                    'actions'=>array('delete'),
                    'users'=>array('*'),
                ),
            );
        }
    }

上面设定了三个规则，每个用个数组表示。数组的第一个元素不是`'allow'`就是`'deny'`，其他的是名-值成对形式设置规则参数的。上面的规则这样理解：`create`和`edit`动作不能被匿名执行；`delete`动作可以被`admin`角色的用户执行；`delete`动作不能被任何人执行。 访问规则是一个一个按照设定的顺序一个一个来执行判断的。和当前判断模式（例如：用户名、角色、客户端IP、地址）相匹配的第一条规则决定授权的结果。如果这个规则是`allow`，则动作可执行；如果是`deny`，不能执行；如果没有规则匹配，动作可以执行。 为了确保某类动作在没允许情况下不被执行，设置一个匹配所有人的`deny`规则在最后，类似如下： 
    
    
    return array(
     // ... 别的规则...
     // 以下匹配所有人规则拒绝'delete'动作
     array('deny',
     'action'=>'delete',
     ),
     );

因为如果没有设置规则匹配动作，动作缺省会被执行。 访问规则通过如下的上下文参数设置： [actions](http://www.yiiframework.com/doc/api/1.1/CAccessRule#actions): 设置哪个动作匹配此规则。 [users](http://www.yiiframework.com/doc/api/1.1/CAccessRule#users): 设置哪个用户匹配此规则。此当前用户的[name](http://www.yiiframework.com/doc/api/1.1/CWebUser#name) 被用来匹配. 三种设定字符在这里可以用： 

  * `*`: 任何用户，包括匿名和验证通过的用户。
  * `?`: 匿名用户。
  * `@`: 验证通过的用户。
[roles](http://www.yiiframework.com/doc/api/1.1/CAccessRule#roles): 设定哪个角色匹配此规则。这里用到了将在后面描述的[role-based access control](http://www.yiiframework.com/doc/guide/1.1/zh_cn/topics.auth#role-based-access-control)技术。In particular, the rule is applied if [CWebUser::checkAccess](http://www.yiiframework.com/doc/api/1.1/CWebUser#checkAccess) returns true for one of the roles.提示，用户角色应该被设置成`allow`规则，因为角色代表能做某些事情。 [ips](http://www.yiiframework.com/doc/api/1.1/CAccessRule#ips): 设定哪个客户端IP匹配此规则。 [verbs](http://www.yiiframework.com/doc/api/1.1/CAccessRule#verbs): 设定哪种请求类型(例如：`GET`, `POST`)匹配此规则。 **二、访问规则中：** expression: 设定一个PHP表达式。它的值用来表明这条规则是否适用。在表达式，你可以使用一个叫`$user`的变量，它代表的是`Yii::app()->user`。 expression的具体用法： 
    
    
    class AdminController extends CController
    {
    
    ……
        public function accessRules()
        {
            return array(
            array('allow',  //允许所有人执行'login','error','index'
                    'actions'=>array('login','error','index'),
                    'users'=>array('*'),
            ),
            array('allow', //允许超级管理员执行所有动作
                    'actions'=>array('create','update','delete'),
                    'expression'=>array($this,'isSuperAdmin'),
            ),
            array('allow',//允许普通管理员执行
                    'actions'=>array('update'),
                    'expression'=>array($this,'isNormalAdmin'),    //表示调用$this(即AdminController)中的isNormalAdmin方法。
            ),      
            array('deny',  // deny all users
                    'users'=>array('*'),
            ),
            );
        }
        protected function isSuperAdmin($user)//判断是否是超级管理员
        {
            return ($this->loadModel($user->id)->adminAdminFlag==1);
        }
        protected function isNormalAdmin($user)//判断是否是普通管理员
        {
            return ($this->loadModel($user->id)->adminAdminFlag==0);
        }
    
        public function loadModel($id)
        {
            $model=Admin::model()->findByPk((int)$id);
            if($model===null){
                throw new CHttpException(404,'页面不存在');     
            }
            return $model;
        }
    }

注：其中$user代表`Yii::app()->user`即登录用户。