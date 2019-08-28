# 检查用户实例

如果不在控制器内,你更有可能检查特定用户的功能而不是当前经过身份验证的用户。 可以使用Gate facade的forUser\(\)方法，但有时语法可能会略微差别。

值得庆幸的是,User 类上的Authorizable提供了三个可读性好的方法:$user-&gt;can\(\), $user-&gt;cant\(\), 和 $user-&gt;cannot\(\)，你可能猜到了，cant\(\) 和 cannot\(\) 做同样的事，can\(\)与他们相反。

查看示例9-22

{% code-tabs %}
{% code-tabs-item title="Example 9-22. Checking authorization on a User instance" %}
```php
$user = User::find(1);
if ($user->can('create-contact')) { 
    // Do something
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

背后这些方法将参数传给Gate,如之前的实例:`Gate::forUser($user)->check('create-contact')`

