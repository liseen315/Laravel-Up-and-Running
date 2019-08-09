# 将依赖注入到控制器

Laravel的facades以及全局辅助函数为Laravel代码库中的类提供了简单的接口，你可以获得当前请求,用户输入,会话,缓存等.

但是,如果你喜欢注入依赖,或者你想使用没有facade或者辅助函数的服务,那么你需要找到一些方法将这些类的示例注入到控制器.

这是我们第一次接触到Laravel的服务容器概念,现在如果你不熟悉,你可以把它看做是Laravel的小魔术，或者你想知道具体的运作方式你可跳到第11章

所有控制器方法\(包括构造函数\)都是从Laravel容器中解析出来的,这意味着你键入的任何内容都暗示容器进行注入.

> PHP的Typehints
>
> PHP中的"Typehinting"意思是把类或者接口放到变量前面
>
> public function \_\_construct\(Logger $logger\) {}
>
> 这个typehint告诉php无论传递到方法中的是什么，都必须是logger类型，它可以是接口或类。

作为好的例子,你应该喜欢用Request实例代替使用全局的辅助函数,就像方法参数内使用typehint `Illuminate\Http\Request`

{% code-tabs %}
{% code-tabs-item title="Example 3-27. Controller method injection via typehinting" %}
```php
// TasksController.php
...
public function store(\Illuminate\Http\Request $request) {
    Task::create($request->only(['title', 'description'])); 
    return redirect('tasks');
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

所以，您已经定义了一个传递到store\(\)方法中的必选参数。而且Laravel知道如何解析这个类名，那么您就可以在方法中使用请求对象，而不需要进行任何工作。没有显式绑定，没有其他任何内容，它只是作为$request变量存在。

并且，通过比较示例3-26和示例3-27可以看出，request\(\)辅助函数和request对象的行为完全相同



