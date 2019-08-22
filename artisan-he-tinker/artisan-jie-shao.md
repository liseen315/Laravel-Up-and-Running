# Artisan 介绍

如果你逐章读过本书,实际你已经学习如何使用Artisan命令了。看起来如下:

`php artisan make:controller PostsController`

如果你查看应用程序根目录，你会发现Artisan实际只是一个PHP文件。当你用php调用artisan，然后你传递此文件给PHP解析，之后的一切都作为参数传给Artisan。

> Symfony控制台语法
>
> Artisan实际上是Symfony控制台组件的上层封装。如果你熟悉Symfony，那么这些都很容易理解。

由于应用程序的Artisan命令列表可以通过包或应用程序的特定代码进行更改，因此您值得检查遇到的每个新应用程序以查看可用的命令。

要获取所有可用的Artisan命令,你需要在根目录运行 php artisan list \(虽然你运行 php artisan 也一样\)

