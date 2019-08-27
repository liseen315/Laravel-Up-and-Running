# ”记住我“

认证脚手架已经实现了这一点，但是学习如何工作的以及如何使用仍然有必要，如果你想要实现”remember me“ 这样的长期访问令牌，确保你的users表有个叫做remember\_token的字段\(如果你使用了默认的迁移，则会有此字段\)

当您正常登录用户时（这就是LoginController使用AuthenticatesUsers特性的方式），您将“尝试”使用用户提供的信息进行身份验证，如例9-6所示

{% code-tabs %}
{% code-tabs-item title="Example 9-6. Attempting a user authentication" %}
```php
if (auth()->attempt([
    'email' => request()->input('email'),
    'password' => request()->input('password'),
])) {
    // Handle the successful login
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

它提供了一个与用户登录一样长的用户会话，如果你想要Laravel无限期的继承登录（只要用户在一台机器登录并不注销登录），你可以传递true作为auth\(\)-&gt;attempt\(\)的第二个参数，看示例9-7的请求

{% code-tabs %}
{% code-tabs-item title="Example 9-7. Attempting a user authentication with a “remember me” checkbox check" %}
```php
if (auth()->attempt([
    'email' => request()->input('email'), 
    'password' => request()->input('password'),
]), request()->filled('remember')) { // Handle the successful login
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

可以看到，我们检查是否输入一个非空字段的remember属性，它将返回一个布尔值。这允许我们的用户通过登录表单中的复选框来决定是否要记住他们。

稍后，如果您需要手动检查当前用户是否通过记忆令牌进行身份验证，则可以使用以下方法：auth\(\)-&gt;viaRemember\(\)返回一个布尔值，指示当前用户是否通过记忆令牌进行身份验证。 这将允许您通过记忆令牌来防止某些敏感的功能被访问; 相反，您可以要求用户重新输入密码。

