# 服务提供者

虽然这些引导程序内有一些过程代码，但是几乎所有的Laravel的引导代码都被划分为服务调用者。服务提供者是一个类，它封装了应用程序的各个部分，用于引导它们的核心功能。

例如，AuthServiceProvider用于引导Laravel的身份认证系统所需的注册，RouteServiceProvider用于引导路由系统。

首先服务提供者的概念可能有点难以理解，因此请这样思考，当应用初始化时，应用的组件需要被引导。服务提供者是一个将引导打包到类的工具。如果你的代码要为应用准备运行提供准备的话，那么可以让它当做服务提供者。

例如，你发现需要在容器内注册一些类（你将在第11章学习更多相关内容），你需要为此创建一个服务提供者，你将可能有GitHubServiceProvider和MailerServiceProvider。

**服务提供者的boot\(\),register\(\)和deferring**

服务提供者有两个重要的方法：boot\(\)和register\(\)，\(5.8+\)还提供了DeferrableProvider接口，5.7+版本的$defer属性。你可以选择使用。

首先，当你想要绑定类和aliases到容器的时候,服务提供者的register\(\)会被调用。你不应在register\(\)中做任何依赖于整个应用程序被引导的事情。

其次，所有服务提供者的boot\(\)方法会被调用，在此你可以执行其他引导。例如绑定时间跟定义路由，依赖于整个应用已经被引导。

如果你的服务提供者只打算在容器中注册绑定（即，教容器如何解析给定的类或接口），而不执行任何其他引导，则可以“延迟”其注册，这意味着除非其中一个绑定显式地从容器请求。这可以加快应用程序的平均引导时间。

如果打算推迟服务提供者注册,在5.8+你可以实现Illuminate\Contracts\Support\DeferrableProvider接口，在5.7及之前版本,你需要设置$defer属性为true，然后在所有版本中，给服务提供者提供一个provides\(\)方法然后返回提供者绑定列表，如示例10-1

{% code-tabs %}
{% code-tabs-item title="Example 10-1. Deferring the registration of a service provider" %}
```php
use Illuminate\Contracts\Support\DeferrableProvider;
class GitHubServiceProvider extends ServiceProvider implements DeferrableProvider {
    public function provider() {
        return [
            GitHubClient::class,
        ]
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> 服务提供者的更多用途。
>
> 服务提供商还有一套方法和配置选项，当提供者作为Composer程序包的一部分发布时，可以为最终用户提供高级功能。 查看Laravel源代码中的服务提供者定义，了解更多详细信息。

现在，我们已经介绍了应用的引导，让我们看一下请求对象，引导最重要的输出

