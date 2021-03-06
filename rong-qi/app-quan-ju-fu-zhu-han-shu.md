# app\(\)全局辅助函数

在我们深入了解容器的实际工作方式之前，让我们快速了解从容器中获取对象的最简单方法：app\(\)辅助函数。

传递类的完全限定类名给辅助函数或者Laravel快捷方式它将返回一个类实例:

```text
$logger = app(Logger::class);
```

这是与容器交互最简单的方式，它创建了类实例并返回给你。

> 用于生成具体实例的不同语法
>
> 创建任何类或接口的具体实例的最简单方法是使用全局帮助程序并使用app（'FQCN'）将类或接口名称直接传递给帮助程序。
>
> 然而如果你有一个容器实例，不管它是在哪注入的。或者你在一个服务提供者中使用$this-&gt;app，或者你用$container = app\(\)获取一个容器实例。那么有几种方法可以从中创建一个实例。
>
> 最常见的方法是运行make\(\)方法。$app-&gt;make\('FQCN'\)工作正常。但是，您可能还会看到其他开发人员和文档有时使用这种语法：$app\['fqcn'\]。别担心。这是在做同样的事情，只是写的方式不同而已。

如所示创建Logger实例似乎很简单，但您可能已经注意到示例11-3中的$ logger类有两个参数：$logPath 和 $minimumLogLevel。 容器如何知道这里要传递什么？

简短回答:它知道，你可以用app\(\)全局辅助函数创建一个无参数构造器实例，当构造器参数复杂时，我们需要了解容器如何弄清楚构造函数参数的。



