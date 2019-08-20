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

由于单个联系人不能拥有user\_id列，并且单个用户不能拥有contact\_id列，因此多对多关系依赖于连接两者的数据中枢表。 这个表的常规命名是通过将两个单个表名放在一起，按字母顺序排序，并用下划线分隔它们来完成。

因此，由于我们要链接用户和联系人，我们的数据中枢表应该命名为contacts\_users（如果您想自定义表名，请将其作为第二个参数传递给belongsToMany（）方法）。 它需要两列：contact\_id和user\_id

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

**从中枢表获取数据**

多对多关系有一个中枢表，中枢表内字段越少越好,但是有些时候需要在中枢表内存储信息,比如有时候你希望在中枢表内存放created\_at字段，来查看关系何时被创建的。

为了存储这些字段,你需要将它们添加到关系定义中，如示例5-51,你可以用withPivot\(\)定义特殊字段,也可以用withTimestamps\(\)添加created\_at 和 updated\_at 时间戳

{% code-tabs %}
{% code-tabs-item title="Example 5-51. Adding fields to a pivot record" %}
```php
public function contacts() {
    return $this->belongsToMany(Contact::class) 
        ->withTimestamps()
        ->withPivot('status', 'preferred_greeting');
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

当您通过关系获取模型实例时，它上面会有一个Pivot属性,它表示在中枢表中的位置.如示例5-52

{% code-tabs %}
{% code-tabs-item title="Example 5-52. Getting data from a related item’s pivot entry" %}
```php
$user = User::first();

