# Blade认证指令

如果你想检查用户是否认证过，不在路由层而是在视图上，你可以使用@auth和@guest（如示例9-10）

{% code-tabs %}
{% code-tabs-item title="Example 9-10. Checking a user’s authentification status in templates" %}
```text
@auth
// The user is authenticated
@endauth
@guest
// The user is not authenticated
@endguest
```
{% endcode-tabs-item %}
{% endcode-tabs %}

你还可以给两个方法传递守护名作为参数来执行守护,如示例9-11

{% code-tabs %}
{% code-tabs-item title="Example 9-11. Checking a specific auth guard’s authentification in templates" %}
```text
@auth('trainees')
// The user is authenticated
@endauth
@guest('trainees')
// The user is not authenticated
@endguest
```
{% endcode-tabs-item %}
{% endcode-tabs %}

