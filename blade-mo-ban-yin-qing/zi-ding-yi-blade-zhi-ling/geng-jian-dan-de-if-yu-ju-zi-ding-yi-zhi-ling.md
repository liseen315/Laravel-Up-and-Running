# 更简单的if语句自定义指令

虽然Blade指令功能强大,但是通常都用在if语句上,所以有一种更简单的方法创建自定义if指令Blade::if\(\),示例4-33显示了如何用Blade::if\(\)重构示例4-32

{% code-tabs %}
{% code-tabs-item title="Example 4-33. Defining a custom “if ” Blade directive" %}
```php
// Binding
Blade::if('ifPublic', function () { 
    return (app('context'))->isPublic();
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

您可以以完全相同的方式使用这些指令，但是正如您所看到的，这更简单一些。不必手动键入php大括号，只需编写一个返回布尔值的闭包。

