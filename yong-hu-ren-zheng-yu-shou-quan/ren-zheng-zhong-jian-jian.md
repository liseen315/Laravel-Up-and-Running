# 认证中间件

在例9-3中，您了解了如何检查访问者是否已登录，如果没有则重定向。 您可以在应用程序的每个路径上执行这些类型的检查，但很快就会变得乏味。 事实证明，路由中间件（请参阅第10章了解有关它们如何工作的更多信息）非常适合限制访客或经过身份验证的用户的某些路由。

Laravel为我们提供了现成的中间件，可以在App\Http\Kernel查看提供了哪些中间件。

```php
protected $routeMiddleware = [
    'auth' => \App\Http\Middleware\Authenticate::class,
    'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
    'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
    'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
    'can' => \Illuminate\Auth\Middleware\Authorize::class,
    'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
    'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
    'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
    'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,
];
```

四个默认路由中间件与认真相关：

_auth_

限制对经过身份验证的用户的路由访问

_auth.basic_

使用基于HTTP限制对经过身份验证的用户的访问

_guest_

限制对未经身份验证的用户的访问。

_can_

通常用户认证用于需要认证的部分，认证过的用户不需要看到\(类似登录表单\)，auth.basic是一个不太常用的中间件。

示例9-8显示了一些受auth中间件保护的路由的示例

{% code-tabs %}
{% code-tabs-item title="Example 9-8. Sample routes protected by auth middleware" %}
```php
Route::middleware('auth')->group(function () { 
    Route::get('account', 'AccountController@dashboard');
});
Route::get('login', 'Auth\LoginController@getLogin')->middleware('guest');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

