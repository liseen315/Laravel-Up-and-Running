# Mix 文件夹结构

Mix的许多简单性源自于目录结构。为什么要为每一个新的应用程序设定源和编译后的资产将存放在哪里？只要遵守mix的约定，你就不必再去想它了。

每一个新的Laravel应用程序都有一个资源文件夹，在这个文件夹中，Mix将期望您的前端资源存放于此。您的sass将位于resources/sass中，或者放到resources/less中，或者您的源css位于resources/css中，而您的javascript将位于resources/js中。这些将导出到public/css和public/js内

> Laravel 5.7之前版本的资源子目录
>
> Laravel 5.7版本的sass、less和js目录嵌套在resources/assets目录下，而不是直接嵌套在resources目录下。

