# 直接使用Route::view\(\)返回简单路由

因为通常路由需要返回不带数据的视图,Laravel5.5+允许你定义不需要定义闭包或者是控制器/方法的路由,如例子3-20

{% code-tabs %}
{% code-tabs-item title="Example 3-20. Route::view\(\)" %}
```php
// Returns resources/views/welcome.blade.php
Route::view('/', 'welcome');
// Passing simple data to Route::view()
Route::view('/', 'welcome', ['User' => 'Michael']);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

