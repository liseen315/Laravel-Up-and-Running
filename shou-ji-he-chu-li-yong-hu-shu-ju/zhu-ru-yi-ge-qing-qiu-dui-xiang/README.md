# 注入一个请求对象

在Laravel中，访问用户数据的最常见工具是注入一个Illuminate\Http\Request对象实例。它可以方便的访问用户提供的数据:POST的表单数据或者JSON,GET请求\(查询参数\)，和URL段

> 访问请求对象的其他操作方式
>
> 有一个request\(\)全局辅助函数和一个Request facade,他们都公开了相同的方法，这些操作公开了整个Illuminate请求对象，但是现在我只讨论用户数据相关的方法。

一旦我们注入一个请求对象，让我们看下如何获取$request对象

```php
Route::post('form', function (Illuminate\Http\Request $request) { 
    // $request->etc()
});
```

