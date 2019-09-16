# 触发回退路由

如果要自定义当laravel捕获“未找到”异常时返回的路由，可以使用respondWithRoute\(\)方法更新异常处理程序，如示例13-43所示。

{% code-tabs %}
{% code-tabs-item title="Example 13-43. Calling the fallback route when “not found” exceptions are caught" %}
```php
// App\Exceptions\Handler
public function render($request, Exception $exception) {
    if ($exception instanceof ModelNotFoundException && $request->isJson()) { 
        return Route::respondWithRoute('api.fallback.404');
    }
    return parent::render($request, $exception); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

