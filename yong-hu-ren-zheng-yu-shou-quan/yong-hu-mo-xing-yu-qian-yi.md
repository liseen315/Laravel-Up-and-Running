# 用户模型与迁移

当你创建一个新的Laravel应用，你将会看到creae\_users\_table迁移和App\User模型。示例9-1显示了迁移，以及users表内的字段。

{% code-tabs %}
{% code-tabs-item title="Example 9-1. Laravel’s default user migration" %}
```php
Schema::create('users', function (Blueprint $table) {
  $table->bigIncrements('id');
  $table->string('name');
  $table->string('email')->unique();
  $table->timestamp('email_verified_at')->nullable();
  $table->string('password');
  $table->rememberToken();
  $table->timestamps();
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

我们有一个自增的主键ID、一个名称、一个唯一的电子邮件、一个密码、一个“记住我”标记，以及创建和修改的时间戳。这涵盖了在大多数应用程序中处理基本用户身份验证所需的所有内容。

> 认证和授权的区别
>
> 身份验证意味着验证某人是谁，允许他们充当系统用户。包括登录和登出过程。允许用户在使用应用的同时标识自己。
>
> 授权意味着确定经过身份验证的用户是否被允许（授权）执行特定行为。 例如，授权系统允许您禁止任何非管理员查看网站的收入。

用户模型有点复杂，如例9-2所示。App\Author类本身很简单，但是它继承了Illuminate\Foundation\Auth\User类，其中包含了几个特性。

{% code-tabs %}
{% code-tabs-item title="Example 9-2. Laravel’s default User model" %}
```php
<?php

namespace App;

use Illuminate\Notifications\Notifiable;
use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    use Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        'name', 'email', 'password',
    ];

    /**
     * The attributes that should be hidden for arrays.
     *
     * @var array
     */
    protected $hidden = [
        'password', 'remember_token',
    ];

    /**
     * The attributes that should be cast to native types.
     *
     * @var array
     */
    protected $casts = [
        'email_verified_at' => 'datetime',
    ];
}
// Illuminate\Foundation\Auth\User
<?php

namespace Illuminate\Foundation\Auth;

use Illuminate\Auth\Authenticatable;
use Illuminate\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Auth\Passwords\CanResetPassword;
use Illuminate\Foundation\Auth\Access\Authorizable;
use Illuminate\Contracts\Auth\Authenticatable as AuthenticatableContract;
use Illuminate\Contracts\Auth\Access\Authorizable as AuthorizableContract;
use Illuminate\Contracts\Auth\CanResetPassword as CanResetPasswordContract;

class User extends Model implements
    AuthenticatableContract,
    AuthorizableContract,
    CanResetPasswordContract
{
    use Authenticatable, Authorizable, CanResetPassword, MustVerifyEmail;
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

> Eloquent 模型刷新器
>
> 如果对这些陌生，请查看第五章然后继续阅读。

我们能从模型中学到些什么呢?首先,Laravel通过类名推断，users存在于users 表格。当创建一个新用户，我们需要填写name,email,以及password属性。当将user作为JSON输出时,排除remember\_token和password属性。到目前为止看起来还不错。

我们还可以从Illuminate\Foundation \Auth看到一些框架特性\(认证、授权和重置密码的能力\)，这些特性不仅仅可以用于User模型，理论还可以用于其他模型，可以单独和一起使用。

> 契约和接口
>
> 你可能注意到，有时候我会写单词"contract"有时会写成"interface"。Laravel大多数接口都位于Contracts命名空间下。
>
> PHP接口本质上是两个类之间的协议，其中一个类将以某种方式“行为”。这有点像他们之间的一个契约，把它当作一个契约来考虑比把它称为接口更能赋予这个名字内在的意义。
>
> 但最后，它们是相同的：一个类将为某些方法提供特定的协议。
>
> 在相关的说明中，Illuminate\Contracts命名空间包含一组Laravel组件实现的接口和typehint。 这样可以轻松开发实现相同接口的类似组件，并将它们交换到您的应用程序中，代替Illuminate组件。 例如，当Laravel核心和组件typehint邮件程序时，它们不会键入Mailer类。 相反，他们typehint了Mailer契约（接口），使您可以轻松提供自己的邮件。 要了解有关如何执行此操作的更多信息，请查看第11章

Authenticatable契约必选方法\(例如,getAuthIdentifier\(\)\)，用于允许框架向auth系统验证该模型的实例。Authenticatable包含的方法需要带Eloquent模型的契约。

Authorizable契约必选方法\(can\(\)\)，该方法允许框架在不同的上下文中为该模型的实例授权访问权限。不出所料，Authorizable提供的方法将满足Eloquent模型的Authorizable契约。

最后，CanResetPassword契约的方法（getEmail ForPasswordReset\(\), sendPasswordResetNotification\(\)），用于重置密码，CanResetPassword提供了满足Eloquent模型的合同的方法。

到此，我们可以轻松的在数据库内表述用户\(通过迁移\)，并使用一个模型实例将其提取，该模型实例可以进行身份验证（登录和退出），授权（检查对特定资源的访问权限）和发送一个密码重设邮件。



