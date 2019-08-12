# redirect\(\)-&gt;with\(\)

虽然它与你可以调用的redirect\(\)的其他方法类似,但是with\(\)它咩有定义重定向的位置,而是定义了在重定向过程中传递的数据,当你将用户重定向到不同的页面时,你通常希望同时传递某些数据,你可以将数据手动存到session中,Laravel有一些方便的方法来帮助你实现这一目标.

大多数情况,你可以使用witch\(\)传递键值对数组或者是单个键值,如例子3-42，它将with\(\)数据到你下一个加载的页面.

{% code-tabs %}
{% code-tabs-item title="Example 3-42. Redirect with data" %}
```php
Route::get('redirect-with-key-value', function () { 
    return redirect('dashboard')->with('error', true);
});
Route::get('redirect-with-array', function () { 
    return redirect('dashboard')->with(['error' => true, 'message' => 'Whoops!']);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> 重定向上的链式方法
>
> 与许多其他facade一样,重定向facade可以使用fluent方法链,就像3-42例子一样,你将在105页的”什么是Fluent interface“学习具体的内容

您还可以使用withInput\(\)，如示例3-43所示，在用户的表单输入被刷新的情况下重定向；这在验证错误的情况下最常见，在验证错误的情况下，您希望将用户发送回他们刚来自的表单

{% code-tabs %}
{% code-tabs-item title="Example 3-43. Redirect with form input" %}
```php
Route::get('form', function () { 
    return view('form');
});
Route::post('form', function () { 
    return redirect('form')
      ->withInput()
      ->with(['error' => true, 'message' => 'Whoops!']);
});

```
{% endcode-tabs-item %}
{% endcode-tabs %}

你可以配合使用old\(\)辅助函数来生成之前输入的内容，如下面的示例所示，如果没有旧值，则使用第二个参数作为默认值）。您通常会在视图中看到这一点，这用于为表单"创建","编辑"视图.

`<input name="username" value="<?=old('username', 'Default username instructions here');?>">`

说到验证,有一个有用的方法传递验证错误:withErrors\(\),您可以将任何错误的“提供者”传递给它，这些错误可能是错误字符串、错误数组，或者最常见的，是Illuminate证器的实例，我们将在第10章中介绍。示例3-44显示了其使用的示例。

{% code-tabs %}
{% code-tabs-item title="Example 3-44. Redirect with errors" %}
```php
Route::post('form', function (Illuminate\Http\Request $request) {
    $validator = Validator::make($request->all(), $this->validationRules);
    if ($validator->fails()) {
        return back()
            ->withErrors($validator)
            ->withInput();
    }
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

withErrors\(\)自动将$errors变量与它重定向到的页面的视图共享，您可以随意处理。

> 请求的验证
>
> 觉得例子3-44很麻烦是不是?有一个简单的方法,关于在"在请求对象上的验证"，请查看189页

