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

