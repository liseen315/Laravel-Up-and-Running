# $request-&gt;has\(\)

使用$request-&gt;has\(\)，您可以检测特定的用户输入是否对您可用。请查看示例7-4，其中包含之前URL的utm查询字符串参数

{% code-tabs %}
{% code-tabs-item title="Example 7-4. $request->has\(\)" %}
```php
// POST route at /post-route
if ($request->has('utm')) { 
    // Do some analytics work
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

