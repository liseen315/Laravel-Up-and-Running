# 名称前缀

路由名称通常会反映路径元素的继承链，因此users / comments / 5将由名为users.comments.show的路由提供服务

就像我们可以为URL和控制器命名空间加前缀一样，我们也可以为路由名称加前缀。使用路由组名称前缀，我们可以定义该组中的每个路由都可以在其名称前面添加一串字符。\(示例3-17\)我们将在每个路由名称前加上“users”，然后加上“comments”

{% code-tabs %}
{% code-tabs-item title="Example 3-17. Route group name prefixes" %}
```php
Route::name('users.')->prefix('users')->group(function () {
    Route::name('comments.')->prefix('comments')->group(function () {
        Route::get('{id}', function ($id) {
            return $id;
        })->name('show');
    });
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}



