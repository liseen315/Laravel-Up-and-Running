# 使用Eloquent检索数据

大多数情况用Eloquent从数据库提取数据时,你将会调用Eloquent模型上的静态方法.

让我们开始吧:

`$allContacts = Contact::all();`

这非常简单,让我们添加点过滤:

`$vipContacts = Contact::where('vip', true)->get();`

我们可以看到，Eloquent facade能让我们使用链式约束，这一切都非常的熟悉

```php
$newestContacts = Contact::orderBy('created_at', 'desc')
        ->take(10)
        ->get();
```

事实上一旦你访问了初始化的facade名字,你就在使用Laravel的查询器,稍后你将了解更多内容,你在DB facade上所能做的一切同样都可以作用到Eloquent对象上.

**取一条记录**

与本章前面介绍的一样，您可以使用first\(\)返回查询中的第一条记录，或者使用find\(\)根据ID提取记录。对于这两种方法中的任何一种，如果在方法名称中附加“orfail”，如果没有匹配的结果，它都会抛出异常。这使得findorfail\(\)称为通过URL段查找实体（或者如果不存在匹配的实体，则抛出异常）的常用工具，如示例5-19所示

{% code-tabs %}
{% code-tabs-item title="Example 5-19. Using an Eloquent OrFail\(\) method in a controller method" %}
```php
// ContactController
public function show($contactId)
{
    return view('contacts.show')
        ->with('contact', Contact::findOrFail());
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

返回单条记录的方法\(first\(\), firstOrFail\(\), find\(\), or findOrFail\(\)\)将返回一个Eloquent实例,所以Contact::first\(\)将返回Contact实例,其中包含表内第一行实例

> 异常
>
> 如示例5-19所示,我们不需要在控制器内捕获Eloquent的模型未找到异常\(Illuminate\Database\Eloquent \ModelNotFoundException\)Laravel的路由系统将会捕获并为我们抛出404
>
> 当然，如果你愿意你可以捕获并抓住这个异常处理它

**获取多条记录**

Eloquent调用get\(\)与查询器调用上一样-创建一个查询器跟着调用get\(\)获取结果

`$vipContacts = Contact::where('vip', true)->get();`

然而,这有一个Eloquent独一的方法,all\(\),你竟然会看到这个方法,它用于获取未过滤的所有数据

`$contacts = Contact::all();`

> 使用get\(\)替换all\(\)
>
> 有时你可以使用all\(\)你也可以使用get\(\),Contact::get\(\)与Contact::all\(\)返回相同数据,但是当你添加过滤条件的时候,all\(\)将不会过滤,get\(\)就会
>
> 虽然all\(\)很常见,但是我依然推荐使用get\(\)来进行查询

另一个不同之处是，在Laravel 5.3之前，它返回了一个模型数组而不是一个集合。在5.3和更高版本中，它们都返回集合

**使用chunk\(\)进行分块响应**

如果您需要一次处理大量（数千或更多）记录，则可能会遇到内存或锁定问题。Laravel可以将您的请求分成小块（块）并批量处理，使您的大请求的内存负载保持较小。示例5-20说明了使用chunk（）将查询拆分为每个100条记录的“chunk”。

{% code-tabs %}
{% code-tabs-item title="Example 5-20. Chunking an Eloquent query to limit memory usage" %}
```php
Contact::chunk(100, function ($contacts) { 
    foreach ($contacts as $contact) {
        // Do something with $contact
    } 
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**合并**

查询器上可用的聚合也可以在Eloquent查询中使用,例如

```php
$countVips = Contact::where('vip', true)->count(); 
$sumVotes = Contact::sum('votes');
$averageSkill = User::avg('skill_level');
```

