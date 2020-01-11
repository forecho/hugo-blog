---
title: "CakePHP的小技巧用法"
date: 2014-01-03T11:25:00+08:00
tags: ["cakephp"] 
draft: false
toc: true
---

**1、问：如果控制器有一个方法，不要视图怎么办？**

答：使用：$this->autoRender = false

**2、对于一直在开发的网站，在频繁更新的情况下，如何保证css文件、js文件和image文件不调用浏览器缓存文件问题？**

答：一个是在调用的文件名最后加变量后缀，如时间。示例代码如下：


    <link rel="stylesheet" type="text/css" href="https://forecho.com/css/index.css?2014-01-03-1" media="all" />

二如果是CakePHP的话还可以修改core.php配置文件，代码如下：


    /**
     * Apply timestamps with the last modified time to static assets (js, css, images).
     * Will append a querystring parameter containing the time the file was modified. This is
     * useful for invalidating browser caches.
     *
     * Set to `true` to apply timestamps when debug > 0. Set to 'force' to always enable
     * timestamping regardless of debug value.
     */
        Configure::write('Asset.timestamp', true);

这样的好处是你接下来可以直接用CakePHP自带的写法调用图片和文件了，如：


    <?php
    	echo $this->Html->css('main');
    	echo $this->Html->image();
    ?>

**3、点赞的时候需要+1，如何更新数据库？**

代码如下：


    $this->Widget->updateAll(
        array('Widget.numberfield' => 'Widget.numberfield + 1'),
        array('Widget.id' => 1)
    );

**4、如何通过主键最简单的方式获取到一条数据？**

代码如下：


    // 只获取name字段信息
    $this->User->read("name", $id);
    // 获取所有信息
    $this->User->read(null, $id);

**5、CakePHP控制器如何返回上一页？**


    $this->redirect($this->referer());

**6、CakePHP A控制器调用B控制器**


    $this->requestAction(
        array('controller'=>'Wx','action'=>'aa'),
        array('data'=>
            array('xing'=>'A1','ming'=>'A2')
        )
    );

这样可以在A控制器调用B控制器方法，而且在后面传参过去，用$this->request->data获取参数值。

**7、输出单个页面执行的SQL语句**


    $log = $this->Model->getDataSource()->getLog(false, false);
    debug($log);

Model要改一下名字才能用。