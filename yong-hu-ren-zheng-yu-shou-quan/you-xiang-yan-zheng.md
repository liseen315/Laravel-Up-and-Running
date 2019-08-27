# 邮箱验证

Laravel 5.7引入了一个新特性，验证他们注册时的邮箱地址。

要启用邮箱验证，更新你的App\User类然后是其实现Illuminate\Contracts\Auth\MustVerifyEmail契约，如示例9-9所示

{% code-tabs %}
{% code-tabs-item title="Example 9-9. Adding the MustVerifyEmail trait to an Authenticatable model" %}
```php
class User extends Authenticatable implements MustVerifyEmail {
    use Notifiable; 
    // ...
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

users表必须包含一个非空的时间戳字段名为email\_verified\_at，在Laravel 5.7版本及之后版本迁移已经默认提供了该字段。

最后，你需要在控制器启用邮箱验证路由，最简单的方式是在路由文件使用Auth::routes\(\)，然后将verify参数设置为true。

```php
Auth::routes(['verify' => true]);
```

现在，您可以保护任何未经验证邮件地址的用户访问您的路由。

```php
Route::get('posts/create', function () { 
    // Only verified users may enter...
})->middleware('verified');
```

你可以自定义路由，在VerificationController验证后用户的重定向。

```php
protected $redirectTo = '/profile';
```

