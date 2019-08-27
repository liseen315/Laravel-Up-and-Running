# 授权（ACL）和角色

最后，让我们介绍一下Larave的授权系统，它用于确定用户是否有权执行特定的操作。你可以使用如下几个动作来进行检查:can, cannot, allows, 和 denies 。在Laravel 5.2引入了访问控制系统\(ACL\)。

大多数此授权控制将使用Gate facade执行，但在你的控制器，User模型，中间件和Blade指令中也提供了便利助手。 看看例9-14，了解我们能够做些什么。

{% code-tabs %}
{% code-tabs-item title="Example 9-14. Basic usage of the Gate facade" %}
```php
if (Gate::denies('edit-contact', $contact)) { 
    abort(403);
}
if (! Gate::allows('create-contact', Contact::class)) { 
    abort(403);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

