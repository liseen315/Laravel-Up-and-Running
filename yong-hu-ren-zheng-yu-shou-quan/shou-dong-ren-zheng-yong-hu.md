# 手动认证用户

用户认证的常见情况是，你允许用户提供他们的凭证，然后使用auth\(\)-&gt;attempt\(\)查看提供的凭证是否匹配真实用户，如果通过，则可以登录。

但有时会出现这样的情况，即你可以选择自己登录用户。 例如，你可能希望允许管理员用户切换用户。

有四种方法可以实现这一点。首先，你只需传递一个用户ID：

```php
auth()->loginUsingId(5);
```

其次，你可以传递User对象（或者任何实现Illuminate\Contracts\Auth\Authenticatable契约的对象）。

```php
auth()->login($user);
```

第三和第四，您可以选择仅针对当前请求对给定用户进行身份验证，这不会影响您的会话或cookie，使用once\(\)或onceUsingId\(\)

```php
auth()->once(['username' => 'mattstauffer']); 
// or
auth()->onceUsingId(5);
```

请注意，传递给once\(\)方法的数组可以包含任何键/值对，以唯一标识您要进行身份验证的用户。 如果它适合您的项目，您甚至可以传递多个键和值。 例如

```php
auth()->once([
    'last_name' => 'Stauffer',
    'zip_code' => 90210,
])
```

