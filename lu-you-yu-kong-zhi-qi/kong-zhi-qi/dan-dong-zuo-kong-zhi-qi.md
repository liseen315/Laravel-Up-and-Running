# 单动作控制器

有时，在您的应用程序中，控制器应该只为单个路由提供服务。 您可能会发现自己想知道如何为该路由命名控制器方法。 值得庆幸的是，您可以在单个控制器上指向单个路由，而无需考虑命名一个方法

您可能已经知道，\_\_ invoke（）方法是一种PHP魔术方法，它允许您“调用”类的实例，将其视为函数并调用它。 这是Laravel的单动作控制器用于将路由指向单个控制器的工具，如示例3-30所示

{% code-tabs %}
{% code-tabs-item title="Example 3-30. Using the \_\_invoke\(\) method" %}
```php
// \App\Http\Controllers\UpdateUserAvatar.php
public function __invoke(User $user) {
    // Update the user's avatar image
}
// routes/web.php
Route::post('users/{user}/update-avatar', 'UpdateUserAvatar');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

