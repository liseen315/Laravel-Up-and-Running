# 授权中间件

 如果您想授权路由，可以使用授权中间件（它有一个can的快捷方式），如示例9-19所示。

{% code-tabs %}
{% code-tabs-item title="Example 9-19. Using the Authorize middleware" %}
```php
Route::get('people/create', function () { 
    // Create a person
})->middleware('can:create-person');

Route::get('people/{person}/edit', function () { 
    // Edit person
})->middleware('can:edit,person');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这里，{person}参数（无论是定义为字符串还是绑定路由模型）将作为附加参数传递给方法。

示例9-19是正常用法,第二种是策略用法,我们会在第247页讨论"策略"。

如果你需要检查动作则不需要一个模型实例\(例如，创建，不像编辑，不需要传递实际的模型实例\)你只需要传递一个类名。

```php
Route::post('people', function () { 
    // Create a person
})->middleware('can:create,App\Person');
```

