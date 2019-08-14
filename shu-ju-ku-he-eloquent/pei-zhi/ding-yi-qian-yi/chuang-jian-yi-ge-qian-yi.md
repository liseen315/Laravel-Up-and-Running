# 创建一个迁移

正如第8章所看到的,Laravel提供了一系列命令行工具,你可以使用这些工具与你的应用交互并生成模板文件,其中一条命令允许你创建迁移文件.你可以运行php artisan make:migration，然后它只有一个参数,就是迁移的名字,例如,要创建刚才介绍的表,你应该运行php artisan make:migration create\_users\_table

{% hint style="info" %}
译者注

使用驼峰命名也可以创建同样名字的迁移文件:

php artisan make:migration CreateUsersTable
{% endhint %}

这条命令还有两个可选选项--create = table\_name用于创建名为table\_name的表,--table=table\_name用于修改现有的表,如下面的例子

```bash
php artisan make:migration create_users_table
php artisan make:migration add_votes_to_users_table --table=users
php artisan make:migration create_users_table --create=users
```

