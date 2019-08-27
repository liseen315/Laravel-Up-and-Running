# 创建一个自定义用户提供者

如下的守卫定义在config/auth.php中。然后一个auth.providers的块用于定义可用的提供者。让我们创建一个名为trainees的守卫。

```php
 'providers' => [
    'users' => [
        'driver' => 'eloquent',
        'model' => App\User::class,
    ],
    'trainees' => [
        'driver' => 'eloquent',
        'model' => App\Trainee::class,
    ], 
],
```

驱动的两个选项是eloquent和database。如果使用Eloquent，你需要一个模型属性来包含Eloquent类名，如果你使用database,则需要一个表属性来定义应针对哪个表进行身份验证。

在我们的例子中，你可以看到应用有一个User和一个学员，他们需要分别认证，这样代码可以区分为auth\(\)-&gt;guard\('users'\) 和 auth\(\)-&gt;guard\('trainees'\)。

最后一个注意事项：auth路由中间件可以接受一个参数，即守卫名称。所以，你可以用一个特定的守卫来守卫某些路由。

```php
Route::middleware('auth:trainees')->group(function () { 
    // Trainee-only routes here
});
```

