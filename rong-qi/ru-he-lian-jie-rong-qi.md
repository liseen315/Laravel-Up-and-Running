# 如何连接容器

在深入研究Logger类之前，请看一下示例11-4。

{% code-tabs %}
{% code-tabs-item title="Example 11-4. Laravel autowiring" %}
```php
class Bar {
    public function __construct() {}
}
class Baz {
    public function __construct() {} 
}
class Foo {
    public function __construct(Bar $bar, Baz $baz) {} 
}
$foo = app(Foo::class);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这看起来类似于例11-3中的mailer示例。 不同的是，这些依赖项（Bar和Baz）都非常简单，容器可以在没有任何深入信息的情况下解析它们。 容器读取Foo构造函数中的类型提示，解析Bar和Baz的实例，然后在创建时将它们注入新的Foo实例。 这称为自动连接：基于类型提示解析实例，而开发人员无需在容器中显式绑定这些类。

自动连接意味着，如果一个类没有被显式绑定到容器（比如在这个上下文中的foo、bar或baz），但是容器可以弄清楚如何解析它。这意味着任何没有构造函数依赖关系的类（如bar和baz）和任何具有构造函数依赖关系的类（如foo）都可以从容器中解析出来。

这使得我们只需要绑定具有不可解析的构造函数参数的类 - 例如，示例11-3中的$ logger类，它具有与我们的日志路径和日志级别相关的参数。

对于这些，我们需要学习如何将某些内容显式绑定到容器。

