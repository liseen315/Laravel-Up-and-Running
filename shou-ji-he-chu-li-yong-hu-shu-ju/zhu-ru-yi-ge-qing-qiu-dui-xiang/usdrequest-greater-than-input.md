# $request-&gt;input\(\)

然而$request-&gt;all\(\), $request-&gt;except\(\), 和 $request-&gt;only\(\)是对用户提供的完整输入数组进行操作。$request-&gt;input\(\)只获取单个字段的值。示例7-5提供了个示例，请注意第二个值是默认值，如果用户没输入值，这样可以容错。

{% code-tabs %}
{% code-tabs-item title="Example 7-5. $request->input\(\)" %}
```php
Route::post('post-route', function (Request $request) { 
    $userName = $request->input('name', 'Matt');
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

