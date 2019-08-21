# JSON 输入\($request-&gt;json\(\)\)

到目前为止，我们已经讨论了来自查询字符串（get）和表单提交（post）的输入。但是还有另一种形式的用户输入，随着JavaScript SPA的出现变得越来越常见：JSON请求。它本质上只是一个post请求，body设置为json，而不是传统的表单post。

让我们看一下向Laravel路由提交JSON可能是什么样子，以及如何使用$request-&gt;input\(\)来提取数据（示例7-8）

{% code-tabs %}
{% code-tabs-item title="Example 7-8. Getting data from JSON with $request->input\(\)" %}
```php
POST /post-route HTTP/1.1 Content-Type: application/json
{
"firstName": "Joe", "lastName": "Schmoe", 
    "spouse": {
        "firstName": "Jill",
        "lastName":"Schmoe" 
    }
}

// Post-route
Route::post('post-route', function (Request $request) { 
    $firstName = $request->input('firstName');
    $spouseFirstname = $request->input('spouse.firstName');
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

由于$request-&gt;input\(\)足够智能，可以从get、post或json中提取用户数据，因此您可能想知道为什么Laravel还提供$request-&gt;json\(\)。您可能喜欢$request-&gt;json\(\)有两个原因。首先，您可能只想对在项目中工作的程序员更明确地说明您期望数据来源。第二，如果post没有正确的application/json头，$request-&gt;input\(\)不会将其作为json，但是$request-&gt;json\(\)会认为是json

> Facade 命名空间, request\(\) 全局辅助函数和$request注入
>
> 当你在命名空间类（例如控制器）使用facade时，都必须导入完整的命名空间路径到文件顶部,例如\(Illuminate\Support\Facades\Request\)
>
> 因此，一些facades也有一个伴生的全局辅助功能。如果这些助手函数不带参数运行，它们将公开与外观相同的语法（例如，request\(\)-&gt;has\(\)与request::has\(\)相同）。当您向它们传递参数时，它们还具有默认行为（例如，request\("firstname"\)是request\(\)-&gt;input\('firstName'\)的快捷方式）。
>
> 对于请求，我们已经讨论了注入请求对象的实例，但是您也可以使用请求facade或request\(\)全局助手。请看第10章了解更多信息。

