# 控制器

我之前提到过几次控制器,但是到目前为止大多数的例子都是采用的闭包路由,在MVC模式中,控制器其实是一个组织或者多个路由逻辑的类,控制器倾向于将类似的路由逻辑组合在一起,特别是以传统的CRUD模式组织的应用程序.如此一来,控制器可以在特定资源处理所有动作.

> 什么是CRUD？
>
> CRUD代表创建、读取、更新、删除，这是Web应用程序通常在资源上提供的四种主要操作。例如，您可以创建一个新的博客文章，可以阅读该文章，可以更新它，也可以删除它。

把逻辑塞到控制器内看起来很赞,但最好把控制器看成应用程序HTTP请求的通讯警察,由于有其他方式发送请求到应用,比如,cron jobs, Artisan command-line calls, queue jobs,等等,因此不要过度的依赖控制器,这意味着控制的主要工作是传递互联网的HTTP请求到应用程序.

所以让我们创建一个控制器,一种简单的方式是使用Artisan命令,所以在命令行内执行如下命令:

```bash
php artisan make:controller TasksController
```

> Artisan与Artisan发电机
>
> Laravel内包含一个叫做Artisan的命令行工具,Artisan可以运行迁移,创建用户,数据库记录,以及执行许多其他手动一次性任务
>
> 在make下Artisan提供了一套工具用于创建系统骨架文件.于是我们可以运行php artisan make:controller.
>
> 学习更多Artisan的特性,请查看第8章

它将在app/Http/Controllers目录下创建一个名为TaskController的文件,内容如示例3-21所示

{% code-tabs %}
{% code-tabs-item title="Example 3-21. Default generated controller" %}
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class TaskController extends Controller
{
    //
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

按照示例3-22修改内容,创建一个公开index\(\)方法,我们在此方法内放回一些文本.

{% code-tabs %}
{% code-tabs-item title="Example 3-22. Simple controller example" %}
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class TaskController extends Controller
{
    public function index() {
        return 'Hello,World';
    }
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

然后我们可以像之前学到的,把它挂在到一个路由上,如示例3-23：

{% code-tabs %}
{% code-tabs-item title="Example 3-23. Route for the simple controller" %}
```php
<?php

Route::get('/','TaskController@index');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

然后访问'/'路由你将会看到"Hello,World"字样

> 控制器命名空间
>
> 在示例3-23中，我们引用了一个“App\Http\Controllers\TasksController“完全限定类名的控制器，但我们只使用了类名。这并不是因为我们可以简单地通过类名称引用控制器。相反，我们可以在引用控制器时忽略App\Http\Controllers\；默认情况下，laravel配置为在该命名空间中查找控制器。
>
> 这意味着，如果您有一个完全限定类名为”App\Http\Controllers\API\ExercisesController“的控制器，您将在路由定义中使用API\ExercisesController引用它。

