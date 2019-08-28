# 策略

到目前为止，所有访问控制都要求您手动将Eloquent模型与能力名称相关联。 您可能已经创建了一个名为visit-dashboard的功能，这个功能与特定的Eloquent模型无关，但您可能已经注意到我们的大部分示例都与某些事情有关 - 在大多数情况下， 行动的接受者的东西是一个Eloquent 模型。

授权策略是一种组织结构，可帮助您根据您控制访问的资源对授权逻辑进行分组。 它们可以轻松管理针对特定Eloquent模型（或其他PHP类）的行为定义授权规则，所有这些都在一个位置。

**生成策略**

Policies是一个PHP类，可以通过Artisan命令来生成。

```bash
php artisan make:policy ContactPolicy
```

生成后，需要进行注册。 AuthServiceProvider有一个$ policies属性，它是一个数组。 每个项的键是受保护资源的类名（几乎总是一个Eloquent类），值是策略类名。 例9-26看起来像这样。

{% code-tabs %}
{% code-tabs-item title="Example 9-26. Registering policies in AuthServiceProvider" %}
```php
class AuthServiceProvider extends ServiceProvider {
    protected $policies = [
        Contact::class => ContactPolicy::class,
    ];
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Artisan生成的策略类没有任何特殊属性或方法。但是你添加的每个方法现在都被映射为这个对象的键

> 策略自动发现
>
> 在运行Laravel5.8+的应用程序中，Laravel试图“猜测”策略与其相应模型之间的链接。例如，它会自动地将后策略应用到您的post模型。
>
> 如果需要自定义Laravel用于猜测此映射的逻辑，请查看[策略文档](https://laravel.com/docs/5.8/authorization#creating-policies)。

让我们定义一个更新方法看看如何工作的，如示例9-27

{% code-tabs %}
{% code-tabs-item title="Example 9-27. A sample update\(\) policy method" %}
```php
<?php
namespace App\Policies;
class ContactPolicy {
    public function update($user, $contact) {
        return $user->id == $contact->user_id; 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

请注意，此方法的内容与Gate定义中的内容完全相同。

> 不带实例的策略方法
>
> 如果你想定义一个策略方法关联到类而不是特定的实例-例如，“此用户是否可以创建联系人？“而不仅仅是”此用户能否查看此特定联系人？在Laravel 5.3中，你可以像对待普通方法一样对待它。在Laravel5.2中，创建该方法时，需要在其名称的末尾添加“any”

```php
class ContactPolicy {
public function createAny($user) {
    return $user->canCreateContacts(); 
}
```

**检查策略**

如果为资源类型定义了策略，则Gate门户将使用第一个参数来确定要检查策略的方法。 如果运行Gate::allow\('update'，$ contact\)，它将检查ContactPolicy@update方法以进行授权。

这也适用于授权中间件以及用户模型检查和Blade检查，如例9-28所示。

{% code-tabs %}
{% code-tabs-item title="Example 9-28. Checking authorization against a policy" %}
```php
// Gate
if (Gate::denies('update', $contact)) { 
    abort(403);
}
// Gate if you don't have an explicit instance
if (! Gate::check('create', Contact::class)) { 
    abort(403);
}
// User
if ($user->can('update', $contact)) { 
    // Do stuff
}
// Blade
@can('update', $contact) 
    // Show stuff
@endcan
```
{% endcode-tabs-item %}
{% endcode-tabs %}

另外，还有一个policy\(\)辅助函数，允许您检索策略类和运行它的方法

```php
if (policy($contact)->update($user, $contact)) { 
    // Do stuff
}
```

**覆盖策略**

就像普通的能力定义一样，策略可以定义一个before\(\)方法，该方法允许你在调用被处理之前重写它（参见示例9-29）。

{% code-tabs %}
{% code-tabs-item title="Example 9-29. Overriding policies with the before\(\) method" %}
```php
public function before($user, $ability) {
    if ($user->isAdmin()) { 
        return true;
    } 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

