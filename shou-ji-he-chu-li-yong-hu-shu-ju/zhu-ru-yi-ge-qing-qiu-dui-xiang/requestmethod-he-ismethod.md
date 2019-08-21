# $request-&gt;method\(\) 和-&gt;isMethod\(\)

$request-&gt;method\(\)返回请求 HTTP verb，$request-&gt;isMethod\(\)检查verb 是否匹配。如示例7-6

{% code-tabs %}
{% code-tabs-item title="Example 7-6. $request->method\(\) and $request->isMethod\(\)" %}
```php
$method = $request->method();
if ($request->isMethod('patch')) {
    // Do something if request method is PATCH
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

