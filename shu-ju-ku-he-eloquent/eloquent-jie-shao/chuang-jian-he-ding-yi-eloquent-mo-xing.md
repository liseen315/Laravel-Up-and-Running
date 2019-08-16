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



