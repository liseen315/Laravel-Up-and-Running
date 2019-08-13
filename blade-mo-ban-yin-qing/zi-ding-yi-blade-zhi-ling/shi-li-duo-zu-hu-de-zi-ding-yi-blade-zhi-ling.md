# 示例:多租户的自定义Blade指令

让我们假设我们正在构建一个支持多租户的应用程序，这意味着用户可能会从www.myapp.com，client1.myapp.com，cli- ent2.myapp.com或其他地方访问该站点。

假设我们编写一个类来封装了多租户逻辑并命名为上下文,此类将捕获有关当前访问上下文的信息和逻辑，例如经过身份验证的用户是谁以及用户是访问公共网站还是客户端子域

我们可能经常在视图中解析Context类并在其上执行条件，如例4-31所示。 app（'context'）是从容器中获取类实例的快捷方式，我们将在第11章中了解更多信息

{% code-tabs %}
{% code-tabs-item title="Example 4-31. Conditionals on context without a custom Blade directive" %}
```php
@if (app('context')->isPublic()) 
    &copy; Copyright MyApp LLC
@else
    &copy; Copyright {{ app('context')->client->name }}
@endif
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果我们将@if \(app\('context'\)-&gt;isPublic\(\)\) 简化成@ifPublic呢？查看示例4-32

{% code-tabs %}
{% code-tabs-item title="Example 4-32. Conditionals on context with a custom Blade directive" %}
```php
// Binding
Blade::directive('ifPublic', function () {
    return "<?php if (app('context')->isPublic()): ?>";
});
// In use
@ifPublic
    &copy; Copyright MyApp LLC
@else
    &copy; Copyright {{ app('context')->client->name }}
@endif
```
{% endcode-tabs-item %}
{% endcode-tabs %}

由于解析为简单的if语句，我们仍可以依赖原生的@else和@endif条件。 但是如果我们想要的话，我们也可以创建一个自定义的@elseIfClient指令，或者单独的@ifClient指令，或者我们想要的任何其他指令



