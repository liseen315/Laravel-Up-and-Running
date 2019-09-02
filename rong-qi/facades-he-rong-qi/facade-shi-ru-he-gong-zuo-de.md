# Facade是如何工作的

让我们看看缓存facade看看它是如何工作的。

首先，打开Illuminate\Support\Facades\Cache类，你将看到类似如示例11-15的内容。

{% code-tabs %}
{% code-tabs-item title="Example 11-15. The Cache facade class" %}
```php
<?php
namespace Illuminate\Support\Facades;
class Cache extends Facade {
    protected static function getFacadeAccessor() {
        return 'cache'; 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

每个facade都有一个方法：getFacadeAccessor\(\)。 它定义了Laravel用于从容器中查找此Facade的支持实例的key。

在这个实例中，我们可以看到每次调用缓存facade都被代理至从容器的一个缓存快捷方式实例上。当然这不是一个真正的class或者是接口名，所以我们知道它是我前面提到的快捷方式之一。

所以，真正发生了什么。

```php
Cache::get('key');
// Is the same as...
app('cache')->get('key');
```

有几种方法可以精确地查找每个facade访问器指向的类，但是检查文档是最简单的。Facades文档页面上有一个表，显示了每个Facades所连接的容器绑定（快捷方式，如缓存）以及返回的类。看起来像这样.

| Facade | Class | Service container binding |
| :--- | :--- | :--- |
| App | Illuminate\Foundation\Application | app |
| ... | ... | ... |
| Cache | Illuminate\Cache\CacheManager | cache |

有了参考你可以做如下三种事

首先，您可以确定facade上可用的方法。只要找到它的支持类并查看该类的定义，就可以知道在facade可调用的公开方法。

然后，你可以确定如何用依赖注入注入到一个facade支持的类。如果你想要facade功能但更喜欢用依赖注入，只需要facade支持的完全限定类或者用app\(\)创建的实例并在facade上调用相同的方法。

其次，你可以看到如何创建一个你自己的facade，创建一个facade继承自Illuminate\Support\Facades\Facade并给它一个getFacadeAccessor方法，该方法返回一个字符串。使该字符串可用于从容器中解析您的支持类 - 可能只是该类的FQCN。 最后，您必须通过将其添加到config / app.php中的别名数组来注册facade。 完成！ 你刚刚建立了自己的facade。



