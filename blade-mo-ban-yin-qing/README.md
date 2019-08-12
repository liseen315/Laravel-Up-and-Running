# Blade 模板引擎

与大多数其他后端语言相比，PHP实际上作为模板语言运行得相对较好。 但它有它的缺点，就是遍地使用&lt;？php 内联,所以你期待一个提供模板语言的时髦框架.

Laravel提供了一个名为Blade的定制模板引擎，它受到.NET的Razor引擎的启发。它拥有简洁的语法、较浅的学习曲线、强大而直观的继承模型和易于扩展的特性。

快速查看一下如下例子,示例4-1

{% code-tabs %}
{% code-tabs-item title="Example 4-1. Blade samples" %}
```php
<h1>{{ $group->title }}</h1>
{!! $group->heroImageHtml() !!}
@forelse ($users as $user)
    • {{ $user->first_name }} {{ $user->last_name }}<br>
@empty
    No users in this group.
@endforelse
```
{% endcode-tabs-item %}
{% endcode-tabs %}

正如您所看到的，Blade使用花括号作为“echo”，并引入了一种约定，以@为前缀的定义标记（称为“指令”），你可以使用指令控制结构,用于继承以及你想要添加的自定义功能.

Blade语法简介明了,核心更是优雅整洁,与此同时,当在模板内涉及嵌套,继承,复杂结构使用Blade更是得心应手,它可以完成复杂的应用需求，并使得这一切用于易于使用

另外,所有的Blade语法都会编译成PHP代码并缓存起来,它是非常快的,你也可以在其中混入PHP代码,但是我并不推荐混写PHP,通常如果用Blade模板或者是自定义指令无法完成的事,这件事就不应该属于模板

> 在Laravel内使用Twig
>
> 与其他基于Symfony的框架不用,Laravel默认不使用Twig,如果你想用Twig，有一个[Twig](https://github.com/rcrowe/TwigBridge)包用于替代Blade



