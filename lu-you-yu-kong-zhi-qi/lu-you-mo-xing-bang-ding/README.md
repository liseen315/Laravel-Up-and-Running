# 路由模型绑定

最常见的路由模式之一是控制器方法的第一行都试图找到具体ID的资源，如例3-31所示

{% code-tabs %}
{% code-tabs-item title="Example 3-31. Getting a resource for each route" %}
```php
Route::get('conferences/{id}', function ($id) { 
    $conference = Conference::findOrFail($id);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Laravel提供了一种简化此模式的功能，称为路由模型绑定。 这允许您定义一个特定的参数名称（例如，{conference}）并指示路由解析器查找具有该ID的Eloquent数据库记录，然后把它替代ID作为参数继续传递.

路由模型绑定有两种：隐式和自定义（或显式）。



