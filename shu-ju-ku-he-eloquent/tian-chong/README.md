# 填充

使用Laravel填充非常简单，它作为正常开发工作流程的一部分得到了广泛采用，其方式在以前的PHP框架中没有.database/seeds文件夹下附带有一个叫做DatabaseSeeder的类,它有一个run方法,当你调用seeder会调用此方法.

有两种方式运行seeders:一种是与迁移一起运行,一种是独立运行.

与迁移运行只需要添加--seed选项

```bash
php artisan migrate --seed
php artisan migrate:refresh --seed
```

独立运行

```bash
php artisan db:seed
php artisan db:seed --class=VotesTableSeeder
```

这将会默认调用DatabaseSeeder的run方法,也可以用--class指定特殊的seeder类

