# 使用Eloquent插入和更新

Eloquent的插入与更新与传统的查询器语法变的有些不同了。

**插入**

使用Eloquent有两种主要的方法插入新记录

首先,使用Eloquent class创建新实例，然后手动设置属性,最后调用save\(\)方法,如示例5-21

{% code-tabs %}
{% code-tabs-item title="Example 5-21. Inserting an Eloquent record by creating a new instance" %}
```php
$contact = new Contact; 
$contact->name = 'Ken Hirata';
$contact->email = 'ken@hirata.com';
$contact->save();
// or

$contact = new Contact([
    'name' => 'Ken Hirata',
    'email' => 'ken@hirata.com',
]);
$contact->save();
// or
$contact = Contact::make([
    'name' => 'Ken Hirata',
    'email' => 'ken@hirata.com',
]);
$contact->save();
```
{% endcode-tabs-item %}
{% endcode-tabs %}

在调用save\(\)前,实例仅表示是一个Contact实例,并没有存储到数据库，这意味着它没有id,如果应用退出并没有持久化，也不会设置created\_at和updated\_at值。

你还可以传递数组给Model::create\(\)，如示例5-22,与make不同的是,create\(\)调用后立即保存数据到数据库.

{% code-tabs %}
{% code-tabs-item title="Example 5-22. Inserting an Eloquent record by passing an array to create\(\)" %}
```php
$contact = Contact::create([
    'name' => 'Keahi Hale',
    'email' => 'halek481@yahoo.com',
]);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

需要注意的是,传递数组的上下文中\(new Model\(\), Model::make\(\), Model::create\(\), or Model::update\(\)\)每个通过Model::create\(\)设置的属性必须被"批量赋值"批准,稍后我们将介绍,对于示例5-21中的第一个示例来说，这是不必要的，您可以在其中分别分配每个属性

记住如果你使用create\(\)则不需要调用save\(\)，它将作为create\(\)的一部分处理

**更新**

更新记录与插入非常相似。您可以获取一个特定的实例，更改其属性，然后保存，或者您可以进行一次调用并传递一个更新的属性数组。示例5-23说明了第一种方法

{% code-tabs %}
{% code-tabs-item title="Example 5-23. Updating an Eloquent record by updating an instance and saving" %}
```php
$contact = Contact::find(1);
$contact->email = 'natalie@parkfamily.com';
$contact->save();
```
{% endcode-tabs-item %}
{% endcode-tabs %}

由于记录已经存在,created\_at时间戳和id保持不变,但是updated\_at字段将变成当前的日期和时间,示例5-24说明了第二种方法

{% code-tabs %}
{% code-tabs-item title="Example 5-24. Updating one or more Eloquent records by passing an array to the update\(\) method" %}
```php
Contact::where('created_at', '<', now()->subYear())
    ->update(['longevity' => 'ancient']);
// or
$contact = Contact::find(1);
$contact->update(['longevity' => 'ancient']);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

此方法需要一个数组，其中每个键都是列名，每个值都是列值。

**批量赋值**

我们已经研究关于如何将数组传递给Eloquent类方法的示例,但是实际在正式填充数据前,这些字段都不起作用

这样做的目的是保护用户输入与你期望不符的数据,考虑示例5-25场景

{% code-tabs %}
{% code-tabs-item title="Example 5-25. Updating an Eloquent model using the entirety of a request’s inpu" %}
```php
// ContactController
public function update(Contact $contact, Request $request) {
    $contact->update($request->all());
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果您不熟悉Illuminate请求对象，示例5-25将获取每一个用户输入并将其传递给update\(\)方法。all\(\)方法包含URL参数和表单输入等内容，因此恶意用户可以很容易地在其中添加一些内容，如ID和所有者ID，而您不希望更新这些内容。

值得庆幸的是，在定义模型的可填充字段之前，这实际上不会起作用。 您可以将可填写字段列入白名单，或者将“保护”字段列入黑名单，以确定哪些字段可以通过“批量赋值”进行编辑或不可以进行编辑,注意未填充属性依然可以通过直接赋值更改\(例如,$contact-&gt;password = 'abc';\)，示例5-26显示如下:

{% code-tabs %}
{% code-tabs-item title="Example 5-26. Using Eloquent’s fillable or guarded properties to define mass-assignable fields" %}
```php
class Contact {
    protected $fillable = ['name', 'email'];
    // or
    protected $guarded = ['id', 'created_at', 'updated_at', 'owner_id']; 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> 使用Request::only\(\)
>
> 示例5-25我们使用Request对象的all\(\)接收了用户的整体输入,这是批量赋值很好的工具,但是也有个技巧用于接收用户的旧输入.
>
> request类有一个only\(\)方法，它只允许您从用户输入中提取几个键。所以现在你可以这样做了

> Contact::create\($request-&gt;only\('name', 'email'\)\);

**firstOrCreate\(\)和firstOrNew\(\)**

有时你想告诉应用程序”帮我创建一个带属性的示例,如果不存在就帮我创建“所以firstOr\*\(\)方法出现了。

firstOrCreate\(\) 和 firstOrNew\(\)将键值数组作为第一个参数

```php
$contact = Contact::firstOrCreate(['email' => 'luis.ramos@myacme.com']);
```

它们都会查找并检索匹配这些参数的第一条记录，如果没有匹配的记录，它们将创建一个具有这些属性的实例; firstOrCreate\(\)将该实例持久保存到数据库然后返回它，而firstOrNew\(\)将返回它而不保存它

如果将值数组作为第二个参数传递，则这些值将添加到创建的条目中（如果已创建）但不会用于查找条目是否存在

