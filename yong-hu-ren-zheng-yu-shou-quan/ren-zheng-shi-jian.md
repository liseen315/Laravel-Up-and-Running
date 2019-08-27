# 认证事件

我们将在第16章中更多地讨论事件，但是Laravel的事件系统是一个基本的pub/sub框架。有些系统和用户生成的事件是广播的，并且用户能够创建事件侦听器来响应某些事件执行某些操作。

所以如果用户因为多次登录失败而被锁定，你想给特定的安全服务发送ping应该怎么办？也许这项服务会关注来自某些地理区域或其他地方的特定数量的失败登录，当然，您可以在适当的控制器中注入一个调用。但是对于事件，您可以创建一个事件监听器来监听“用户锁定”事件，并注册它。

查看示例9-13看认证系统派发的所有事件：

{% code-tabs %}
{% code-tabs-item title="Example 9-13. Authentication events generated by the framework" %}
```php
protected $listen = [ 
    'Illuminate\Auth\Events\Registered' => [],
    'Illuminate\Auth\Events\Attempting' => [],
    'Illuminate\Auth\Events\Authenticated' => [],
    'Illuminate\Auth\Events\Login' => [],
    'Illuminate\Auth\Events\Failed' => [],
    'Illuminate\Auth\Events\Logout' => [],
    'Illuminate\Auth\Events\Lockout' => [],
    'Illuminate\Auth\Events\PasswordReset' => [],
];
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如所示，这些事件用于“用户注册”、“用户尝试登录”、“用户已验证”、“成功登录”、“失败登录”、“注销”、“锁定”和“密码重置”,要学习如何为这些事件创建侦听，请查看第16章。


