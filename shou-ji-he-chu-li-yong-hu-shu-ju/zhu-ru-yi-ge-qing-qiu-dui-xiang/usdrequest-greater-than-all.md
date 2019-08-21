# $request-&gt;all\(\)

就像名字提示的，$request-&gt;all\(\)返回个数组，它包含用户提供的所有输入。例如，处于某种原因，你决定将表单POST到一个带查询参数的URL，例如发送一个POST请求到[http://myapp.com/signup?utm=12345](http://myapp.com/signup?utm=12345)，如示例7-1,你将看到会从$request-&gt;all\(\)获取什么\(注意$request-&gt;all\(\)还包含上传的任何文件信息，但是我们将在本章后面介绍这些信息\)。

{% code-tabs %}
{% code-tabs-item title="Example 7-1. $request->all\(\)" %}
```php
<!-- GET route form view at /get-route -->
<form method="post" action="/signup?utm=12345"> 
    @csrf
    <input type="text" name="first_name">
    <input type="submit"> 
</form>
// routes/web.php
Route::post('signup', function (Request $request) { 
    var_dump($request->all());
});

// Outputs:
/** *[
 *     '_token' => 'CSRF token here',
 *     'first_name' => 'value',
 *     'utm' => 12345, 
 *   ]
*/
```
{% endcode-tabs-item %}
{% endcode-tabs %}

