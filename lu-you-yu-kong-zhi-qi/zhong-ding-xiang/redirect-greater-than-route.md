# redirect\(\)-&gt;route\(\)

route\(\)方法与to\(\)方法相同，但它不是指向特定路径，而是指向特定的路由名称（参见示例3-40）。

{% code-tabs %}
{% code-tabs-item title="Example 3-40. redirect\(\)->route\(\)" %}
```php
Route::get('redirect', function () {
    return redirect()->route('conferences.index');
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

请注意,有一些路由名字需要参数,参数的顺序有点不同,route\(\)有一个可选的第二个参数用于传递参数。

`function route($to = null, $parameters = [], $status = 302, $headers = [])`

所以,使用的例子如下3-41:

{% code-tabs %}
{% code-tabs-item title="Example 3-41. redirect\(\)->route\(\) with parameters" %}
```php
Route::get('redirect', function () {
    return redirect()->route('conferences.show', ['conference' => 99]);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

