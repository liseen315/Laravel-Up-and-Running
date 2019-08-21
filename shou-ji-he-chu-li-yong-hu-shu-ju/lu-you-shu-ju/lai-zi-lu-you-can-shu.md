# 来自路由参数

另一种获取URL数据的主要方法是从路由参数中获取数据，这些参数被注入到为当前路由服务的控制器方法或闭包中，如示例7-9所示。

{% code-tabs %}
{% code-tabs-item title="Example 7-9. Getting URL details from route parameters" %}
```php
// routes/web.php
Route::get('users/{id}', function ($id) {
    // If the user visits myapp.com/users/15/, $id will equal 15
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

学习更多路由和路由绑定,请查看第三章

