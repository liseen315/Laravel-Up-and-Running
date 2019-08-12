# 重定向

截至目前,我们已经讨论过从控制器方法或者是路由定义返回视图,但是我们也可以返回一些给浏览器一些其他类型的结构.

首先,让我们讨论一下重定向,你可能从其他例子看到过.有两种方法来生成重定向,我们将使用redirect\(\)全局辅助函数,但是你可能会更新使用facade，两者都会创建Illumina\Http\RedirectResponse实例,在实例上执行一些方法,然后返回它,你也可以手动执行重定向,但是你需要多做一些工作,查看实例3-38，了解返回重定向的几种方法.

{% code-tabs %}
{% code-tabs-item title="Example 3-38. Different ways to return a redirect" %}
```php
// Using the global helper to generate a redirect response
Route::get('redirect-with-helper', function () { 
    return redirect()->to('login');
});
// Using the global helper shortcut
Route::get('redirect-with-helper-shortcut', function () { 
    return redirect('login');
});
// Using the facade to generate a redirect response
Route::get('redirect-with-facade', function () { 
    return Redirect::to('login');
});
// Using the Route::redirect shortcut in Laravel 5.5+
Route::redirect('redirect-by-route', 'login');

```
{% endcode-tabs-item %}
{% endcode-tabs %}

注意,redirect\(\)辅助函数与Redirect facade暴露同样的方法,但是它也有一个快捷方式,如果你直接把参数传递给辅助函数，而不是在后面调用方法,则它是to\(\)的快捷方式.

还要注意的是,Route::redirect\(\)的第三个可选参数,可以帮助你为重定向生成状态码\(例如,302等\)



