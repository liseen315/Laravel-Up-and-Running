# 子记录更新父记录时间戳

记住，Eloquent模型默认会带created\_at和updated\_at时间戳,每次记录进行了更改updated\_at都会随之变化.

当一个关系项拥有belongsTo或者belongsToMany关系时,在相关项目更新时，将其他项目标记为已更新可能很有价值，例如一个PhoneNumber更新了,它关联的联系人也应该被标记为已更新。

我们可以将这种关系的方法名添加到子类的$touches数组中.例如5-60

{% code-tabs %}
{% code-tabs-item title="Example 5-60. Updating a parent record any time the child record is updated" %}
```php
class PhoneNumber extends Model {
    protected $touches = ['contact'];
    public function contact() {
        return $this->belongsTo(Contact::class); 
    }
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

**预加载**

默认情况下，Eloquent使用“延迟加载”加载关系。这意味着当您第一次加载模型实例时，它的相关模型将不会与它一起加载。相反，只有当您在模型上访问它们时，它们才会被加载；它们是“懒惰的”，在被调用之前不会做任何工作

如果您在模型实例列上进行迭代并且每个模型实例都有一个您正在处理的相关项目（或多个项目），则这可能会成为一个问题。 延迟加载的问题是它可能会引入大量的数据库负载（如果您熟悉该术语，通常会出现N + 1问题;如果不熟悉，请忽略此括号内容）。 例如，每次示例5-61中的循环运行时，它都会执行新的数据库查询以查找该联系人的电话号码

{% code-tabs %}
{% code-tabs-item title="Example 5-61. Retrieving one related item for each item in a list \(N+1\)" %}
```php
$contacts = Contact::all();
foreach ($contacts as $contact) {
    foreach ($contact->phone_numbers as $phone_number) {
        echo $phone_number->number; 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果您正在加载一个模型实例，并且您知道将处理它的关系，那么您可以选择“预加载”它的一个或多个相关项集

```php
 $contacts = Contact::with('phoneNumbers')->get();
```

使用with\(\)方法进行检索可以获取与所提取项相关的所有项；正如您在本例中看到的，可以将关系定义方法的名称传递给它。

当我们使用预加载时，而不是在请求时一次提取一个相关项目（例如，每次foreach循环运行时选择一个联系人的电话号码），我们有一个查询来提取初始项目（选择所有联系人）和第二个查询来提取所有相关项目。（选择我们刚提取的联系人拥有的所有电话号码）。

你可以给witch\(\)传递多个参数启用预加载关系.

```php
$contacts = Contact::with('phoneNumbers', 'addresses')->get();
```

你有可以嵌套预加载

```php
$authors = Author::with('posts.comments')->get();
```

**限制预加载**

如果你想预加载关系,但不是所有项,则可以将一个闭包传递给with\(\)，以精确定义要预先加载的相关项

```php
$contacts = Contact::with(['addresses' => function ($query) { 
    $query->where('mailable', true);
}])->get();
```

**懒预加载**

我知道这听起来很疯狂，因为我们刚刚定义了一种与懒加载相反的加载方式，但有时在拉取初始实例之前，您不知道您是否希望形成一个预加载查询。在这种情况下，您仍然能够进行单个查询来查找所有相关的项目，从而避免了N+1成本。我们称之为“懒预加载”

```php
$contacts = Contact::all();
if ($showPhoneNumbers) { 
    $contacts->load('phoneNumbers');
}
```

加载一个没有被加载的关系,使用loadMissing\(\)方法（Laravel 5.5版本后有效）

```php
$contacts = Contact::all();
if ($showPhoneNumbers) { 
    $contacts->loadMissing('phoneNumbers');
}
```

**预加载计数**

如果你想要预加载,但只希望能够访问每个关系中的项目计数，你可以使用withCount\(\)

```php
$authors = Author::withCount('posts')->get();
// Adds a "posts_count" integer to each Author with a count of that
// author's related posts
```

