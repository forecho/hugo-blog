---
title: "Yii 非主键关联"
date: 2014-01-24T13:53:00+08:00
tags: ["yii"] 
draft: false
toc: true
---

```php
function relations()
    {
        return array(
            'last_experience' => array(
                self::HAS_ONE,
                'Experience',
                '',
                'on' => 'user_id=last_experience.user_id'
            ),
        );
    }
```