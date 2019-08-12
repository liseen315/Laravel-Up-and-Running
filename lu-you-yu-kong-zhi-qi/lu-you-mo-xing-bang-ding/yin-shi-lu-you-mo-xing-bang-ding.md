# 隐式路由模型绑定

使用路由模型绑定的最简单方法是将路由参数命名为该模型特有的值（例如，将其命名为$conference而不是$id），然后在控制器闭包方法中键入该参数，并在其中使用相同的变量名。所以请看示例3-32

{% code-tabs %}
{% code-tabs-item title="Example 3-32. Using an implicit route model binding" %}
```php
Route::get('conferences/{conference}', function (Conference $conference) { 
    return view('conferences.show')->with('conference', $conference);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

由于路由参数（conference）与方法参数（$conference）相同，并且方法参数通过会议模型（Conference $conference）进行类型化提示，因此Laravel将其视为路由模型绑定。每次访问此路由时，应用程序都将假定传入URL（代替会议）的是一个ID，该ID应用于查找会议，然后生成的模型实例将传递给闭包或控制器方法。

> 为Eloquent模型自定义路由key
>
> 每当通过URL段查找Eloquent模型时（通常是因为路由模型绑定），默认Eloquent将查找它的主键（ID\)
>
> 要更改Eloquent模型用于URL查找的列，请在模型中添加名为getRouteKeyName（）的方法

```php
public function getRouteKeyName() {
    return 'slug'; 
}
```

> 现在，像conferences/{conference}这样的URL将从slug列中获取条目，而不是ID，并将相应地执行其查找

Laravel 5.2版本才添加的隐试绑定,因此你无法在之前的版本使用此功能

