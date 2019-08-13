# 自定义指令传参

如果你想在自定义逻辑内接收参数应该怎么办?请查看示例4-30

{% code-tabs %}
{% code-tabs-item title="Example 4-30. Creating a Blade directive with parameters" %}
```php
// Binding
Blade::directive('newlinesToBr', function ($expression) { 
    return "<?php echo nl2br({$expression}); ?>";
});
// In use
<p>@newlinesToBr($message->body)</p>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

闭包内的$expression接收外部传递的内容，然后用php输出了内容.

> Laravel 5.3之前的$expression参数作用域
>
> 在laravel 5.3之前，$expression参数还包括括号本身。所以，在示例4-30中，$expression（在Laravel 5.3和更高版本中是$message-&gt;body）应该是（$message-&gt;body），而我们必须写&lt;？php echo nl2br$expression？&gt;

如果你发现你在不停的写重复逻辑,你应该考虑使用自定义指令

