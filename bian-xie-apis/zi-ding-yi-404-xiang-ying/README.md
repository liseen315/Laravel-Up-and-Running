# 自定义404响应

Laravel为普通HTML视图提供可自定义的错误消息页面，但您也可以为具有JSON内容类型的调用自定义默认的404回退响应。 为此，请向您的API添加Route::fallback\(\)调用，如例13-42所示

{% code-tabs %}
{% code-tabs-item title="Example 13-42. Defining a fallback route" %}
```php
// routes/api.php
Route::fallback(function () {
    return response()->json(['message' => 'Route Not Found'], 404);
})->name('api.fallback.404');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

