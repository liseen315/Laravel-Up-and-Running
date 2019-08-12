# HTML 表单中的HTTP方法伪造

想让Laravel中的表单提交一个除POST以外的请求,请在表单中添加一个名为"PUT","PATCH"后者"DELETE"的隐藏变量,Laravel将匹配并路由表单提交，就好像它实际上是一个带有该verb的请求

如示例3-35，它将传递Laravel的"DELETE"，所以它将匹配Route::delete路由,而不使用Route::post\(\)匹配

{% code-tabs %}
{% code-tabs-item title="Example 3-35. Form method spoofing" %}
```php
<form action="/tasks/5" method="POST">
    <input type="hidden" name="_method" value="DELETE"> <!-- or: -->
    @method('DELETE')
</form>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

