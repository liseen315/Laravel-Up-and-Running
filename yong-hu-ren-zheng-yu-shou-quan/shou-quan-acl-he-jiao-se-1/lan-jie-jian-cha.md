# 拦截检查

如果您曾经使用admin用户类构建了一个应用程序，那么您可能已经在本章中查看了所有简单的授权闭包，并考虑了如何在每种情况下添加一个覆盖这些检查的超级用户类。 值得庆幸的是，已经有了一个工具。

在AuthServiceProvider中，您已经定义了自己的能力，您还可以添加一个before\(\)检查，该检查在所有其他功能之前运行，并且可以选择性地覆盖它们，如示例9-25所示。

{% code-tabs %}
{% code-tabs-item title="Example 9-25. Overriding Gate checks with before\(\)" %}
```php
Gate::before(function ($user, $ability) { 
    if ($user->isOwner()) {
        return true; 
    }
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

注意能力的名字也被传入了，因此你可以用名字来区分before\(\)钩子能力。



