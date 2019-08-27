# 非关系数据库的自定义用户提供者

刚才描述的用户提供者创建流程仍然依赖于UserProvider类，这意味着它期望从关系数据库中提取标识信息。 但是如果你使用Mongo或Riak或类似的东西，你实际上需要创建自己的类。

要执行此操作，你需要创建一个实现Illuminate\Contracts\Auth \UserProvider接口的类，然后将其绑定到AuthServiceProvider@boot

```php
auth()->provider('riak', function ($app, array $config) {
    // Return an instance of Illuminate\Contracts\Auth\UserProvider... 
    return new RiakUserProvider($app['riak.connection']);
});
```

