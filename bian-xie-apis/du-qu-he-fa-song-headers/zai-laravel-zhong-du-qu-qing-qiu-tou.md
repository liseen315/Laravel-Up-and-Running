# 在Laravel中读取请求头

如果你获取了一个请求，也可以轻松的读取头。例如13-6所示

{% code-tabs %}
{% code-tabs-item title="Example 13-6. Reading a request header in Laravel" %}
```php
Route::get('dogs', function (Request $request) { 
    var_dump($request->header('Accept'));
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

现在你已经可以读取请求头以及设置响应头，让我们看看如何自定义API

