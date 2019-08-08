# 路由组

通常，一组路由共享一个特定的特性——特定的身份验证需求、路径前缀，或者控制器命名空间。在每个路由上一次又一次地定义这些共享特性，不仅看起来很乏味，而且还可能混淆路由文件的形状，并模糊应用程序的某些结构。

路由组允许您将多个路由组合在一起，并对整个组应用一次共享配置设置,用来减少重复配置.

要将两个或多个路由组合在一起，可以使用路由组“包围”路由定义，如示例3-10所示。实际上，您实际上是将一个闭包传递给组定义，并在该闭包中定义分组的路由。

{% code-tabs %}
{% code-tabs-item title="Example 3-10. Defining a route group" %}
```php
Route::group(['prefix' => ''],function (){
   Route::get('hello',function () {
       return 'Hello';
   });
   Route::get('world',function () {
       return 'World';
   });
});
// 译者注,书中作者在group方法的第一个参数缺失,会导致运行报错
```
{% endcode-tabs-item %}
{% endcode-tabs %}

默认情况下，路由组实际上不执行任何操作,上面的代码与分开定义路由没有什么区别

