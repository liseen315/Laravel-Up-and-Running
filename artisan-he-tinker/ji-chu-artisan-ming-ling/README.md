# 基础Artisan命令

这里没有足够的空间来覆盖所有的Artisan命令，但是我们将覆盖其中的许多命令。让我们从基本命令开始

_clear-compiled_

删除Laravel编译的类文件，这类似于内部Laravel缓存；当出现问题并且不知道原因时，将其作为首选方法运行

_down,up_

将应用程序置于"维护模式"，然后你可以修复错误，运行迁移或其他操作。然后从维护模式恢复程序。

_dump-server \(5.7+\)_

启动dump server\(查看218页的"Laravel Dump Server"\),用于收集和输出dump变量。

_env_

查看当前应用运行的环境，相当于app\(\)-&gt;environment\(\)

_help_

为命令提供帮助，例如_php artisan help commandName_

_migrate_

运行所有数据库迁移

_optimize_

清理和刷新配置和路由文件

_preset_

更改前端脚手架

_serve_

在localhost:8000启动一个PHP server \(你可以用--host和--port自定义域名跟端口\)

tinker

打开Tinker REPL，我们在本章后面介绍

> 在Laravel的发展过程中，Artisan命令及其名称的列表发生了微小的变化。我会随时注意到它们的变化，但这里的一切对于Laravel5.8来说都是最新的。如果您不在5.8中工作，查看可用内容的最佳方法是运行php artisan。







