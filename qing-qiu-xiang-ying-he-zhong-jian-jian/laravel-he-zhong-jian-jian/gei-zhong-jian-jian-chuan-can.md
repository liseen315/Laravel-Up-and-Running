# 给中间件传参

这种情况并不常见，但有时您需要将参数传递给路由中间件。 例如，您可能有一个身份验证中间件，根据你是在保护成员用户类型还是所有者用户类型，它们的行为会有所不同。

```php
Route::get('company', function () { 
    return view('company.admin');
})->middleware('auth:owner');
```

要使其工作，您需要向中间件的handle\(\)方法添加一个或多个参数，并相应地更改该方法的逻辑，如示例10-24所示。

{% code-tabs %}
{% code-tabs-item title="Example 10-24. Defining a route middleware that accepts parameters" %}
```php
public function handle($request, $next, $role) {
    if (auth()->check() && auth()->user()->hasRole($role)) { 
        return $next($request);
    }
    return redirect('login'); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

请注意，还可以向handle\(\)方法添加多个参数，并通过用逗号分隔多个参数来将其传递给路由定义：

```php
Route::get('company', function () { 
    return view('company.admin');
})->middleware('auth:owner,view');
```

> 表单请求对象
>
> 在本章中，我们介绍了如何注入一个Illuminate请求对象，它是最基本和最常见的请求对象。
>
> 但是，您也可以扩展Request对象并注入它。 您将在第11章中了解有关如何绑定和注入自定义类的更多信息，但是有一种特殊类型，称为表单请求，它有自己的一组行为
>
> 有关创建和使用表单请求的详细信息，请参阅第194页的“表单请求”。

