# 方法注入

在应用程序一些地方Laravel不仅读取了构造签名，它还读取了方法签名并执行了注入依赖。

使用方法注入最常见的地方是控制器方法，如果你有一个依赖想要为控制器方法使用，你可以如示例11-13一样注入到方法中。

{% code-tabs %}
{% code-tabs-item title="Example 11-13. Injecting dependencies into a controller method" %}
```php
class MyController extends Controller{
    // Method dependencies can come after or before route parameters.
    public function show(Logger $logger, $id) {
        // Do something
        $logger->error('Something happened');
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> 使用makeWith\(\)传递不可解析的构造参数
>
> 所有用于解析类实例的工具-app\(\),$container-&gt;make\(\)等等。假设可以解析类的所有依赖项，而无需传入任何内容，但是，如果您的类在其构造函数中接受一个值，而不是容器可以为您解决的依赖项，该怎么办？请使用makeWith\(\)方法。

```php
class Foo {
    public function __construct($bar) {
        // ...
    } 
}
$foo = $this->app->makeWith(
    Foo::class,
    ['bar' => 'value']
);
```

> 这是一个不常见案例。 您将从容器中解析出来的大多数类应将依赖项注入其构造函数中。

服务提供者者的boot\(\)方法也可以做同样的事，你也可以在容器上调动任意类的任意方法，用于进行方法注入，如示例11-14。

{% code-tabs %}
{% code-tabs-item title="Example 11-14. Manually calling a class method using the container’s call\(\) method" %}
```php
class Foo {
    public function bar($parameter1) {} 
}
// Calls the 'bar' method on 'Foo' with a first parameter of 'value'
app()->call('Foo@bar', ['parameter1' => 'value']);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

