---
title: "一个很简单的jquery图片自动切换"
date: 2012-12-10T14:02:00+08:00
categories: 
draft: false
toc: true
---

[效果页面~](http://www.dofaith.net/) html代码如下： 
    
    
    <div class="ad">
    	<a href="" target="_blank"><img src="01.jpg"  /></a>
    	<a href="" target="_blank"><img src="02.jpg"  /></a>
    	<a href="" target="_blank"><img src="03.jpg"  /></a>
    </div>

jquery代码如下： 
    
    
    <script>
    	function swapImages(){
    		var $active = $('.ad .active');
    		var $next = ($('.ad .active').next().length > 0) ? $('.ad .active').next() : $('.ad a:first');
    		$active.fadeOut(function(){
    		$active.removeClass('active');
    		$next.fadeIn().addClass('active');
    		});
    	}
    
        $(document).ready(function(){
    		$('.ad a:first').addClass('active');
    		  // Run our swapImages() function every 3secs
    		  setInterval('swapImages()', 3000);
        });
    </script>

CSS代码如下： 
    
    
    .ad a{
    	display:none;
    }
    .ad .active{
        display:block;
    }