# Eloquent集合

当您在Eloquent中进行任何有可能返回多行的查询时，它们将打包在Eloquent集合中，这是一种特殊类型的集合。 让我们来看看集合和Eloquent集合，以及它们比普通数组更好的原因

**集合基础**

Laravel的集合对象\(Illuminate\Support\Collection\)有点像steroids的数组,他们在类似数组的对象上公开的方法非常有用，一旦用了一段时间,你非常希望将它们用于非Laravel的项目中,你可以使用[Tightenco/Collect](https://github.com/tightenco/collect)包

创建集合最简单的方法是使用collect\(\)复制函数,传递一个数组或者使用不带参数创建空集合,然后推送到其中,我们试试看。

`$collection = collect([1, 2, 3]);`

现在假设我们要过滤偶数

```php
$odds = $collection->reject(function ($item) { 
    return $item % 2 === 0;
});
```

或者我们想要集合的每个元素乘以10.

```php
$multiplied = $collection->map(function ($item) { 
    return $item * 10;
});
```

我们甚至可以获取偶数然后将它们乘以10最后通过sum\(\)返回一个数字

```php
$sum = $collection->filter(function ($item)
{
    return $item % 2 == 0;
})->map(function ($item)
{
    return $item * 10;
})->sum();

```

如您所见，集合提供了一系列方法，可以选择链接这些方法来对数组执行函数操作。它们提供与本机php方法（如array\_map\(\)和array\_reduce\(\)）相同的功能，但是你不必记住PHP的不可预测的参数顺序，并且方法链接语法无限可读

在集合上有超过60个可用的方法,比如max\(\), whereIn\(\), flatten\(\), 和flip\(\),在这里没有足够的篇幅全都说明,我们将在第17章讨论更多内容，或者可以查看[Laravel Collections](https://laravel.com/docs/master/collections)文档以查看所有方法

> 数组中的集合
>
> 集合也可以在任何可以使用数组的上下文中使用（typehinting除外）; 它们允许迭代，因此您可以将它们传递给foreach，并且它们允许数组访问，因此如果它们有键，您可以尝试$ a = $ collection \['a'\]

**Eloquent集合增加了什么**

Eloquent集合就是一个普通的集合,只不过拓展了特殊需求.

还是没有足够的空间来说明全部增加的内容,例如每个Eloquent集合有一个keys\(\)方法返回集合实例的主键数组,find\($id\)用于查找主键为$id的实例

还有一个额外的特性是,模型可以返回指定集合类的包装结果,因此如果你想给订单模型添加特定方法,你可以创建一个继承自Illuminate\Database\Eloquent\Collection的OrderCollection然后在模型中注册,如示例5-40

{% code-tabs %}
{% code-tabs-item title="Example 5-40. Custom Collection classes for Eloquent models" %}
```php
...
class OrderCollection extends Collection {
    public function sumBillableAmount() {
        return $this->reduce(function ($carry, $order) {
            return $carry + ($order->billable ? $order->amount : 0);
        }, 0); 
    }
}

class Order extends Model {
    public function newCollection(array $models = []) {
        return new OrderCollection($models); 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

现在你可以随时获取订单集合\(例如:Order::all\(\)\),它实际是OrderCollection类实例.

```php
$orders = Order::all();
$billableAmount = $orders->sumBillableAmount();
```

