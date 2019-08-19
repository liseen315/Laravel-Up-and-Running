# 使用Eloquent删除

使用Eloquent删除与更新非常相似,但是可以使用\(可选\)软删除,你可以将已删除的存档,以便后期恢复

**普通删除**

一种简单的方式删除模型记录是调用实例上的delete\(\)方法

```php
$contact = Contact::find(5);
$contact->delete();
```

然而，如果你仅有ID，那么就没有理由只删除它了,你可以直接给destroy\(\)方法传递一个ID或者是ID数组来直接清除它们

```php
Contact::destroy(1);
// or
Contact::destroy([1, 5, 7]);
```

最后，你可以删除整个查询结果

```php
Contact::where('updated_at', '<', now()->subYear())->delete();
```

**软删除**

软删除将数据库记录标记为已删除，而不实际从数据库中删除它们。 这使您能够在以后检查它们，当显示历史信息时显示“无信息，删除”，并允许你的用户（或管理员）恢复部分或全部数据

手动处理软删除的难点在于,你编写的每个查询都要排除软删除数据,幸好,如果你使用Eloquent的软删除,你所做的每个查询默认都忽略软删除,除非你明确指定.

Laravel的软删除功能需要给表添加一个deleted\_at列,一旦您在这个Eloquent模型上启用了软删除，您编写的每个查询（除非您明确地包括软删除的记录）都将被限定为忽略软删除的行

> 何时应该用软删除
>
> 仅仅因为某个特性存在，并不意味着你应该总是使用它。Laravel社区中的许多人默认在每个项目上使用软删除，这仅仅是因为该特性存在。但是，软删除是有实际成本的。如果你使用诸如Sequel Pro工具来查看数据库，你可能忘记检查deleted\_at字段，一旦你忘记清除软删除记录,你的数据库会变的越来越大。
>
> 我的建议是：默认情况下不要使用软删除。相反，在需要时，尽可能使用类似[Quicksand](https://github.com/tightenco/quicksand)的工具来清除旧的软删除。软删除功能是一个强大的工具，但除非您需要，否则不值得使用。

**启用软删除**

你可以通过三种办法来在迁移内添加deleted\_at字段,在模型中导入SoftDeletes特性,以及给你的$dates添加属性deleted\_at，在模式上的softDeletes\(\)可以将deleted\_at添加到表,如示例5-27,示例5-28显示Eloquent模型启用软删除。

{% code-tabs %}
{% code-tabs-item title="Example 5-27. Migration to add the soft delete column to a table" %}
```php
Schema::table('contacts', function (Blueprint $table) { 
    $table->softDeletes();
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Example 5-28. An Eloquent model with soft deletes enabled" %}
```php
<?php
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\SoftDeletes;
class Contact extends Model {
    use SoftDeletes; // use the trait
    protected $dates = ['deleted_at']; // mark this column as a date
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

一旦进行了这些更改，现在每次delete\(\)和destroy\(\)调用都会将行上的deleted\_at列设置为当前日期和时间，而不是删除该行。所有以后的查询都将排除该行

**软删除查询**

所以，我们如何获取软删除？

首先，你可以给查询添加soft-deleted项目

```text
$allHistoricContacts = Contact::withTrashed()->get();
```

接下来，可以使用trashed\(\)方法查看特定实例是否被软删除：

```php
if ($contact->trashed()) { 
    // do something
}
```

最后，获取软删除的项

`$deletedContacts = Contact::onlyTrashed()->get();`

**恢复软删除**

如果要还原软删除项，可以对实例或查询运行restore\(\)。

```php
$contact->restore();
// or
Contact::onlyTrashed()->where('vip', true)->restore();
```

**强制删除软删除项**

你可以在实例或者查询调用forceDelete\(\)强制删除

```php
$contact->forceDelete();
// or 
Contact::onlyTrashed()->forceDelete();
```



