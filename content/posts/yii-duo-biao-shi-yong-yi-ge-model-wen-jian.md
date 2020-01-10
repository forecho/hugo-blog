---
title: "Yii 多表使用一个Model文件"
date: 2013-08-03T14:20:00+08:00
categories: 
draft: false
toc: true
---

表结构一样，现在需要把这些表全部都的数据都查出来，使用一个Model文件。 首先要声明三个私有变量： 
    
    
    private $tableName = 'fr_goods';//默认表名
    private static $_models=array();
    private $_md;

添加 构造函数:创建和初始化对象成员属性，代码如下： 
    
    
    public function __construct($scenario='search', $tableName = null)
    {
        if($tableName !== null)
            $this->tableName = $tableName;
        parent::__construct($scenario);
    }

把默认的function tableName() 改为如下： 
    
    
    public function tableName()
    {
         return $this->tableName;
    
    }

这个时候你调用的时候，输出tableName的时候，虽然成功了，但是数据还是没有变。 下面我们要改变数据元才行。   把默认生成的 function model()，改成如下代码： 
    
    
    public static function model($tableName = false, $className=__CLASS__)
    {
        if($tableName === null) $className=null; // 这个字符串将节省内部CActiveRecord的功能
        if(!$tableName)
            return parent::model($className);
    
        if(isset(self::$_models[$tableName.$className]))
            return self::$_models[$tableName.$className];
        else
        {
            $model=self::$_models[$tableName.$className]=new $className(null);
            $model->tableName = $tableName;
    
            $model->_md=new CActiveRecordMetaData($model);
            $model->attachBehaviors($model->behaviors());
    
            return $model;
        }
    }

最关键的是添加下面这行代码： 
    
    
    public function getMetaData()
    {
        if($this->_md!==null)
            return $this->_md;
        else
            return $this->_md=static::model($this->tableName())->_md;
    }

现在配置完成。下面我们写一个调用的小例子。 下面是Model的function search()代码： 
    
    
    public function search()
    {
        // Warning: Please modify the following code to remove attributes that
        // should not be searched.
    
        $criteria=new CDbCriteria;
    
        $criteria->compare('id',$this->id,true);
        $criteria->compare('goods_sn',$this->goods_sn,true);
        $criteria->compare('language_ids',$this->language_ids,true);
        $criteria->compare('site_ids',$this->site_ids,true);
    
        return new CActiveDataProvider($this, array(
            'criteria'=>$criteria,
        ));
    }

Controller文件代码如下： 
    
    
    public function actionAdmin()
    {
        $model=new KindsGoods('search', 'es_goods');//es_goods表名
        $model->unsetAttributes();  // clear any default values
        if(isset($_GET['KindsGoods']))
            $model->attributes=$_GET['KindsGoods'];
    
        $this->render('admin',array(
            'model'=>$model,
        ));
    }

这个时候输出的结果如果是es_goods表的数据，那就说明你成功了。   参考文章：<http://stackoverflow.com/questions/16399561/yii-one-model-for-multiple-tables>