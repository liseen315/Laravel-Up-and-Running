# 查询器

现在你已经学会了连接数据库,执行迁移,以及填充数据库表,现在让我们介绍如何使用数据库工具,Laravel数据库功能的核心是数据查询,它是一个fluent接口，可以通过一个清晰的API与几种不同类型的数据库进行交互。

> 什么是fluent接口
>
> Fluent接口主要给终端用户提供便捷的API，对比将数据传递给构造函数或者是调用方法,Fluent采用链式调用.如下是其比较

```php
// Non-fluent:
$users = DB::select(['table' => 'users', 'where' => ['type' => 'donor']]);

// Fluent:
$users = DB::table('users')->where('type', 'donor')->get();
```

Laravel可以通过一个简单的接口连接MySQL, Postgres, SQLite, 和 SQL Server，这只需要更改一些配置

如果您曾经使用过PHP框架，那么您可能已经使用了一个工具，它允许您运行“原始”SQL查询，并提供基本的转义以确保安全性。 查询器就是这样，顶部有很多便利层和帮助器。 所以，让我们从一些简单的调用开始吧



