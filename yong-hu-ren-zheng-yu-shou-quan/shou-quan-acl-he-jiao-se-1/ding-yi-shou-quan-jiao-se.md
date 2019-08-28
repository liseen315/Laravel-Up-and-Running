# 定义授权角色

定义授权规则的默认位置在AuthServiceProvider的boot\(\)方法中，您将在该方法中调用Auth facade上的方法。

授权有两部分构成:一个字符串键\(例如,update-contact\)和一个返回Boolean的闭包，示例9-15显示更新联系人。

{% code-tabs %}
{% code-tabs-item title="Example 9-15. Sample ability for updating a contact" %}
```php
class AuthServiceProvider extends ServiceProvider {
    public function boot()
    {
        $this->registerPolicies();
        Gate::define('update-contact', function ($user, $contact) {
            return $user->id == $contact->user_id;
        });
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

让我们看一下定义的步骤

首先，你要定义一个键，然后给这个键命名，你应该考虑给用户提供一个有意义的名字，你可以在示例中看到使用的约定{verb}-{modelName}:create-contact, update-contact等等。

其次，你要定义一个闭包，第一个参数将是当前经过身份验证的用户，之后的所有参数将是您要检查访问的对象-在本例中是联系人。

因此，对于这两个对象，我们检查用户是否有权更新联系人。你可以根据需求编写逻辑，但此时我们研究的是，授权依赖于联系人的创建者。如果是当前用户创建的联系人则返回True,如果不是返回False。

就像闭包路由一样，你也可以用类来代替闭包。

```php
$gate->define('update-contact', 'ContactACLChecker@updateContact');
```



