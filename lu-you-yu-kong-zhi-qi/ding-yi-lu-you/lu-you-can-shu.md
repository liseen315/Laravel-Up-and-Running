# 路由参数

如果你定义的路由在URL中有可变的参数,那么在路由中定义他们并传递给闭包是非常简单的\(参见示例3-5\)

{% code-tabs %}
{% code-tabs-item title="Example 3-5. Route parameters" %}
```php
Route::get('users/{id}/friends',function ($id) {
    return $id;
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

你可以在路由参数后面添加"?"使得该参数变成可选参数,如示例3-6,在这种情况下还应该为路由对应的变量提供默认值.

{% code-tabs %}
{% code-tabs-item title="Example 3-6. Optional route parameters" %}
```php
Route::get('users/{id?}',function ($id='fallbackId') {
    return $id;
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

而且，您可以使用正则表达式（regex）来定义路由只在在参数满足特定要求时匹配，如示例3-7所示。

```php
Route::get('users/{id}',function ($id) {
    return $id;
})->where('id','[0-9]+');
```

