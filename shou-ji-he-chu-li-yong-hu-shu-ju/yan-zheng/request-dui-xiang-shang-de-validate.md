# Request对象上的validate\(\)

Request对象上有一个validate\(\)方法用于大多数验证。如示例7-13

{% code-tabs %}
{% code-tabs-item title="Example 7-13. Basic usage of request validation" %}
```php
// routes/web.php
Route::get('recipes/create', 'RecipesController@create');
Route::post('recipes', 'RecipesController@store');

// app/Http/Controllers/RecipesController.php
class RecipesController extends Controller {
    public function create()
    {
        return view('recipes.create');
    }

    public function store(Request $request)
    {
        $request->validate([
            'title' => 'required|unique:recipes|max:125',
            'body' => 'required'
        ]);
        // Recipe is valid; proceed to save it
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

我们只用了四行代码进行验证，但实际上内部做了很多工作。

首先，我们显示定义期望的字段并应用规则\(使用管道字符\|\),然后用管道连接。

接着，validate\(\)方法接收来自$request的数据并验证是否有效。

如果验证通过,可以在控制器内接着进行保存或者其他操作。

如果数据无效，则会抛出ValidationException异常，它包含了路由如何处理异常的说明。如果请求来自于Javascript\(或者JSON响应\)，异常将会包含验证错误json响应，如果不是，则会连同所有用户输入和验证错误重定向回上一页 - 非常适合重新填充失败的表单并显示一些错误。

> Laravel 5.5之前版本在控制器上调用validate\(\)
>
> Laravel 5.5之前版本使用\($this-&gt;validate\(\)\)而不是在request上调用

> Laravel更多验证规则
>
> 在例子中我们使用管道语法：'fieldname': 'rule\|otherRule\|anotherRule'，你也可以使用数组语法：'fieldname': \['rule', 'otherRule', 'anotherRule'\]
>
> 此外你可以验证嵌套属性，例如在一个HTML表单有多个用户，每个用户都有个关联的名字，例如

```php
$request->validate([
    'user.name' => 'required',
    'user.email' => 'required|email',
]);
```

> 我们没有足够的篇幅来介绍全部，但是这有一些常用的规则和方法：
>
> _必填字段_
>
> required; required\_if:anotherField,equalToThisValue; required\_unless:anotherField,equalToThisValue
>
> _字段必须包含某些类型的字符_
>
> alpha; alpha\_dash; alpha\_num; numeric; integer
>
> _字段必须匹配某些模式_
>
> email; active\_url; ip
>
> _日期_
>
> after:date; before:date
>
> _数字_
>
> between:min,max; min:num; max:num; size:num
>
> _图像尺寸_
>
> 尺寸:min\_width=XXX; 也可以使用 and/or 和 max\_width, min\_height, max\_height, width, height, and ratio 结合
>
> _数据库_
>
> exists:tableName; unique:tableName\(应与字段名在同一个表列中查找；有关如何自定义信息，请参阅[文档](https://laravel.com/docs/5.3/validation#rule-unique)\)

