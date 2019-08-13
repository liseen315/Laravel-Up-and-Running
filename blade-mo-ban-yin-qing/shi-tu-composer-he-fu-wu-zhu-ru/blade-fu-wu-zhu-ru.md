# Blade 服务注入

通常我们会给视图注入三种类型的数据:要迭代的数据集合,页面显示用的单独对象,以及服务生成的数据或者视图.

对于服务来说,这个模式很可能类似于示例4-26,我们在路由方法内注入了一个分析服务实例,然后传递给视图.

{% code-tabs %}
{% code-tabs-item title="Example 4-26. Injecting services into a view via the route definition constructor" %}
```php
Route::get('backend/sales', function (AnalyticsService $analytics) { 
    return view('backend.sales-graphs')
        ->with('analytics', $analytics);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

与视图composer类似,Blade的服务注入提供了一个快捷方式,用于减少在路由内减少重复定义,通常如示例4-27所示

{% code-tabs %}
{% code-tabs-item title="Example 4-27. Using an injected navigation service in a view" %}
```text
<div class="finances-display">
     {{ $analytics->getBalance() }} / {{ $analytics->getBudget() }}
</div>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Blade服务注入使得从容器注入类实例变得异常简单,如示例4-28

{% code-tabs %}
{% code-tabs-item title="Example 4-28. Injecting a service directly into a view" %}
```php
@inject('analytics', 'App\Services\Analytics')

<div class="finances-display">
     {{ $analytics->getBalance() }} / {{ $analytics->getBudget() }}
</div>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如例子所示,@inject指令使得$analytics在视图内可用.

@inject的第一个参数是您要注入的变量的名称，第二个参数是您要注入实例的类或接口。 解决这个问题就像在Laravel中的其他地方的构造函数中键入依赖关系一样; 如果您不熟悉其工作原理，请查看第11章以了解更多信息

如视图Composer一样,Blade服务注入可以轻松地为视图的每个实例提供某些数据或功能，而不需要每次在路由定义注入它.

