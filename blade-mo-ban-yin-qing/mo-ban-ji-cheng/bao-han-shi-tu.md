# 包含视图

现在我们已经建立了基础的继承结构,接下来我们可以使用更多的技巧了.

_**@include**_

如果我们在一个视图内想嵌入另外的视图怎么做呢?也许我们有个"注册按钮"想在整个站点内使用,或许我们想在我们每次使用这个按钮的时候都可以进行自定义呢?请看示例4-10

{% code-tabs %}
{% code-tabs-item title="Example 4-10. Including view partials with @include" %}
```text
<!-- resources/views/home.blade.php -->
<div class="content" data-page-name="{{ $pageName }}">
    <p>Here's why you should sign up for our app: 
        <strong>It's Great.</strong>
    </p>
    @include('sign-up-button', ['text' => 'See just how great it is'])
</div>
<!-- resources/views/sign-up-button.blade.php -->
<a class="button button--callout" data-page-name="{{ $pageName }}">
    <i class="exclamation-icon"></i> 
    {{ $text }}
</a>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

@include引入视图并可以给它传递数据,注意你不但可以通过@include第二个参数显示的传递数据,也可以通过包含可用变量传递\(例如$pageName\)，但是为了清晰,我建议总是显示的传递变量.

你也可以使用@includeIf,@includeWhen,以及@includeFirst指令,如例子4-11所示

{% code-tabs %}
{% code-tabs-item title="Example 4-11. Conditionally including views" %}
```php
{{-- Include a view if it exists --}}
@includeIf('sidebars.admin', ['some' => 'data'])
{{-- Include a view if a passed variable is truth-y --}}
@includeWhen($user->isAdmin(), 'sidebars.admin', ['some' => 'data'])
{{-- Include the first view that exists from a given array of views --}}
@includeFirst(['customs.header', 'header'], ['some' => 'data'])
```
{% endcode-tabs-item %}
{% endcode-tabs %}

_**@each**_

有些情况你希望循环include某些模板,这就是@each要干的事.

假设我们有一个由模块组成的侧边栏，我们希望包含多个模块，每个模块都有不同的标题。请看示例4-12

{% code-tabs %}
{% code-tabs-item title="Example 4-12. Using view partials in a loop with @each" %}
```php
<!-- resources/views/sidebar.blade.php -->
<div class="sidebar">
@each('partials.module', $modules, 'module', 'partials.empty-module')
</div>
<!-- resources/views/partials/module.blade.php -->
<div class="sidebar-module"> <h1>{{ $module->title }}</h1>
</div>
<!-- resources/views/partials/empty-module.blade.php -->
<div class="sidebar-module"> No modules :(
</div>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

回顾一下@each语法。 第一个参数是视图部分的名称。 第二个是迭代的数组或集合。 第三个是变量名，每个项（在本例中，$ modules数组中的每个元素）将传递给视图。 并且可选的第四个参数用于显示数组或集合是否为空的视图（或者，可选地，您可以在此处传递将用作模板的字符串）

