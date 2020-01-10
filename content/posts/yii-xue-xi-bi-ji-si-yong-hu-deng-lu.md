---
title: "yii学习笔记（四）-用户登录"
date: 2012-07-10T15:44:00+08:00
tags: ["yii"] 
draft: false
toc: true
---

先在 \protected\components\文件夹里面找到UserIdentity.php

然后用gii的Model Generator生成User的Model

然后在UserIdentity.php找到这段代码：


    public function authenticate()
    {
        $users=array(
            // username => password
            'demo'=>'demo',
            'admin'=>'admin',
        );
        if(!isset($users[$this->username]))
            $this->errorCode=self::ERROR_USERNAME_INVALID;
        else if($users[$this->username]!==$this->password)
            $this->errorCode=self::ERROR_PASSWORD_INVALID;
        else
            $this->errorCode=self::ERROR_NONE;
        return !$this->errorCode;
    }


替换为：


    private $_id;

    public function authenticate()
    {
        //用户名转换为小写
        $username=strtolower($this->username);

        //$username作为条件进入数据库查询匹配
        $user=User::model()->find('LOWER(username)=?',array($username));

        //用户名不存在，报错
        if ($user===null) {
            $this-> errorCode=self::ERROR_USERNAME_INVALID;
        }else{

            //调用一个函数，匹配相应的密码
            if (!$user->validatePassword($this->password)) {
                $this->errorCode=self::ERROR_PASSWORD_INVALID;
            }else {

                //匹配成功，赋值
                $this->_id = $user->id;
                $this->username = $user->username;
                $this->errorCode=self::ERROR_NONE;
            }
        }
        return $this->errorCode === self::ERROR_NONE;
    }

    public function getId() {
        return $this->_id;
    }


在Models里面的User.php添加两个新的方法。添加如下代码：



    // 查询密码是否匹配
    public function validatePassword($password)
    {
        return $this->encrypt($password)===$this->password;
    }

    public function encrypt($pass)
    {
        return md5($pass);
    }

    // 添加的密码进行MD5加密
    protected function beforeSave() {
        if (parent::beforeSave()) {
            //判断是否是新的密码
            if ($this->isNewRecord) {
                $this->password = $this->encrypt($this->password);
            }
            return true;
        }else {
            return false;
        }
    }


然后用gii的Crud Generator生成User的Controllers文件UserController.php 这个文件的下面代码是一个简单的权限系统


    public function accessRules()
    {
        return array(
            array('allow',  // allow all users to perform 'index' and 'view' actions
                'actions'=>array('index','view'),
                'users'=>array('*'),
            ),
            array('allow', // allow authenticated user to perform 'create' and 'update' actions
                'actions'=>array('create','update'),
                'users'=>array('@'),
            ),
            array('allow', // allow admin user to perform 'admin' and 'delete' actions
                'actions'=>array('admin','delete'),
                'users'=>array('admin'),
            ),
            array('deny',  // deny all users
                'users'=>array('*'),
            ),
        );
    }

注：
- “ * ”----任何人都可以访问。
- “ @ ”----登录用户才能访问。
- “ admin ”----是指只有admin用户才能访问。