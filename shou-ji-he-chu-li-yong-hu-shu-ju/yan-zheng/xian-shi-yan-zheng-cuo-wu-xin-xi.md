# 显示验证错误信息

我们已经在第6章中介绍了其中的大部分内容，但是这里有一个关于如何显示验证错误的快速回顾

请求上的validate\(\)方法（以及它依赖的重定向上的withErrors\(\)方法）会将任何错误闪烁到会话中。 这些错误可供您在$errors变量中重定向到的视图使用。 请记住，作为Laravel魔法的一部分，每次加载视图时都会提供$errors变量，即使它只是空的，所以你不必用isset\(\)检查是否存在。

意味着你可以在页面上如示例7-16一样处理。

{% code-tabs %}
{% code-tabs-item title="Example 7-16. Echo validation errors" %}
```php
@if ($errors->any())
    <ul id="errors">
        @foreach ($errors->all() as $error)
        <li>{{ $error }}</li>
        @endforeach
    </ul>
@endif
```
{% endcode-tabs-item %}
{% endcode-tabs %}

