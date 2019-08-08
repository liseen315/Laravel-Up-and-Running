# 子域路由

子域路由与路由前缀相同，子域名路由和路由路径前缀一样，不过是通过子域名而非路径前缀对分组路由进行约束，子域名路由有两个使用场景，一个是为应用子系统设置不同的子域名。示例3-14显示了如何实现这一点

{% code-tabs %}
{% code-tabs-item title="Example 3-14. Subdomain routing" %}
```php
Route::domain('api.myapp.com')->group(function () {
    Route::get('/', function () {
    });
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

其次，您可能希望将子域的一部分设置为参数，如示例3-15所示。这通常是在多租户的情况下完成的（想想slack或harvest，每个公司都有自己的子域，比如tence.slack.co）。

{% code-tabs %}
{% code-tabs-item title="Example 3-15. Parameterized subdomain routing" %}
```php
Route::domain('{account}.myapp.com')->group(function () {
    Route::get('/', function ($account) {
//
    });
    Route::get('users/{id}', function ($account, $id) {
//
    });
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

注意，这种情况下，`$account` 永远是所有分组路由的第一个路由参数。

