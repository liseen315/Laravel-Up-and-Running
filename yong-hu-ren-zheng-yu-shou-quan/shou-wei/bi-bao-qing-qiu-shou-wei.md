# 闭包请求守卫

如果你想自定义一个守卫，守卫条件是简单的描述为响应HTTP请求\(如何根据请求查找指定用户\)，那么你可能会查找用户的代码放到闭包内，而不是创建一个新的守卫类。

viaRequest\(\)认证方法使用闭包\(第二个参数\)通过HTTP请求返回指定用户来定义守卫。要注册一个闭包请求守卫，在AuthServiceProvider的boot方法内调用viaRequest\(\),如示例9-12

{% code-tabs %}
{% code-tabs-item title="Example 9-12. Defining a closure request guard" %}
```php
public function boot() {
    $this->registerPolicies();
    Auth::viaRequest('token-hash', function ($request) {
        return User::where('token-hash', $request->token)->first();
    }); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

