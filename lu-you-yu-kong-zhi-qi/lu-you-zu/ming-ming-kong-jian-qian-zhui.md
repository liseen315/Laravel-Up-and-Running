# 命名空间前缀

当按照子域或者是路由前缀进行分组时,他们的控制器可能有点类似PHP的命名空间,在仪表盘示例中,左右的仪表盘路由的控制器可能都位于仪表盘命名空间下,通过使用路由组命名空间前缀可以避免较长的控制器引用,如例子3-16所示

{% code-tabs %}
{% code-tabs-item title="Example 3-16. Route group namespace prefixes" %}
```php
// App\Http\Controllers\UsersController
Route::get('/', 'UsersController@index');
Route::namespace('Dashboard')->group(function () {
// App\Http\Controllers\Dashboard\PurchasesController Route::get('dashboard/purchases', 'PurchasesController@index');
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

