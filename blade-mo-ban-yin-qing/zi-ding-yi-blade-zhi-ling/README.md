# 自定义Blade指令

我们已经了解了许多Blade内建语法,@if，@unless,以及更多都被称为指令,每个Blade指令都是一种模式\(例如@if \($condition\)\)然后会编译成PHP输出\(&lt;?php if\($condition\) ?&gt;\)

你也可以手动创建指令,你可以认为指令是大量代码的一种简写方式,例如`@button('buttonName')`会被编译成一大段关于button的HTML代码,但是对于像这样的简单代码扩展，你最好包括一个视图部分

自定义指令往往用于简化某些重复逻辑,假设我们已经厌倦了不停的输入@if\(auth-&gt;guest\(\)\)\(用于检测用户是否登录\),然后我们就可以自定义一个类似@ifGuest的指令,与视图composer一样,使用自定义的服务提供者注册比较好,我们将其放到App\Providers\AppServiceProvider的boot方法内,如示例4-29所示.

{% code-tabs %}
{% code-tabs-item title="Example 4-29. Binding a custom Blade directive in a service provider" %}
```php
public function boot() {
    Blade::directive('ifGuest', function () { 
        return "<?php if (auth()->guest()): ?>";
    }); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

现在我们注册了一个自定义指令用于代替&lt;?php if \(auth\(\)-&gt;guest\(\)\): ?&gt;PHP代码

这感觉有点奇怪，你正在编写一个作为PHP返回的字符串,但这意味着你可以将PHP模板语法的丑陋,复杂,不清晰等问题隐藏在一个更清晰的语法背后。

> 自定义指令结果缓存
>
> 您可能想要通过在绑定中执行操作然后将结果嵌入返回的字符串中来做一些逻辑以使您的自定义指令更快

```text
Blade::directive('ifGuest', function () { 
    // Antipattern! Do not copy. 
    $ifGuest = auth()->guest();
    return "<?php if ({$ifGuest}): ?>";
});
```

> 这个想法的问题在于它假定在每个页面加载时都会重新创建该指令。 但是，Blade会积极地进行缓存，因此如果您尝试这样做，您将发现自己陷入了困境

