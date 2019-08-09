# 视图

截至目前，路由的知识已经讲了很多了,我们会在之前看到类似return view\('account'\)的代码,这又是什么?

在MVC模式\(图3-1\)中,视图\(或是模板\)是用于描述展示内容的文件,你可能看到过JSON,XML或者是电子邮件的视图,但是web框架中最常见的视图是HTML

在Laravel中有两种格式的视图,一种是纯PHP,一种是Blade模板,about.php文件使用php引擎渲染,about.blade.php使用Blade引擎渲染.

> 三种方式加载视图
>
> 有三种不用的方式返回视图,当前,你可以使用view\(\),你也可以使用View::make\(\)其实是一回事,或者你喜欢的话也可以使用注入Illuminate\View\ViewFactory

你可以像\(例子3-18\)使用view\(\)辅助函数返回了视图,如果视图不依赖控制器变量就可以正常工作了.

{% code-tabs %}
{% code-tabs-item title="Example 3-18. Simple view\(\) usage" %}
```php
Route::get('/', function () { 
    return view('home');
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

此代码在resources/views/home.blade.php or resources/views/ home.php查找视图,并解析内容,内联PHP以及控制结构后输出.一旦你返回它,它就会通过堆栈最终返回给用户.

但是如果你想要给视图传递变量呢？看例子3-19

{% code-tabs %}
{% code-tabs-item title="Example 3-19. Passing variables to views" %}
```php
Route::get('tasks', function () { 
    return view('tasks.index')
        ->with('tasks', Task::all());
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

闭包从resources/views/tasks/index.blade.php 或 resources/views/tasks/ index.php加载视图,然后传递了一个名为tasks的简单变量,并且包含了一个Task::all\(\)的方法,Task::all是一个额Eloquent 数据库查询方法,你将在第五章学习

