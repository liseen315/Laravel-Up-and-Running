# 手动验证

如果你不在控制器内工作,或者其他原因，你可以手动创建Validator facade实例，然后检查成功或者失败，如示例7-14

{% code-tabs %}
{% code-tabs-item title="Example 7-14. Manual validation" %}
```php
Route::get('recipes/create', function () { 
    return view('recipes.create');
});

Route::post('recipes', function (Illuminate\Http\Request $request) {
    $validator = Validator::make($request->all(), [
        'title' => 'required|unique:recipes|max:125',
        'body' => 'required'
    ]);
    if ($validator->fails()) {
        return redirect('recipes/create')
            ->withErrors($validator)
            ->withInput();
    }
    // Recipe is valid; proceed to save it
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如看到的，我们创建了一个验证实例，将输入当做第一个参数，验证规则当第二个参数，验证对象公开了一个fails\(\)方法，用于检测验证,然后可以将验证实例传递给withErrors\(\)



