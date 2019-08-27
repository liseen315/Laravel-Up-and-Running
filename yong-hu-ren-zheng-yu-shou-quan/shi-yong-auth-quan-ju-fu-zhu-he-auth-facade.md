# 使用auth\(\)全局辅助和Auth Facade

auth\(\)全局辅助函数是在整个应用内与已认证用户交互最简单的方法。你也可以注入Illuminate\Auth \AuthManager实例获取相同的功能，或者使用Auth facade。

最常见的需求是检测用户是或否登录\(如果已经登录auth\(\)-&gt;check\(\)将返回true,auth\(\)-&gt;guest\(\)如果用户没登录则返回true\)。以及获取当前登录的用户\(auth\(\)-&gt;user\(\),或者auth\(\)-&gt;id\(\)用于获取用户ID，如果用户没登录，这俩方法返回null\)。

查看示例9-3，在控制器内使用全局auth\(\)辅助函数。

{% code-tabs %}
{% code-tabs-item title="Example 9-3. Sample usage of the auth\(\) global helper in a controller" %}
```php
public function dashboard() {
    if (auth()->guest()) {
        return redirect('sign-up');
    }
    return view('dashboard') ->with('user', auth()->user());
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

下一章介绍auth系统的幕后，这是有用的但不是特别重要的信息，如果你想查看的话请翻阅231页面的"The Auth Scaffold"

