# 定义路由

在Laravel中,routes/web.php定义web路由,routes/api.php定义API路由,web路由用于终端用户访问,API路由用于你的API,现在我们主要看web路由

{% hint style="info" %}
Laravel 5.3版本路由文件

5.3版本的Laravel只有一个路由文件,它位于app/Http/routes.php
{% endhint %}

定义路由最简单的方式是用"/"加闭包函数,看例子3-1

{% code-tabs %}
{% code-tabs-item title="routes/web.php" %}
```php
<?php

Route::get('/',function () {
    return 'Hello World!';
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> 什么是闭包
>
> PHP内的匿名函数又称闭包,闭包是一个函数，可以作为对象传递、分配给变量、作为参数传递给其他函数和方法，甚至可以序列化

如果访问/\(根域名\),Laravel的路由会执行定义的闭包函数,然后返回结果,注意我们需要return我们的结果,不要使用echo或者是print

许多简单的网站可以完全在Web路由文件中定义。如示例3-2所示，通过一些简单的GET路径结合一些模板，您可以轻松地为经典网站提供服务。

> 中间件简介
>
> 你可能会想为什么要用return来代替echo,答案有很多,但是最简单的是Laravel对于请求跟响应进行了包装,包括一些称为中间件的东西,当路由关闭或控制器方法完成后，还没有时间将输出发送到浏览器；return内容允许它在返回给用户之前继续流过响应堆栈和中间件。

```php
Route::get('/', function () { return view('welcome');
});
Route::get('about', function () { return view('about');
});
Route::get('products', function () { return view('products');
});
Route::get('services', function () { return view('services');
});
```

> 译者注:你需要在resouse目录下的views文件夹建立对应的视图文件模板,比如welcome.blade.php



> 如果你有丰富的PHP开发经验,你可能会对Route调用静态方法感到吃惊,实际上这不是真的在调用静态方法,而是Laravel的facades,我们会在第十一章介绍
>
> 如果想绕过facade,你可以使用如下方法:

```php
$router->get('/',function () {
    return 'Hello';
});
```

