# 关于请求

除了返回视图以及重定向以外,通常退出路由的方法是abort,有几个全局的可用方法\(abort\(\).abort\_if\(\)以及abort\_unless\(\)\)，他们可以将HTTP状态码,一条消息,或者是头数组作为参数携带.

如例3-45所示，abort\_if\(\)和abort\_until\(\)取第一个参数，对其真实性进行评估，并根据结果执行abort。

{% code-tabs %}
{% code-tabs-item title="Example 3-45. 403 Forbidden aborts" %}
```php
Route::post('something-you-cant-do', function (Illuminate\Http\Request $request) {
    abort(403, 'You cannot do that!');
    abort_unless($request->has('magicToken'), 403);
    abort_if($request->user()->isBanned, 403);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

