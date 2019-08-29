# 在控制器内使用和创建响应对象

在我们讨论如何自定义响应对象之前，让我们先后退一步，看看我们通常如何处理响应对象

最后，从路由定义返回的任何响应对象都将转换为HTTP响应。它可以定义特定的头或特定的内容、设置cookie或其他任何内容，但最终它将转换为用户浏览器可以解析的响应。

让我们看一下简单的响应，如示例10-4

{% code-tabs %}
{% code-tabs-item title="Example 10-4. Simplest possible HTTP response" %}
```php
Route::get('route', function () {
    return new Illuminate\Http\Response('Hello!');
});
// Same, using global function:
Route::get('route', function () { 
    return response('Hello!');
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

我们创建了一个响应，给了一些数值，然后返回了他们。我们还可以自定义HTTP状态，头，cookies以及更多，如示例10-5。

{% code-tabs %}
{% code-tabs-item title="Example 10-5. Simple HTTP response with customized status and headers" %}
```php
Route::get('route', function () { 
    return response('Error!', 400)
        ->header('X-Header-Name', 'header-value')
        ->cookie('cookie-name', 'cookie-value');
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**设置头**

我们在响应上使用header\(\)设置了响应头,如示例10-5，第一个参数是头的名字，第二个是头的值。

**添加cookies**

我们还可以在响应上设置cookie。我们会在第14章详细介绍，如示例10-6我们可以轻松的在响应上设置cookie。

{% code-tabs %}
{% code-tabs-item title="Example 10-6. Attaching a cookie to a response" %}
```php
return response($content) ->cookie('signup_dismissed', true);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

