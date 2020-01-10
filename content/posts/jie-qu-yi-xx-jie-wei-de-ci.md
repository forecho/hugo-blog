---
title: "截取以xx结尾的词"
date: 2014-05-14T11:06:00+08:00
tags: ["php"] 
draft: false
toc: true
---

```php
function endsWith($haystack, $needle)
{
   $length = strlen($needle);
   if ($length == 0) {
       return true;
   }

   return (substr($haystack, -$length) === $needle);
}
```