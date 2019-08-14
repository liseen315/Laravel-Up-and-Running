# 其他数据库配置选项

config/database.php包含了其他的配置项,你可以配置redis,自定义迁移表名,默认链接驱动,以及切换non-Eloquent是返回stdClass还是数组实例

Laravel中的任何服务都允许来自多个来源的连接，session可以靠数据库或者是文件进行存储,缓存可以使用Redis或者是Memcached，数据库可以使用MySQL或者是PostgreSQL,你可以定义多个链接,也可以选择一个特定的链接作为默认,如下是你指定特定链接的方式.

```text
$users = DB::connection('secondary')->select('select * from users');
```

\[role="less\_space pagebreak-before"\]' === Migrations

像Laravel这种现代化框架可以很容易的使用代码驱动迁移来定义数据库结构,可以在代码中定定义表,列,索引跟键,你可以很方便的将数据库迁移到新环境.



