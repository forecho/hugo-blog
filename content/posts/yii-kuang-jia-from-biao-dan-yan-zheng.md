---
title: "Yii框架From表单验证"
date: 2013-06-03T20:55:00+08:00
categories: 
draft: false
toc: true
---

**1.在控制器里面新建Helloworld控制器**
    
    
    class HelloworldController extends Controller{
        public function actionFeedback(){
            $model= new FeedbackForm;//实例化一个from
            $model->unsetAttributes();  // 清除默认值
            if(isset($_POST['FeedbackForm'])){//判断是否提交
                $model->attributes=$_POST['FeedbackForm'];//获得from表单的内容
                if($model->validate()){//进行验证（FeedbackForm方法里面的rules验证）
                    $this->renderPartial('feedbackshow',array('model'=>$model));
                }else{
                    var_dump($model->geterrors());//验证失败把失败原因打印出来
                } 
            }else{
                $this->renderPartial('feedback',array('model'=>$model));
            }
        }
    }

**2.在model下面新建FeedbackForm**
    
    
    class FeedbackForm extends CFormModel{
        public $name;
        public $email;
        public $subject;
        public $body;
        public function rules(){
            return array(
                    array('name,email,subject,body','required'),//不能为空
                    array('email','email'),//email格式验证
                    );
        }
        public function attributeLabels()//给变量别名
        {
            return array(
                'subject'=>'主题',
                'name'=>'用户名',
                'email'=>'E-mail',
                'body'=>'内容'
            );
        }
    }

**3.在view里面新建Helloworld文件夹，在文件夹里面新建feedback、feedbackshow视图** **feedback:**
    
    
    <?php header('Content-Type:text/html;charset=UTF-8');?>
    <div>
    <?php $form=$this->beginWidget('CActiveForm', array(
        'id'=>'feedback-form',
        'enableAjaxValidation'=>false,//是否使指定的属性的AJAX验证可用。默认值是false。
        'enableClientValidation'=>true,  //是否使客户端验证可用。默认值是false。 
        //下面是当表单被提交时是否执行AJAX验证。如果存在任何验证错误，表单的提交动作将被停止。默认值是false。   
        'clientOptions'=>array('validateOnSubmit'=>true),
    
    )); ?>
    <?php echo $form->hiddenField($model,'uid',array('value'=>Yii::app()->user->userid));//隐藏input示例 ?>
        <div>
            <?php echo $form->labelEx($model,'name');?>
            <?php echo $form->textField($model,'name');?>
            <?php echo $form->error($model,'name'); ?>
        </div>
        <div>
            <?php echo $form->labelEx($model,'email');?>
            <?php echo $form->textField($model,'email');?>
            <?php echo $form->error($model,'email'); ?>
        </div>
        <div>
            <?php echo $form->labelEx($model,'subject');?>
            <?php echo $form->textField($model,'subject',array('size'=>60,'maxlength'=>128));?>
            <?php echo $form->error($model,'subject'); ?>
        </div>
        <div>
            <?php echo $form->labelEx($model,'body');?>
            <?php echo $form->textArea($model,'body',array('rows'=>6,'cols'=>50));?>
            <?php echo $form->error($model,'body'); ?>
        </div>
        <div>
            <?php echo CHtml::submitButton('提交');?>
        </div>
    <?php $this->endWidget();?>
    </div>

**feedbackshow:**
    
    
    <?php header('Content-Type:text/html;charset=UTF-8');?>
    <div>
        <div>
            <?php echo "姓名:";?>
            <?php echo $model->name;?>
        </div>
        <div>
            <?php echo "E-mail:";?>
            <?php echo $model->email;?>
        </div>
        <div>
            <?php echo "主题：";?>
            <?php echo $model->subject;?>
        </div>
        <div>
            <?php echo "内容:";?>
            <?php echo $model->body;?>
        </div>
    </div>