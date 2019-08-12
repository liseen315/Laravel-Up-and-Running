# redirect\(\)-&gt;to\(\)

redirects方法的签名如下:

`function to($to = null, $status = 302, $headers = [], $secure = null)`

$to是一个有效的路径,$status是HTTP状态码\(默认302\),$header允许你定义与重定向一起发送的HTTP头,$secure允许您覆盖http与https的默认选择（通常根据您当前的请求URL设置\),示例3-39显示了其使用示例

{% code-tabs %}
{% code-tabs-item title="Example 3-39. redirect\(\)->to\(\)" %}
```php
Route::get('redirect', function () { 
    return redirect()->to('home');
    // Or same, using the shortcut:
    return redirect('home'); 
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

