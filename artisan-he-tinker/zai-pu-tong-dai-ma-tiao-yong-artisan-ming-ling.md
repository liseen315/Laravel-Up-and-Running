# 在普通代码调用Artisan命令

虽然Artisan命令为命令行设计,但是你也可以从其他代码中调用它们。

最简单的方式是使用Artisan facade。你可以使用Artisan::call\(\)\(它将返回命令退出码\)，或者使用Artisan::queue\(\)对命令执行队列。

两者都有两个参数，第一个参数是终端命令\(password:reset\)，第二个参数是传递给它的参数数组，如示例8-13看看如何处理参数跟选项的。

{% code-tabs %}
{% code-tabs-item title="Example 8-13. Calling Artisan commands from other code" %}
```php
Route::get('test-artisan', function () { 
        $exitCode = Artisan::call('password:reset', [
        'userId' => 15,
        '--sendEmail' => true, 
        ]);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如您所见，通过键入参数名称来传递参数，而没有值的选项可以传递为true或false。

> 在Laravel 5.8中你可以更自然的调用命令，只需将命令行调用的相同字符串传递给Artisan :: call\(\)

您也可以使用$this-&gt;call\(\)（与Artisan::call\(\)相同）或$this-&gt;callSilent\(\)从其他命令调用Artisan命令，但禁用所有输出。示例见示例8-14。

{% code-tabs %}
{% code-tabs-item title="Example 8-14. Calling Artisan commands from other Artisan commands" %}
```php
public function handle() {
    $this->callSilent('password:reset', [
        'userId' => 15,
    ]); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

最后，你可以注入Illuminate\Contracts\Console\Kernel实例，然后调用其call方法。

