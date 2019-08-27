# 更改默认守卫

守卫定义在config/auth.php中，你可以更改它们然后添加新的守卫，默认守卫也定义在此。对于它的价值，这是一种相对不常见的配置; 大多数Laravel应用程序只使用一个守卫。

在使用认证功能时会使用默认守卫除非你指定特别的守卫。例如auth\(\)-&gt;user\(\)将使用默认的守卫来提取已验证的用户。你可以通过更改config/auth.php来更改auth.defaults.guard设置。

```php
'defaults' => [
    'guard' => 'web', // Change the default here 
    'passwords' => 'users',
],
```

如果您使用的是Laravel5.1，您会注意到身份验证信息的结构与此稍有不同。别担心，所有的功能都是一样的，只是结构不同而已。

> 配置约定
>
> 你可能注意到我们使用auth.defaults.guard引用配置，在config/auth.php中应该有一个名为guard的键。

