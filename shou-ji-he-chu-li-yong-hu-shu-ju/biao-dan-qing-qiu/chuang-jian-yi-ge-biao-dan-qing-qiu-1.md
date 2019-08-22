# 使用表单请求

既然我们已经创建了表单请求,那么如何使用它呢？在Laravel中可以将创建的请求作为参数传递给闭包或者是控制器方法。

让我们看一下示例7-18

{% code-tabs %}
{% code-tabs-item title="Example 7-18. Using a form request" %}
```php
Route::post('comments', function (App\Http\Requests\CreateCommentRequest $request) { 
    // Store comment
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

它验证用户输入和请求认证，如果输入无效，则像Request的validate\(\)一样，将用户重定向到上一页，保留其输入并传递错误消息，如果用户未经授权，Laravel将返回403禁止错误，不执行路由代码。

