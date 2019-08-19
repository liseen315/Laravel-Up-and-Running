# 使用访问,赋值和属性转换自定义字段

现在我们已经介绍了如果通过Eloquent从数据库存储和读取数据，那么让我们谈谈如何在Eloquent模型上装饰和操作各个属性.

访问器，变换器和属性转换都允许您自定义Eloquent实例的各个属性的输入或输出方式,Eloquent实例上的每个属性都被视为字符串,并且数据库内不存在任何可改属性的模型,但是我们可以改.

**访问器**

访问器允许你在模型实例读取数据时自定义属性,这可能是因为您想要更改特定列的输出方式，或者因为您想要创建数据库表中根本不存在的自定义属性

你可以按照如下格式书写访问器,get{PascalCasedPropertyName}Attribute,所以你的属性为first\_name那么你应该写成getFirstNameAttribute

让我们试试,首先让我们修饰一个以存在的字段,如示例5-34

{% code-tabs %}
{% code-tabs-item title="Example 5-34. Decorating a preexisting column using Eloquent accessors" %}
```php
// Model definition:
class Contact extends Model {
    public function getNameAttribute($value) {
        return $value ?: '(No name provided)'; 
    }
}
// Accessor usage:
$name = $contact->name;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

但是我们也可以使用访问器来定义数据库中从未存在的属性，例如5-35。

{% code-tabs %}
{% code-tabs-item title="Example 5-35. Defining an attribute with no backing column using Eloquent accessors" %}
```php
// Model definition:
class Contact extends Model {
    public function getFullNameAttribute() {
        return $this->first_name . ' ' . $this->last_name; 
    }
}
// Accessor usage:
$fullName = $contact->full_name;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**赋值器**

赋值器与访问器的工作方式相同,只不过它用于设置而不是获取,就像访问器一样,你可以修改已存在的字段,或者是设置数据库不存在的字段.

在模型上通过这种结果来定义赋值器set{PascalCasedPropertyName}Attribute,所以如果你的属性名为first\_name则赋值器应该命名为setFirstNameAttribute

让我们试试看。首先，我们将添加一个约束来更新先前存在的字段

{% code-tabs %}
{% code-tabs-item title="Example 5-36. Decorating setting the value of an attribute using Eloquent mutators" %}
```php
// Defining the mutator
class Order extends Model {
    public function setAmountAttribute($value) {
        $this->attributes['amount'] = $value > 0 ? $value : 0;
    }
}
// Using the mutator
$order->amount = '15'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这表明赋值器以$this-&gt;attributes以字段名设置数据

现在让我们给字段设置个代理,如示例5-37

{% code-tabs %}
{% code-tabs-item title="Example 5-37. Allowing for setting the value of a nonexistent attribute using Eloquent mutators" %}
```php
// Defining the mutator
class Order extends Model {
    public function setWorkgroupNameAttribute($workgroupName) {
        $this->attributes['email'] = "{$workgroupName}@ourcompany.com";
    }
}
// Using the mutator
$order->workgroup_name = 'jstott';
```
{% endcode-tabs-item %}
{% endcode-tabs %}

正如您可能猜到的那样，为不存在的列创建赋值器是不常见的，因为设置一个属性并让它更改不同的列可能会造成混淆 - 但这是可能的

**属性转换**

您可以想象编写访问器将所有整型字段强制转换为整型，对JSON进行编码和解码以存储在TEXT字段中，或者将tinyint 0和1与布尔值进行转换

值得庆幸的是，Eloquent已经有了一个系统。 它被称为属性转换，它允许您定义读取和写入时处理字段，就像它们是特定数据类型一样。 选项列于表5-1中

| Type | Description |
| :--- | :--- |
| int\|integer | Casts with PHP \(int\) |
| real\|float\|double | Casts with PHP \(float\) |
| string | Casts with PHP \(string\) |
| bool\|boolean | Casts with PHP \(bool\) |
| object | Parses to/from JSON, as a stdClass object |
| array | Parses to/from JSON, as an array |
| collection | Parses to/from JSON, as a collection |
| date\|datetime | Parses from databaseDATETIMEto Carbon, and back |
| timestamp | Parses from databaseTIMESTAMPto Carbon, and back |

示例5-38显示在模型上进行属性转换

{% code-tabs %}
{% code-tabs-item title="Example 5-38. Using attribute casting on an Eloquent model" %}
```php
class Contact {
    protected $casts = [
         'vip' => 'boolean',
         'children_names' => 'array', 
         'birthday' => 'date',
    ]; 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**日期转换**

你可以选择将特定的字段添加到$dates数组作为时间戳,如示例5-39

{% code-tabs %}
{% code-tabs-item title="Example 5-39. Defining columns to be mutated as timestamps" %}
```php
class Contact {
    protected $dates = [ 
        'met_at',
    ]; 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

默认数组会默认带created\_at和updated\_at。因此向dates添加条目只会将它们添加到列表中

但是，将列添加到此列表并将它们添加到$ this-&gt; casts作为时间戳之间没有区别，因此现在属性转换可以转换时间戳（Laravel 5.2以来的新增内容）这已成为一个不必要的功能



