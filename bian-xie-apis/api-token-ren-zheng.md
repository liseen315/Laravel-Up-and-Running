# API Token认证

Laravel 提供了一个简单的API Token认证机制，这与username与password没什么不同：有一个单一的token分配给每个用户，客户端可以传递一个请求来验证该用户的请求。

API令牌机制并不像OAuth 2.0那样安全，因此在决定使用它之前，请确保您知道它适合您的应用程序。 因为只有一个令牌，它几乎就像一个密码 - 如果有人获得该令牌，他们就可以访问您的整个系统。 同时，它也可以安全，因为你可以强制令牌不容易被猜测出来，并且你可以在最小的代价下擦除和重置令牌，而这是密码无法做到的。

因此，令牌API身份验证可能不适合您的应用; 但如果是的话，实施起来就不会那么简单了。

首先，在users表中添加一个60个字符的唯一api\_token列:

```php
$table->string('api_token', 60)->unique();
```

接着，更新创建新用户的方法，并确保它为每个新用户为此字段设置一个值，Laravel有一个助手用于生成随机字符串，因此如果您想使用它，只需为每个字符串将字段设置为str\_random（60）。如果要将此添加到实时应用程序中，还需要为先前存在的用户执行此操作。

想要用认证包装路由，请使用auth:api路由中间件，如13-41所示

{% code-tabs %}
{% code-tabs-item title="Example 13-41. Applying the API auth middleware to a route group" %}
```php
Route::prefix('api')->middleware('auth:api')->group(function () { 
    //
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

请注意，由于您使用的是除标准防护之外的身份验证防护，因此您需要在使用auth\(\)方法时指定防护.

```php
$user = auth()->guard('api')->user();
```

