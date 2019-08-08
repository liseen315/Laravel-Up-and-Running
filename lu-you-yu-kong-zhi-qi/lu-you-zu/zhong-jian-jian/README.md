# 中间件

路由组最常见的用途可能是将中间件应用于一组路由。您将在第10章中了解更多关于中间件的内容，但除其他外，它们是Laravel用于认证用户和限制用户使用网站的某些部分的内容。

在示例3-11中，我们围绕仪表板和帐户视图创建了一个路由组，并将认证中间件应用于这两者。在本例中，这意味着用户必须登录到应用程序才能查看仪表板或帐户页。

{% code-tabs %}
{% code-tabs-item title="Example 3-11. Restricting a group of routes to logged-in users only" %}
```php
Route::middleware('auth') ->group(function () {
   Route::get('dashboard',function () {
      return 'dashboard';
   });
   Route::get('account',function () {
      return 'account';
   });
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> Laravel 5.4之前的路线组
>
> 就像在5.2之前Laravel中不存在Fluent路由定义一样，在5.4之前不可能将中间件、前缀、域等修饰符应用到路由组中

```php
Route::group(['middleware' => 'auth'], function () {
    Route::get('dashboard', function () {
        return view('dashboard');
    });
    Route::get('account', function () {
        return view('account');
    });
});
```

