# 手动登出用户

如果你需要手动登出用户，只需要调用logout\(\)

```php
auth()->logout();
```

**其他设备上的无效会话**

如果您不仅想注销用户当前的会话，还想注销任何其他设备上的会话，则需要提示用户输入密码，并将其传递给logoutOtherDevices\(\)方法（在Laravel 5.6及更高版本中提供）。为了做到这一点，您必须在app\Http\Kernel.php中将AuthenticateSession中间件添加到您的web组中（默认情况下已注释掉）

```php
'web' => [ // ...
    \Illuminate\Session\Middleware\AuthenticateSession::class,
],
```

然后您可以在任何需要的地方使用它：

```php
auth()->logoutOtherDevices($password);
```

