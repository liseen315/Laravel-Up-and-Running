# Eloquent介绍

我们已经介绍了查询器,现在让我们谈谈Laravel在查询器上构建的旗舰数据库工具Eloquent

Eloquent是一个ActiveRecord ORM,意思就是它是个数据库抽象层提供了简单的接口与多种类型的数据库交互,”ActiveRecord“意思是单个Eloquent不仅负责与表交互\(例如:User::all\(\)获取所有用户\)还能代表表单独的行\(例如$sharon=new User\),每个实例都能够管理自己的持久性; 你可以调用$ sharon-&gt;save\(\)或$ sharon-&gt; delete\(\)

Eloquent主要聚焦于易用性,与框架的其他部分一样,它依赖于"约定胜于配置"，允许你用最少的代码构建强大的模型

例如，您可以使用示例5-17中定义的模型执行示例5-18中的所有操作。

{% code-tabs %}
{% code-tabs-item title="Example 5-17. The simplest Eloquent model" %}
```php
<?php
use Illuminate\Database\Eloquent\Model; 
class Contact extends Model {}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Example 5-18. Operations achievable with the simplest Eloquent model" %}
```php
// In a controller
public function save(Request $request)
{
    // Create and save a new contact from user input
    $contact = new Contact();
    $contact->first_name = $request->input('first_name');
    $contact->last_name = $request->input('last_name');
    $contact->email = $request->input('email');
    $contact->save();
    return redirect('contacts');
}

public function show($contactId)
{
    // Return a JSON representation of a contact based on a URL segment;
    // if the contact doesn't exist, throw an exception
    return Contact::findOrFail($contactId);
}

public function vips()
{
    // Unnecessarily complex example, but still possible with basic Eloquent
    // class; adds a "formalName" property to every VIP entry
    return Contact::where('vip', true)->get()->map(function ($contact) {
        $contact->formalName = "The exalted {$contact->first_name} of the {$contact->last_name}s";
        return $contact;
    });
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

怎么样？ 惯例。 Eloquent假设表名（Contact成为contacts），并且您拥有一个功能齐全的Eloquent模型

让我们介绍如何使用Eloquent模型

