# 启动应用程序

每个Laravel应用程序在web server级别都有一些配置，Apache 的.htaccess文件或者Nginx配置，或者一些类似的东西，这些配置捕获Web请求，并将其路由到Laravel的public/index.php目录。

index.php实际没那么多代码，它有三个主要功能。

首先，它加载Composer的autoload文件，该文件注册所有Composer加载的依赖项。

> Composer 和 Laravel
>
> Laravel的核心功能被划分到Illuminate命名空间下的一系列组件内。这些组件被Composer拉到Laravel内。Laravel还从Symfony和其他几个社区开发的包中提取了相当多的包。这样一来，Laravel实际是汇集众多组件的框架。

接下来，它启动了Laravel的引导程序，创建了应用程序容器（您将在第11章中了解更多关于容器的内容），并注册了一些核心服务（包括内核，我们将在稍后讨论）。

最后，创建了一个内核实例，创建一个请求代表当前用户的web请求,然后传递请求到内核处理，内核返回一个Illuminate响应对象然后返回给终端用户，然后内核终止请求。

**Laravel内核**

内核是每个Laravel应用程序的核心路由器，负责接收用户请求，通过中间件进行处理，处理异常并将其传递给页面，然后返回最终响应。实际上有两个内核，一个用于页面Web请求，另外一个用于控制台, cron, 和 Artisan 请求\(称之为控制台内核\)，每一个内核都有个handle\(\)方法负责接收Illuminate请求对象然后返回一个Illuminate响应对象。

内核运行每个请求之前需要运行的所有引导程序，包括确定当前请求正在运行的环境（试运行、本地、生产等）和运行所有服务提供者。HTTP内核还定义了将包装每个请求的中间件列表，包括负责会话和CSRF保护的核心中间件。

