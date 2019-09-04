# 在Laravel中发送响应头

我们已经在第10章介绍了一些，让我们回顾一下，一旦你有一个响应对象，你可以使用header添加头\($headerName,$headerValue\)，如示例13-5

{% code-tabs %}
{% code-tabs-item title="Example 13-5. Adding a response header in Laravel" %}
```php
Route::get('dogs', function () {
    return response(Dog::all())
        ->header('X-Greatness-Index', 12);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

轻松跟简单吧

