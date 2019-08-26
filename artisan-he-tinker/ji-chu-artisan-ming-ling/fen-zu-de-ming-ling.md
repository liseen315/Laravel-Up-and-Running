# 分组的命令

开箱即用的命令行按照上下文分组，我们不会全部介绍它们，但是会粗略的介绍每个上下文。

_app_

它只有一条命令app:name，允许你设置顶级命令空间，例如php artisan app:name MyApplication，我建议避免使用此功能，并保持你的根命名空间为App

_auth_

只有一条命令auth:clear-resets，用于从数据库中刷新所有过期的密码重置令牌。

_cache_

cache:clear _清除缓存,_ cache:forget 从缓存移除单个项, and cache:table 如果你计划使用数据库缓存，会创建一个数据库迁移。

_config_

config:cache 缓存配置设置以加快查找速度; _清理缓存配置_使用 config:clear.

_db_

db:seed 填充数据库,如果你已经配置数据库填充。

_event_

event:generate 根据EventServiceProvider中的定义构建缺少的事件和事件侦听器文件，在16章学习关于事件知识。

_key_

key:generate 创建随机应用程序加密密钥到.env文件。

> 重复运行Artisan密钥：生成意味着丢失一些加密数据
>
> 如果在应用程序上运行php-artisan-key:generate多次，则当前登录的每个用户都将注销。此外，手动加密的任何数据都将不再可解密。要了解更多信息查看Tightenite Jake Bathman的”[APP\_KEY and You](https://tighten.co/blog/app-key-and-you)“

_make_

make:auth 为登录页面、用户仪表板以及登录和注册页面提供视图和相应的路由。

make: actions 剩余部分创建单独的项。要进一步了解任何单个命令的参数，请使用“帮助”阅读其文档。

例如，你可以运行`php artisan help make:migration`了解--create=tableNameHere用于创建一个迁移，代表在迁移文件内有create table语法，比如`php artisan make:migration create_posts_table --create=posts`

_migrate_

之前提到了用于迁移的命令，但还有几个关于迁移的命令，你可以使用migrate:install创建migrations表\(用于跟踪执行的迁移\)，用migrate:reset重置迁移，使用migrate:refresh重置迁移然后重新运行迁移，用migrate:rollback回滚迁移，用migrate:fresh移除所有表，然后从新运行迁移，或者使用migrate:status查看迁移状态。

_notifications_

notifications:table创建迁移,然后生成一个表用于数据库通知。

_package_

在Laravel 5.5之前版本,要包含一个新的Laravel特性包需要手动在config/app.php内注册。在5.5版本可以自动找到这些包，而不需要手动注册。package:discover重建Laravel的"discovered"自动发现外部包。

_queue_

我们会在第16章介绍队列，队列的意思是将job推送到队列然后由worker挨个执行。这一组命令提供与队列相关的交互:例如queue:listen用于侦听队列，queue:table为数据库支持的队列创建迁移，queue:flush用于刷新失败队列，还有很多，你将在第16章中了解到。

_route_

如果你运行route:list，你会看到应用内路由的定义，包括路由的动词，路径，名字，控制器/闭包动作，以及中间件。可以使用route:cache缓存路由定义以加快查找速度，你可以用route:clear清除路由缓存。

_schedule_

我们将在16章介绍类似cron的任务调度，为了运行调度，你需要设置系统cron运行调度:一分钟运行一次。

\* \* \* \* \* php /home/myapp.com/artisan schedule:run &gt;&gt; /dev/null 2&gt;&1

如您所见，此Artisan命令旨在定期运行，以便为核心Laravel服务提供动力。

_session_

使用数据库会话为应用创建会话

_storage_

storage:link创建一个public/storage 到 storage/app/public 的symbolic link，这是Laravel应用程序中的常见惯例，可以轻松地将用户上载（或通常最终存储在应用程序中的其他文件）放在可以在公共URL上访问的位置。

_vendor_

一些特定于Laravel的软件包需要“发布”它们的一些_资源_，以便它们可以从您的公共目录提供，或者您可以修改它们。 无论哪种方式，这些软件包都会将这些“可发布的资源”注册到Laravel，当您运行vendor:publish时，它会将它们发布到指定的位置.

_view_

Laravel视图渲染引擎自动缓存视图。它通常能够很好地处理自己的缓存失效，但是如果您发现它卡住了，请运行view:clear清除缓存  






