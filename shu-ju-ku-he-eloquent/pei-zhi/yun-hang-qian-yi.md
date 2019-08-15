# 运行迁移

一旦迁移定义好,如何运行它们?采用如下的Artisan命令:

`php artisan migrate`

此命令运行所有"未完成"的迁移\(运行每个迁移的up\(\)方法\),Laravel会跟踪你运行过哪些迁移,哪些没运行过,每次运行此命令时,它会检测是否运行过所有可用的迁移，如果没有运行过，则会运行所有剩余的迁移。

在此命名空间下,有一些操作可用,首先你可以运行你的迁移和你的seeds\(我们将讨论它\):

`php artisan migrate --seed`

你也可以运行如下命令:

**migrate:install**

跟踪没运行的迁移并且创建数据库表,当你运行的时候它会自动运行,所以你基本可以忽略它

**migrate:reset**

运行这条命令用于回滚所有的数据库迁移

**migrate:refresh**

回滚迁移,然后执行所有的可用迁移,它与运行migrate:reset 然后运行migrate一样.

**migrate:fresh**

丢弃所有表,然后运行迁移,它与refresh相同,但是不运行"down"只删除表,然后继续运行"up"迁移

**migrate:rollback**

仅回滚上次运行migrate时的迁移，或者使用选项--step=n指定回滚的迁移数。

**migrate:status**

显示一个表，列出每个迁移，每个迁移旁边都有一个Y或N，显示它是否已经在这个环境中运行

> Homestead/Vagrant下的迁移
>
> 如果你在本机运行迁移,并且你的.env文件指向Vagrant box内的数据库,你需要通过ssh进入到虚拟机,然后运行迁移.seeds以及其他artisan命令也是如此.

