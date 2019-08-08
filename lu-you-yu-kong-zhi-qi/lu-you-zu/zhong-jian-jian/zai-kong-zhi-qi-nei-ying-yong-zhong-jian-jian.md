# 在控制器内应用中间件

通常将中间件应用在控制器内比直接在路由定义内使用更清晰,更直接。你可以在控制器构造函数中调用middleware\(\)来实现,给middleware\(\)方法传递字符串中间件名称,你可以选择链接修饰符方法\(only\(\)和except\(\)\)来定义哪些方法将接收该中间件

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class WelcomeController extends Controller
{
    public function __construct()
    {
        $this->middleware('auth');
        $this->middleware('admin-auth')->only('editUsers');
        $this->middleware('team-member')->except('editUsers');
    }

}
```

请注意,如果你定义了大量的only\(\),except\(\),那么你应该为此创建一个新的控制器

