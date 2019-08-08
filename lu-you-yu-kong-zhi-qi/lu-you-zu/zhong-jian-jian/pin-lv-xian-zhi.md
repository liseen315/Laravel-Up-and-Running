# 频率限制

如果你需要限制在给定时间范围内访问路由的次数限制\(称为频率限制,常见用于API\),那么在5.2或者更高版本有一个现成的中间件.它就是throttle,它有两个参数,第一个是允许用户尝试访问的次数,第二个是重置尝试计数钱等待的分钟数,示例3-12演示了其用法:

{% code-tabs %}
{% code-tabs-item title="Example 3-12. Applying the rate limiting middleware to a route" %}
```php
Route::middleware('auth:api','throttle:60,1')->group(function () {
    Route::get('/profile', function () {
        return 'profile';
    });
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

动态频率限制

如果您想区分一个用户的速率限制，可以指示throttle中间件从用户的Eloquent模型中提取尝试次数（第一个参数）。 而不是将计数作为throttle的第一个参数传递，而是在Eloquent模型上传递属性的名称，该属性将用于计算用户是否已超过其速率限制。 因此，如果您的用户模型上有一个plan\_rate\_limit属性，则可以使用带有throttle的中间件：plan\_rate\_limit，1。

> Eloquent简介
>
> 我们会在第5章详细讲解Eloquent,现在简单的介绍一下,Eloquent是Laravel的对象关系映射\(ORM\),它可以轻松的将Post模型与Post表关联起来,并通过Post:all\(\)获取全部数据
>
> 查询builder是一个工具,可以用Post::where\('active', true\)-&gt;get\(\)或者 DB::table\('users'\)-&gt;all\(\)来查询数据.

