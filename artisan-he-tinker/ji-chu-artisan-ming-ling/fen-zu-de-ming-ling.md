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





