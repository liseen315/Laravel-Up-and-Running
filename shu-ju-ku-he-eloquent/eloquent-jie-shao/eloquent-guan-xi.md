# Eloquent关系

在关系数据库模型中,表与表之前保持相应关系，Eloquent提供简单强大的工具处理表之间的关系比以往都简单。

本章的许多例子都会集中在用户的联系人身上,这是一种常见情况。在Eloquent的ORM中这称之为一对多关系。一个用户有许多联系人。

如果是CRM，联系人被分配给用户，这将是多对多关系。多个用户与单个联系人关联,每个用户与多个联系人关联,用户拥有并属于多个联系人

如果每个联系人都有许多电话号码，而且用户想要一个每个电话号码的数据库用于他们的CRM，你会说用户通过联系人有很多电话号码 - 也就是说，用户有很多联系人，而且联系人有很多电话号码 数字，所以联系人是一种中间人

如果每个联系人都有一个地址，但您只想跟踪一个地址呢？您可以在联系人上包含所有地址字段，但也可以创建一个地址模型，表示联系人只有一个地址。

最后，如果您希望能够加入明星（最喜欢的）联系人，还有活动，该怎么办？ 这将是一种多态关系，其中用户有很多明星，但有些可能是联系人，有些可能是事件

那么让我们深入如何去定义这些关系。

**一对一**

让我们看个例子,一个联系人_has one电话号,这种关系定义在示例5-42_

