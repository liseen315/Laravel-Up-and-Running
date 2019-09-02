# Laravel框架内的构造注入

我们已经介绍了构造注入的概念，我们也研究了容器如何解析类实例后者是接口。你也看到了使用app\(\)辅助函数来创建实例，以及容器在创建时解析构造类依赖。

我们尚未涉及的是容器如何负责解析应用程序的许多核心操作类。例如，每个控制器都是由容器实例化的。这意味着，如果你想要在控制器内实例化一个logger。你可以在控制器构造内类型限定一个logger类，当Laravel创建控制器，它将从容器解析并且Logger实例将对控制器可用。如示例11-12。

{% code-tabs %}
{% code-tabs-item title="Example 11-12. Injecting dependencies into a controller" %}
```php
class MyController extends Controller {
    protected $logger;
    public function __construct(Logger $logger) {
        $this->logger = $logger;
    }
    public function index() {
        // Do something
        $this->logger->error('Something happened');
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

容器负责解析控制器、中间件、队列作业、事件侦听器，以及在应用程序的生命周期过程中由Laravel自动生成的任何其他类，所以任何类都可以被类型限定依赖到他们的构造器，然后执行自动注入。



