# 作用域

我们已经介绍过查询"过滤",意味着任何查询我们不仅仅返回表的每个查询。我们在本章内手写查询的时，都是使用手动编写的查询器

Eloquent的本地和全局作用域允许你预定义"作用域"\(过滤\),意味着你可以\("全局"\)模型查询时使用,也可以\("本地"\)用链式写法查询时使用.

**本地作用域**

本地作用域非常容易理解,让我们看个例子

`$activeVips = Contact::where('vip', true)->where('trial', false)->get();`

首先，我们一次一次的这么写有点烦,特别是如果我们应用内有大量这种查询,我们想是否有办法能简化呢?类似这种写法:

```text
$activeVips = Contact::activeVips()->get();
```

是的，我们可以这么干,我们称之为本地作用域,我们可以很轻松的将其定义在Contact类上,如示例5-29

{% code-tabs %}
{% code-tabs-item title="Example 5-29. Defining a local scope on a model" %}
```php
class Contact {
    public function scopeActiveVips($query) {
        return $query->where('vip', true)->where('trial', false); 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

为了定义本地作用域,我们给Eloquent类添加了一个方法,它以"scope"开头然后以驼峰命名作用域,这个方法传递一个查询器,然后返回一个查询器,当然你可以在返回之前修改查询。这就是全部的要点

你也可以定义带参数的作用域,如示例5-30

{% code-tabs %}
{% code-tabs-item title="Example 5-30. Passing parameters to scopes" %}
```php
class Contact {
    public function scopeStatus($query, $status) {
        return $query->where('status', $status); 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

同样你可以传递参数到作用域

`$friends = Contact::status('friend')->get();`

**全局作用域**

还记得我们是如何讨论软删除的吗？只有当您将模型上的每个查询都限定为忽略软删除项时，软删除才起作用。这是一个全局作用域。我们可以定义我们自己的全局作用域，它将应用于给定模型的每个查询。

有两种方式定义全局作用域,使用闭包或者是class,每一种方法,你需要在模型的boot\(\)定义作用域,先让我们看下闭包,如示例5-31

{% code-tabs %}
{% code-tabs-item title="Example 5-31. Adding a global scope using a closure" %}
```php
class Contact extends Model
{
    protected static function boot()
    {
        parent::boot();
        static::addGlobalScope('active', function (Builder $builder) {
            $builder->where('active', true);
        });
    }

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这样我们创建了一个名为active的全局作用域,这样模型上的所有查询都被限定为active为true的行

接着我们尝试另外一种方法,如示例5-32，创建一个实现Illuminate\Database\Eloquent\Scope的类,它有一个apply\(\)方法,带有查询器跟模型参数

{% code-tabs %}
{% code-tabs-item title="Example 5-32. Creating a global scope class" %}
```php
<?php
namespace App\Scopes;
use Illuminate\Database\Eloquent\Scope; 
use Illuminate\Database\Eloquent\Model; 
use Illuminate\Database\Eloquent\Builder;

class ActiveScope implements Scope {
    public function apply(Builder $builder, Model $model) {
        return $builder->where('active', true); 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

给模型应用作用域,覆盖父类的boot方法,然后通过static调用addGlobalScope\(\),如示例5-33

{% code-tabs %}
{% code-tabs-item title="Example 5-33. Applying a class-based global scope" %}
```php
<?php

use App\Scopes\ActiveScope;
use Illuminate\Database\Eloquent\Model;

class Contact extends Model
{
    protected static function boot()
    {
        parent::boot();
        static::addGlobalScope(new ActiveScope);
    }

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> 不带命名空间的Contact
>
> 你可能注意到几个关于Contact 类的例子都没有命名空间,这不正常,我这是为了省书的空间,一般情况下都应该位于App\Contact下面

**删除全局作用域**

删除全局作用域有三种方法，所有三种方法都使用withoutGlobalScope\(\)或withoutGlobalScope\(\)方法，如果你删除基于闭包的作用域,第一个参数为用addGlobalScope注册时启用的key

`$allContacts = Contact::withoutGlobalScope('active')->get();`

如果你要删除基于类的全局作用域,你可以将类名传递给withoutGlobalScope\(\) 或者withoutGlobalScopes\(\)方法

`Contact::withoutGlobalScope(ActiveScope::class)->get();`

`Contact::withoutGlobalScopes([ActiveScope::class, VipScope::class])->get();`

或者你可以为查询禁用全局作用域

`Contact::withoutGlobalScopes()->get();`  


