# 绑定到闭包

让我们看下如何绑定到容器，注意绑定到容器的合适位置是放置在服务提供者的register\(\)方法中，如示例11-5。

{% code-tabs %}
{% code-tabs-item title="Example 11-5. Basic container binding" %}
```php
// In any service provider (maybe LoggerServiceProvider)
public function register() {
    $this->app->bind(Logger::class, function ($app) { 
        return new Logger('\log\path\here', 'error');
    }); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

在这个例子中有一些重要的事情需要注意。首先，我们运行$this-&gt;app-&gt;bind\(\)。$this-&gt;app是容器的一个实例，每个服务提供者都可以使用它。容器的bind\(\)方法是我们用来绑定到容器的。

bind\(\)的第一个参数是我们要绑定到的“key”。这里我们使用了完全限定类名FQCN。第二个参数根据你所设置不同而不同，但本质上它应该是一些向容器显示如何解析绑定键的实例的内容。

所以，在这个例子中，我们传递了一个闭包。 现在，每当有人运行app\(Logger::class\)时，他们都会得到这个闭包的结果。 闭包传递容器本身的一个实例\($app\)，所以如果你正在解析的类有你想要从容器解析出的依赖，你可以在你的定义中使用它，如例11-6所示。

{% code-tabs %}
{% code-tabs-item title="Example 11-6. Using the passed $app instance in a container binding" %}
```php
// Note that this binding is not doing anything technically useful, since this // could all be provided by the container's auto-wiring already. 
$this->app->bind(UserMailer::class, function ($app) {
    return new UserMailer( 
        $app->make(Mailer::class),
        $app->make(Logger::class),
        $app->make(Slack::class)
    ); 
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

请注意，每次请求类的新实例时，此闭包都将再次运行，并返回新的输出。