{% code-tabs %}
{% code-tabs-item title="Example 5-42. Defining a one-to-one relationship" %}
```php
class Contact extends Model {
    public function phoneNumber() {
        return $this->hasOne(PhoneNumber::class); 
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如示,在Eloquent模型上定义了关系方法\(\($this-&gt;hasOne\(\)\),在实例上采用完全限定类名定义他们之间的关系。

如何在数据库上定义?因为我们定义一个联系人对应一个电话号,Eloquent期望支持PhoneNumber类的表\(可能是phone\_numbers\)有一个叫contact\_id的字段,如果你将其命名为不同的东西\(例如，owner\_id\)，你需要更改你的定义。

`return $this->hasOne(PhoneNumber::class, 'owner_id');`

如下是如何获取联系人的电话号

```php
$contact = Contact::first();
$contactPhone = $contact->phoneNumber;
```

注意，我们在示例5-42定义了phoneNumber\(\)方法,然后我们使用-&gt;phoneNumber来访问。这是个魔法,你也可以通过-&gt;phone\_number来访问,它将返回关联PhoneNumber记录的完整Eloquent实例。

但是，如果我想从PhoneNumber获取联系人怎么办？如下是解决办法，查看示例5-43

{% code-tabs %}
{% code-tabs-item title="Example 5-43. Defining a one-to-one relationship’s inverse" %}
```php
class PhoneNumber extends Model {
    public function contact() {
        return $this->belongsTo(Contact::class); 
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

然后通过同样的方式访问.

`$contact = $phoneNumber->contact;`

> 插入关系项
>
> 每种关系类型都有自己的特性来关联模型,但是核心工作原理是:传递实例给save\(\)或者传递实例数组给saveMany\(\),你还可以传递属性给createMany\(\)或者是create\(\),它们将为你生成新的实例.

```php
$contact = Contact::first();
$phoneNumber = new PhoneNumber;
$phoneNumber->number = 8008675309;
$contact->phoneNumbers()->save($phoneNumber);
// or
$contact->phoneNumbers()->saveMany([
    PhoneNumber::find(1),
    PhoneNumber::find(2),
]);
// or
$contact->phoneNumbers()->create([
    'number' => '+13138675309',
]);
// or
$contact->phoneNumbers()->createMany([
    ['number' => '+13138675309'],
    ['number' => '+15556060842'],
]);
```

> createMany\(\)方法只在Laravel5.4及之后版本可用

**一对多**

一对多关系是很常见的,让我们看看如何定义用户拥有多个联系人，如示例5-44

{% code-tabs %}
{% code-tabs-item title="Example 5-44. Defining a one-to-many relationship" %}
```php
class User extends Model {
    public function contacts() {
        return $this->hasMany(Contact::class); 
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

同样，联系人模型对应的表\(可能是contacts\)有一个user\_id字段，如果没有通过给hasMany\(\)传递正确字段名覆盖.

所以我们可以按照如下方式获取用户的联系人

```php
$user = User::first();
$usersContacts = $user->contacts;​
```

如同一对一一样，我们将方法当做属性来调用,但是这个方法返回的是集合而不是实例，它是一个Eloquent集合，所以我们再其上面各种玩.

```php
$donors = $user->contacts->filter(function ($contact) { 
    return $contact->status == 'donor';
});
$lifetimeValue = $contact->orders->reduce(function ($carry, $order) {
    return $carry + $order->amount;
}, 0);
```

就像一对一一样,你也可以定义反向获取,如示例5-45

{% code-tabs %}
{% code-tabs-item title="Example 5-45. Defining a one-to-many relationship’s inverse" %}
```php
class Contact extends Model {
    public function user() {
        return $this->belongsTo(User::class); 
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

就像一对一一样,我们可以从联系人获取用户

`$userName = $contact->user->name;`

> 从附加项附加和分离相关项
>
> 大多数情况下，我们通过在父项上运行save\(\)并传入相关项来附加项目，如$user-&gt;contacts\(\)-&gt;save\($contact\)。 但是，如果要在附加（“子”）项上执行这些行为，可以对返回belongsTo关系的方法使用associate\(\)和dissociate\(\)

```php
$contact = Contact::first();
$contact->user()->associate(User::first());
$contact->save(); // and later
$contact->user()->dissociate();
$contact->save();
```

**将关系用作查询器**

到目前为止，我们把方法名（例如contacts\(\)）作为属性（例如$user-&gt;contacts）调用。如果我们按照方法调用，会发生什么？它将返回一个预作用域查询器，而不是处理关系。

因此，如果您有用户1，并且您调用了它的contacts\(\)方法，那么现在您将有一个查询器预处理为“所有具有字段user\_id且值为1的联系人”。然后您可以从中构建一个函数查询。

`$donors = $user->contacts()->where('status', 'donor')->get();`

**选择具有相关项的记录**

你可以使用has\(\)选择相关项的记录

`$postsWithComments = Post::has('comments')->get();`

可以调整条件

`$postsWithManyComments = Post::has('comments', '>=', 5)->get();`

可以嵌套条件

`$usersWithPhoneBooks = User::has('contacts.phoneNumbers')->get();`

最后，你可以编写自定义查询

```php
// Gets all contacts with a phone number containing the string "867-5309"
$jennyIGotYourNumber = Contact::whereHas('phoneNumbers', function ($query) { 
    $query->where('number', 'like', '%867-5309%');
});
```

**Has many through**

hasManyThrough\(\)实际上是关系方法,想象一下之前给的例子,一个用户有多个联系人,每个联系人有多个手机号，如果你想获取一个用户联系人的手机号应该怎么办？这称为has-many-through关系

这种结构假设联系人表有一个user\_id关联联系人与用户,phone\_numbers表有一个contact\_id关联它到联系人,然后我们在User上定义关系,如示例5-46

{% code-tabs %}
{% code-tabs-item title="Example 5-46. Defining a has-many-through relationship" %}
```php
class User extends Model {
    public function phoneNumbers() {
        return $this->hasManyThrough(PhoneNumber::class, Contact::class); 
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

你可以使用$user-&gt;phone\_numbers访问此关系，您可以自定义中间模型上的关系键（用hasmanythrough\(\)的第三个参数）和远程模型上的关系键（第四个参数）

**Has one through**

hasOneThrough\(\)与hasManyThrough\(\)类似，但不是通过中间项访问许多相关项，而是通过单个中间项访问单个相关项

如果用户属于一个公司,公司只有一个电话号,而你希望通过提取用户公司的电话号码来获取该用户的电话号码，那该怎么办？这是hasOneThrough\(\)该干的事

{% code-tabs %}
{% code-tabs-item title="Example 5-47. Defining a has-one-through relationship" %}
```php
class User extends Model {
    public function phoneNumber() {
        return $this->hasOneThrough(PhoneNumber::class, Company::class); 
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**多对多**

现在变的复杂起来了,让我们考虑一种CRM情况,一个用户对应多个联系人,每个联系人又关联着多个用户.

如示例5-48，我们在用户上定义关系

{% code-tabs %}
{% code-tabs-item title="Example 5-48. Defining a many-to-many relationship" %}
```php
class User extends Model {
    public function contacts() {
        return $this->belongsToMany(Contact::class); 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

反向类似5-49

{% code-tabs %}
{% code-tabs-item title="Example 5-49. Defining a many-to-many relationship’s inverse" %}
```php
class Contact extends Model {
    public function users() {
        return $this->belongsToMany(User::class); 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

由于单个联系人不能拥有user\_id列，并且单个用户不能拥有contact\_id列，因此多对多关系依赖于连接两者的数据透视表。 这个表的常规命名是通过将两个单个表名放在一起，按字母顺序排序，并用下划线分隔它们来完成。

因此，由于我们要链接用户和联系人，我们的数据透视表应该命名为contacts\_users（如果您想自定义表名，请将其作为第二个参数传递给belongsToMany（）方法）。 它需要两列：contact\_id和user\_id

就像hasmany\(\)一样，我们可以访问相关项的集合，但这一次它是来自双方的（示例5-50\)

{% code-tabs %}
{% code-tabs-item title="Example 5-50. Accessing the related items from both sides of a many-to-many relationship" %}
```php
$user = User::first();
$user->contacts->each(function ($contact) {
    // do something
}
$contact = Contact::first();
$contact->users->each(function ($user) { 
    // do something
});
$donors = $user->contacts()->where('status', 'donor')->get();
```
{% endcode-tabs-item %}
{% endcode-tabs %}

