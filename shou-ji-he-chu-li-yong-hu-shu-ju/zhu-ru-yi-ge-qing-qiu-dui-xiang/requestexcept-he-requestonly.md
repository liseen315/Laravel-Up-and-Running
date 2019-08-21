# $request-&gt;except\(\)和$request-&gt;only\(\)

$request-&gt;except\(\)提供与$request-&gt;all\(\)相同的输出，但您可以选择一个或多个要排除的字段，例如\_token。您可以给它传递给字符串或字符串数组。

示例7-2显示了$request-&gt;except\(\)与示例7-1的相同表单的使用情况

{% code-tabs %}
{% code-tabs-item title="Example 7-2. $request->except\(\)" %}
```php
Route::post('post-route', function (Request $request) { 
    var_dump($request->except('_token'));
});
// Outputs:
/**
*[
* 'firstName' => 'value',
* 'utm' => 12345 *]
*/
```
{% endcode-tabs-item %}
{% endcode-tabs %}

$request-&gt;only\(\) 与 $request-&gt;except\(\)相反，如示例7-3

{% code-tabs %}
{% code-tabs-item title="Example 7-3. $request->only\(\)" %}
```php
Route::post('post-route', function (Request $request) { 
    var_dump($request->only(['firstName', 'utm']));
});

// Outputs:
/** *[
* 'firstName' => 'value',
* 'utm' => 12345 *]
*/

```
{% endcode-tabs-item %}
{% endcode-tabs %}