$user->contacts->each(function ($contact) { 
echo sprintf(
        'Contact associated with this user at: %s',
        $contact->pivot->created_at
    );
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

你可以用as\(\)自定义中枢键,如示例5-53

{% code-tabs %}
{% code-tabs-item title="Example 5-53. Customizing the pivot attribute name" %}
```php
// User model
public function groups() {
    return $this->belongsToMany(Group::class)
     ->withTimestamps() 
     ->as('membership');
}
// Using this relationship:
User::first()->groups->each(function ($group) { 
    echo sprintf(
        'User joined this group at: %s',
        $group->membership->created_at
    );
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> 附加和分离多对多关系项目
>
> 由于您的数据透视表可以拥有自己的属性，因此您需要能够在附加多对多相关项时设置这些属性。 您可以通过将数组作为第二个参数传递来保存

```php
$user = User::first();
$contact = Contact::first();
$user->contacts()->save($contact, ['status' => 'donor']);
```

> 此外，您可以使用attach\(\)和detach\(\)，而不是传递相关项的实例，您只需传递一个ID即可。它们的工作原理与save\(\)相同，但也可以接受一个ID数组，而无需重命名像attachmany\(\)。

```php
$user = User::first();
    $user->contacts()->attach(1);
    $user->contacts()->attach(2, ['status' => 'donor']);
    $user->contacts()->attach([1, 2, 3]);
    $user->contacts()->attach([
        1 => ['status' => 'donor'],
        2,
        3,
]);
$user->contacts()->detach(1);
$user->contacts()->detach([1, 2]);
$user->contacts()->detach(); // Detaches all contacts
```

> 如果您的目标不是附加或分离，而是颠倒当前的附加状态，那么您需要toggle\(\)方法。当使用toggle\(\)时，如果给定的ID当前已附加，将被分离；如果当前已分离，将附加：

> `$user->contacts()->toggle([1, 2, 3]);`  
> 还可以使用updateExistingPivot\(\)仅对中枢记录进行更改：

```php
$user->contacts()->updateExistingPivot($contactId, [
    'status' => 'inactive',
]);
```

> 如果要替换当前关系，有效地分离所有以前的关系并附加新关系，可以将数组传递给sync\(\)。

```php
$user->contacts()->sync([1, 2, 3]);
      $user->contacts()->sync([
          1 => ['status' => 'donor'],
          2,
          3,
]);
```

**多态性**

请记住，我们的多态关系是我们有多个对应于相同关系的Eloquent类。 我们现在要使用Stars（如收藏夹）。 用户可以为联系人和事件加注星标，这称多态的来源：有多个类型的对象的单个接口

所以，我们需要三张表三个模型,Star,Contact,Event，contacts与events表保持正常,starts表包含id,starrable\_id和starrable\_type字段,对于每个星星，我们将定义哪种“类型”（例如，联系人或事件）以及该类型的ID（例如，1）

让我们创建模型,如示例5-54

{% code-tabs %}
{% code-tabs-item title="Example 5-54. Creating the models for a polymorphic starring system" %}
```php
class Star extends Model {
    public function starrable() {
        return $this->morphTo(); 
    }
}
class Contact extends Model {
    public function stars() {
        return $this->morphMany(Star::class, 'starrable');
    }
}
class Event extends Model {
    public function stars() {
        return $this->morphMany(Star::class, 'starrable'); 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

所以我们如何创建一个Star?

```php
$contact = Contact::first();
$contact->stars()->create();
```

非常简单，联系人现在被标星了.

为了找到给定联系人上的所有星，我们调用stars\(\)方法，如示例5-55所示

{% code-tabs %}
{% code-tabs-item title="Example 5-55. Retrieving the instances of a polymorphic relationship" %}
```php
$contact = Contact::first();
$contact->stars->each(function ($star) { 
    // Do stuff
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果我们有一个Star的实例，我们可以通过调用我们morphTo关系的方法来获得它的目标，在这个上下文中它是starrable\(\)，如示例5-56

{% code-tabs %}
{% code-tabs-item title="Example 5-56. Retrieving the target of a polymorphic instance" %}
```php
$stars = Star::all();
$stars->each(function ($star) {
    var_dump($star->starrable); // An instance of Contact or Event
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

最后，你可能想知道，“如果我想知道是谁星标这个联系人怎么办？”这是一个很好的问题。 这就像在星星表中添加user\_id一样简单，然后设置用户有多个星星和星星属于一个用户 - 一对多的关系（例5-57）。 星表几乎成为用户与您的联系人和活动之间的数据中枢表

{% code-tabs %}
{% code-tabs-item title="Example 5-57. Extending a polymorphic system to differentiate by user" %}
```text
class Star extends Model {
    public function starrable() {
        return $this->morphsTo; 
    }
    public function user() {
        return $this->belongsTo(User::class); 
    }
}
class User extends Model {
    public function stars() {
        return $this->hasMany(Star::class); 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

就这样！现在可以运行$star-&gt;user或$user-&gt;stars来查找用户的星列表或从星中查找星标的用户。另外，当你创建一个新的星时，可以传递用户

```php
$user = User::first();
$event = Event::first();
$event->stars()->create(['user_id' => $user->id]);
```

**多对多多态**

关系类型中最复杂和最不常见的是，多对多的多形态关系就像多态关系

这种关系类型最常见的例子是标签，我会保证它的安全，并以此为例。 让我们假设您希望能够标记联系人和事件。 多对多多态的唯一性在于它是多对多的：每个标签可以应用于多个项目，每个标记的项目可能有多个标签。 除此之外，它还是多态的：标签可以与几种不同类型的项目相关联。 对于数据库，我们将从多态关系的正常结构开始，并且添加一个数据中枢表

这意味着我们需要一个联系人表，一个事件表和一个标签表，所有表格都像普通的ID和你想要的任何属性，以及一个新的taggables表，它将包含tag\_id，taggable\_id和taggable\_type字段。 taggables表中的每个条目都将标记与其中一个可标记内容类型相关联

如示例5-58，我们将在模型上定义关系

{% code-tabs %}
{% code-tabs-item title="Example 5-58. Defining a polymorphic many-to-many relationship" %}
```text
class Contact extends Model {
    public function tags() {
        return $this->morphToMany(Tag::class, 'taggable'); 
    }
}
class Event extends Model {
    public function tags() {
        return $this->morphToMany(Tag::class, 'taggable'); 
    }
}
class Tag extends Model {
    public function contacts() {
        return $this->morphedByMany(Contact::class, 'taggable'); 
    }
    public function events() {
        return $this->morphedByMany(Event::class, 'taggable'); 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

然后创建第一个标签

```php
$tag = Tag::firstOrCreate(['name' => 'likes-cheese']);
$contact = Contact::first();
$contact->tags()->attach($tag->id);
```

然后我们像平时一样获取关系结果,如示例5-59

{% code-tabs %}
{% code-tabs-item title="Example 5-59. Accessing the related items from both sides of a many-to-many polymorphic relationship" %}
```php
$contact = Contact::first();
$contact->tags->each(function ($tag) { 
    // Do stuff
});
$tag = Tag::first();

$tag->contacts->each(function ($contact) { 
// Do stuff
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

