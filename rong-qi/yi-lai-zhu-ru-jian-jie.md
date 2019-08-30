# 依赖注入简介

依赖注入意味着，不是在类内实例化\("newed up"\)，而是在从外部注入类的依赖。常见情况是构造函数注入，意思是在对象创建时注入依赖，但是也有setter注入，意思是类暴露特殊方法用于注入给定的依赖，以及方法注入，意味着一个或者多个方法期望在他们被调用时注入依赖。

查看示例11-1的构造函数注入,常见的依赖注入类型。

{% code-tabs %}
{% code-tabs-item title="Example 11-1. Basic dependency injection" %}
```php
<?php
class UserMailer {
    protected $mailer;
    public function __construct(Mailer $mailer) {
        $this->mailer = $mailer;
    }
    public function welcome($user) {
        return $this->mailer->mail($user->email, 'Welcome!'); 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

可以看出，UserMailer类期望在它实例化时一个类型为Mailer的对象被注入。然后引用实例的方法。

依赖注入的主要好处是它使我们可以自由地改变我们注入的内容。为测试模拟注入，为共享使用实例化共享依赖。

> 控制反转
>
> 您可能听说过“反向控制”和“依赖注入”一起使用的情况，有时Laravel的容器称为ioc容器。
>
> 这两个概念非常相似，控制反转引入了这样的思想：在传统编程中，低层代码-具体类，实例和流程代码-"控制"哪个特定的模式或者接口的实例会被使用。例如你在类中实例化mailer，每个类都将决定使用Mailgun 或者 Mandrill 或者 Sendgrid。
>
> 控制反转的意思是将"控制"进行倒转，通常在配置中定义哪个mailer存放于高层、最抽象的级别上。每一个实例，每一段低级代码，都在查找高级配置，本质上是“ask”：“你能给我一个邮件发送程序吗？”他们“不知道”他们得到的是哪一个邮递员，只知道他们得到了一个。
>
> 依赖注入，尤其是DI容器为控制反转提供了一个很好的机会，例如你可以定义当注入mailers到所用的类中时，哪个mailers实例会被提供。

