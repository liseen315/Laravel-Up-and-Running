# Facades和容器

到目前为止，我们已经在书中介绍了很多facade，但实际上我们还没有讨论它们是如何工作的。

Laravel的facade是提供简单访问Laravel功能核心部分的类。facade有两个特征:首先，他们在全局命名空间可用\(\Log是\Illuminate\Support\Facades\Log别名\)，其次他们使用静态方法访问非静态资源。

让我们看一下Log facade，因为我们已经在本章介绍了logging，在你的控制器或者视图你可以如此调用。

```php
Log::alert('Something has gone wrong!');
```

下面是在没有facade的情况下进行相同调用的效果：

```php
$logger = app('log');
$logger->alert('Something has gone wrong!');
```

正如您所看到的，Facade将静态调用（您在类本身上进行的任何方法调用，使用::而不是实例）转换为对实例的常规方法调用。

> 导入Facade命名空间。
>
> 如果你在一个命名空间class内，你需要在文件顶部导入命名空间。

```php
...
use Illuminate\Support\Facades\Log;
class Controller extends Controller {
    public function index() {
        // ...
        Log::error('Something went wrong!');
    }
```

