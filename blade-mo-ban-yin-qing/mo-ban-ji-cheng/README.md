# 模板继承

Blade提供了一个结构用于模板继承,它允许视图继承,修改以及包含其他视图.

让我们看看使用Blade如何创建继承结构.

使用@section/@show和@yield定义节点

让我们从顶级Blade布局开始,如示例4-8所示，它是一个通用的包装器定义,随后我们可以将特定的页面内容放置其中.

{% code-tabs %}
{% code-tabs-item title="Example 4-8. Blade layout" %}
```text
<!-- resources/views/layouts/master.blade.php -->
<html>
<head>
<title>My Site | @yield('title', 'Home Page')</title> 
</head>
<body>
    <div class="container">
        @yield('content')
    </div>
    @section('footerScripts')
        <script src="app.js"></script> 
    @show
</body>
</html>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这看起来跟普通的HTML页面类似,但是你也可以看到我们在两个地方进行了占位\(title和content\)然后我们在\(footerScripts\)放置了个节点,我们使用了三个Blade指令:单独的@yield\('content'\),带默认值的@yield\('title','Home page'\),以及一个显示实际内容的@section/@show

它们三个看起来不同,但是功能基本相同,它们都定义了给定名字的节点以便后续拓展,并且定义了如果不进行拓展,该怎么做.它们提供一个字符串作为默认值,如果没有默认值并不被继承就不显示任何东西,或者是显示一整块内容\(&lt;script src="app.js"&gt;&lt;/script&gt;\)

它们有什么不同?好吧，显然@yield\('content'\)没有默认值,而@yield\('title'\)的默认值在它不被继承时则会显示.相反,如果它被继承,则继承的子节点不能通过编程访问默认值,另外,@section/@show既定义了默认值,有可以通过@parent将其默认值提供给子节点

一旦你有类似的父结构布局,则你可以像示例4-9在一个新模板内继承它

{% code-tabs %}
{% code-tabs-item title="Example 4-9. Extending a Blade layout" %}
```php
<!-- resources/views/dashboard.blade.php -->
@extends('layouts.master')
@section('title', 'Dashboard')
@section('content')
    Welcome to your application dashboard!
@endsection
@section('footerScripts')
    @parent
    <script src="dashboard.js"></script> 
@endsection
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> @show与@endsection对比
>
> 你可能注意到示例4-8使用了@section/@show而示例4-9使用了@section/@endsection，他们有什么不同？当你在parent定义节点时候使用@show,而在当你为子节点定义内容模板时请使用@endsection

_**@extends**_

在示例4-9中,我们使用@extends\('layouts.master'\)创建模板继承,此视图不会单独进行渲染，而是继承自另外一个视图,这意味着它的作用是定义各个部分内容,但是不能单独使用.它更像一系列内容的集合,而不是一个HTML页面

每一个文件应该只继承另外一个文件,@extends应该放在行首

_**@section和@endsection**_

我们使用@section\('title','Dashbord'\)为第一个节点定义了内容,因为内容很短,所以我们用了快捷方式来定义,虽然你没看到@section/@endsection可能有点小担心,但是不用焦虑,我们在第二个参数可以传递简单的内容.我们也可以使用普通语法来定义.

紧接着我们使用@section\('content'\)这种普通语法定义了内容节点,这里我们只是打了个招呼,但是请注意在子视图内请使用@section/@endsction包裹内容,而@show是用在父视图内的.

_**@parent**_

最后我们使用@section\('footerScript'\)普通语法来定义根脚本内容节点,但是请注意我们已经在master内定义了内容,这是我们面临两个选择,要么我们覆盖父视图内容,要么把父视图内容添加进来.

可以看到我们使用@parent将父视图内容的带到子视图内.如果我们不这么做，这个节点内容将完全覆盖父视图内的内容.





