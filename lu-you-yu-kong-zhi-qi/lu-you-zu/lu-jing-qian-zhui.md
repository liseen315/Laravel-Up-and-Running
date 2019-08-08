# 路径前缀

例如,有一组路由共享一段路径,如站点的仪表盘\(/dashboard\),则可以使用路由组简化\(参加示例3-13\)

{% code-tabs %}
{% code-tabs-item title="Example 3-13. Prefixing a group of routes" %}
```php
Route::prefix('dashboard')->group(function () {
    Route::get('/', function () {
        return 'root';
    });
    Route::get('users', function () {
        return 'users';
    });
});

Route::group(["prefix" => 'dashboard'], function () {
    Route::get('/', function () {
        return 'root';
    });
    Route::get('users', function () {
        return 'users';
    });
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

注意前缀组的/就是前缀的名字,如示例/就是dashborad

