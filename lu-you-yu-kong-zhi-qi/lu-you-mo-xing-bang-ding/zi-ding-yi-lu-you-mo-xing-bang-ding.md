# 自定义路由模型绑定

要手动配置路由模型绑定,请在App\Providers\RouteServiceProvider的boot方法中添加一行

{% code-tabs %}
{% code-tabs-item title="Example 3-33. Adding a route model binding" %}
```php
public function boot() {
    // Just allows the parent's boot() method to still run
    parent::boot();
    // Perform the binding
    Route::model('event', Conference::class);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

您现在已经指定，只要路由在其定义中具有名为{event}的参数，如示例3-34所示，路由解析器将返回具有该URL参数的ID的Conference类的实例

{% code-tabs %}
{% code-tabs-item title="Example 3-34. Using an explicit route model binding" %}
```php
Route::get('events/{event}', function (Conference $event) { 
    return view('events.show')->with('event', $event);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

