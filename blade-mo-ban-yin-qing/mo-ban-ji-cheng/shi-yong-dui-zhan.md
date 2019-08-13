# 使用堆栈

通常使用基于Blade难于管理的情况是,当每个Blade包含层次结构的视图想要添加内容时-就像向数组添加一套内容一样.

最常见的情况是，某些页面（有时候，更广泛地说，网站的某些部分）需要加载的特定CSS和JavaScript文件。 想象一下，你的站点有一个全局的CSS文件,然后有一个包含作业CSS的文件,以及一个申请包含申请作业CSS的文件

Blade的堆栈就是用来解决这类问题的,在父类模板中定义个堆栈,它只是个占位符,然后，在每个子模板中，您可以使用@ push / @ endpush将条目“推送”到该堆栈，最终它们将会被渲染到堆栈尾.你也可以使用@prepend/@endprepend将条目放到堆栈首,如示例4-13

{% code-tabs %}
{% code-tabs-item title="Example 4-13. Using Blade stacks" %}
```php
<!-- resources/views/layouts/app.blade.php -->
<html>
<head><!-- the head --></head> 
<body>
<!-- the rest of the page -->
<script src="/css/global.css"></script>
<!-- the placeholder where stack content will be placed --> 
@stack('scripts')
</body>
</html>

<!-- resources/views/jobs.blade.php -->
@extends('layouts.app')
@push('scripts')
<!-- push something to the bottom of the stack -->
<script src="/css/jobs.css"></script> 
@endpush

<!-- resources/views/jobs/apply.blade.php -->
@extends('jobs')
@prepend('scripts')
<!-- push something to the top of the stack -->
<script src="/css/jobs--apply.css"></script> 
@endprepend
```
{% endcode-tabs-item %}
{% endcode-tabs %}

最终生成如下内容:

```markup
<html>
<head><!-- the head --></head> <body>
<!-- the rest of the page -->
<script src="/css/global.css"></script>
<!-- the placeholder where stack content will be placed --> 
<script src="/css/jobs--apply.css"></script>
<script src="/css/jobs.css"></script>
</body>
</html>
```

