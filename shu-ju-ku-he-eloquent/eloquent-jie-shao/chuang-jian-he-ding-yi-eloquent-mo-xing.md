# 创建和定义Eloquent模型

首先，让我们创建一个模型,这是一条Artisan命令

`php artisan make:model Contact`

然后会生成如下文件在app/Contact.php

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Contact extends Model
{
    //
}
```

> 创建模型同时创建迁移
>
> 当你创建模型时想自动创建迁移,可以通过在命令后面添加-m或者--migration标记
>
> `php artisan make:model Contact -m`

**表名**

Laravel通过“snake cases”并将您的类名复数化后命名表，因此SecondaryContact将以secondary\_contacts名字访问,如果你想自定义表名,需要在模型上明确指定$table属性

`protected $table = 'contacts_secondary';`

**主键**

默认情况下，Laravel假定每个表都有一个自增的整数主键，它将被命名为id。

如果你想更改主键名,需要更改$primaryKey属性

`protected $primaryKey = 'contact_id';`

如果不想自增的话,请进行如下设置:

`public $incrementing = false;`

**时间戳**

Eloquent期望每个表都有create\_at和updated\_at时间戳字段,如果你不想拥有他们，请禁用掉$timestamps功能

`public $timestamps = false;`

通过将$dateformat class属性设置为自定义字符串，可以自定义时间戳存储到数据库的格式,字符串将通过PHP’s date\(\)进行解析,如下将采用unix秒来存储

`protected $dateFormat = 'U';`

