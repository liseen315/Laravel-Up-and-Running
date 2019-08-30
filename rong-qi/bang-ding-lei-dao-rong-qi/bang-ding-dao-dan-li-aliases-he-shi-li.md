# 绑定到单例,Aliases和实例

如果您希望缓存绑定闭包的输出，以便每次请求实例时都不会重新运行此闭包，那么这使用单例模式，您可以运行$this-&gt;app-&gt;singleton\(\)来执行此操作。示例11-7显示了这是什么样子的。

{% code-tabs %}
{% code-tabs-item title="Example 11-7. Binding a singleton to the container" %}
```php
public function register() {
    $this->app->singleton(Logger::class, function () { 
        return new Logger('\log\path\here', 'error');
    }); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果你已经有了一个想要单例返回的对象实例，你也可以执行类似操作，如示例11-8

{% code-tabs %}
{% code-tabs-item title="Example 11-8. Binding an existing class instance to the container" %}
```php
public function register() {
    $logger = new Logger('\log\path\here', 'error');
    $this->app->instance(Logger::class, $logger);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

最后，如果你想要给类起个别名，绑定类到快捷方式或者绑定快捷方式到类，你可以传递两个字符串，如示例11-9

{% code-tabs %}
{% code-tabs-item title="Example 11-9. Aliasing classes and strings" %}
```php
// Asked for Logger, give FirstLogger
$this->app->bind(Logger::class, FirstLogger::class); 
// Asked for log, give FirstLogger
$this->app->bind('log', FirstLogger::class); 
// Asked for log, give FirstLogger
$this->app->alias(FirstLogger::class, 'log');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

注意这些快捷方式在Laravel的核心中很常见，它提供了一个快捷方式到类的系统提供了一一些核心功能，就像Log的easy-to-remember。



