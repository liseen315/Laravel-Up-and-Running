# API 资源控制器

当您使用RESTful API时，资源上的操作列表与HTML资源控制器上的列表不同。 例如，您可以向API发送POST请求以创建资源，但您无法真正在API中“显示创建表单”

Laravel 5.6引入了一种生成API资源控制器的新方法，该控制器具有与资源控制器相同的结构，但不包括创建和编辑操作。 我们可以通过在创建控制器时传递--api标志来生成API资源控制器

```bash
php artisan make:controller MySampleResourceController --api
```

绑定API资源控制器

使用apiResource\(\)方法替代resource\(\)来进行API资源绑定.如例子3-29

{% code-tabs %}
{% code-tabs-item title="Example 3-29. API resource controller binding" %}
```php
// routes/web.php
Route::apiResource('tasks', 'TasksController');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

