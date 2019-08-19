# Eloquent序列化

序列化是将复杂的数据-数组,对象,转换成字符串的过程,在基于web的中,字符串通常是JSON,但也可以是其他形式的。

序列化复杂的数据库记录可能会很复杂,这是许多ORM欠缺的地方,幸运的是,Eloquent中有两个强大的方法,toArray\(\)和toJson\(\),集合还有toArray\(\) 和 toJson\(\)方法,如下这些都是有效的:

```php
$contactArray = Contact::first()->toArray();
$contactJson = Contact::first()->toJson();
$contactsArray = Contact::all()->toArray();
$contactsJson = Contact::all()->toJson();
```

你也可以转换Eloquent实例或者是集合为一个字符串\($string = \(string\) $contact;\),模型和集合都将运行tojson\(\)并返回结果

**从路由直接返回模型**

Laravel的路由最终会将内容转换成字符串,所以你可以利用这个技巧.如果你在控制器返回Eloquent结果,它将自动转换成字符串,并做JSON返回,这意味着路由返回JSON如示例5-41一样简单

{% code-tabs %}
{% code-tabs-item title="Example 5-41. Returning JSON from routes directly" %}
```php
// routes/web.php
Route::get('api/contacts', function () { 
    return Contact::all();
});

Route::get('api/contacts/{id}', function ($id) { 
    return Contact::findOrFail($id);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**从JSON隐藏属性**

在APIs中返回JSON是很常见的,在其中隐藏某些属性也很常见,Eloquent可以很容易让你转成JSON并隐藏属性。

你可以使用黑名单来隐藏属性.

```php
class Contact extends Model {
    public $hidden = ['password', 'remember_token'];
}
```

使用白名单来显示属性.

```php
class Contact extends Model {
    public $visible = ['name', 'email', 'status'];
}
```

它同样用于关系.

```php
class User extends Model {
    public $hidden = ['contacts'];
    public function contacts() {
        return $this->hasMany(Contact::class); 
    }
}
```

> 加载关系内容.
>
> 默认情况下，获取数据库记录时不会加载关系的内容，因此无论是否隐藏它们都无关紧要。正如你即将了解的,可能会通过关系获取记录,在这种情况下，如果你选择隐藏该关系，这些项将不会包含在该记录的序列化副本中。
>
> 假如你很好奇,你可以通过联系人获取用户,-假设你的关系设置正确,如下所示

> $user = User::with\('contacts'\)-&gt;first\(\);

有时你希望属性对单次调用可见,这也是可行的,你可以调用Eloquent的makeVisible方法

`$array = $user->makeVisible('remember_token')->toArray();`

> 将生成的列添加到数组然后JSON输出
>
> 如果您已经为一个不存在的列创建了一个访问器，例如5-35,模型的$appends数组中的full\_name然后JSON输出

```php
class Contact extends Model {
    protected $appends = ['full_name'];
    public function getFullNameAttribute() {
        return "{$this->first_name} {$this->last_name}"; }
    }
```

