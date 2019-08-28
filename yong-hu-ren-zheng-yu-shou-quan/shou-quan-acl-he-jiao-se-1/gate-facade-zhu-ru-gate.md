# Gate Facade\(注入Gate\)

现在你已经定义了，是时候测试一下了。最简单的方法是使用Gate facade。如示例9-16\(或者你可以注入一个Illuminate \Contracts\Auth\Access\Gate实例\)。

{% code-tabs %}
{% code-tabs-item title="Example 9-16. Basic Gate facade usage" %}
```php
if (Gate::allows('update-contact', $contact)) { 
    // Update contact
}
// or
if (Gate::denies('update-contact', $contact)) { 
    abort(403);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

你也可以定义多个参数，可能联系人在一个组内，你想要授权用户是否能添加联系人到组，如示例9-17显示。

{% code-tabs %}
{% code-tabs-item title="Example 9-17. Abilities with multiple parameters" %}
```php
// Definition
Gate::define('add-contact-to-group', function ($user, $contact, $group) { 
    return $user->id == $contact->user_id && $user->id == $group->user_id;
});
// Usage
if (Gate::denies('add-contact-to-group', [$contact, $group])) { 
    abort(403);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果需要检查当前已验证用户以外的用户的授权，请尝试forUser\(\)，如示例9-18所示。

{% code-tabs %}
{% code-tabs-item title="Example 9-18. Specifying the user for Gate" %}
```php
if (Gate::forUser($user)->denies('create-contact')) { 
    abort(403);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

