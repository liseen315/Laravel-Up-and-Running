# 视图Composer和服务注入

正如我们在第3章中所讨论的，从路由定义向视图传递数据很简单（参见示例4-20）。

{% code-tabs %}
{% code-tabs-item title="Example 4-20. Reminder of how to pass data to views" %}
```php
Route::get('passing-data-to-views', function () { 
        return view('dashboard')
        ->with('key', 'value');
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

有时你会发现你会将相同的数据一遍又一遍的传递给多个视图,或者你使用数据的一部分,难道你必须在每个路由定义中传递数据,然后使用数据么？

