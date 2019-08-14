# 数据库连接

默认,Laravel会为每个数据库驱动对应一个链接配置,你可以查看示例5-1

{% code-tabs %}
{% code-tabs-item title="Example 5-1. The default database connections list" %}
```php
'connections' => [

        'sqlite' => [
            'driver' => 'sqlite',
            'url' => env('DATABASE_URL'),
            'database' => env('DB_DATABASE', database_path('database.sqlite')),
            'prefix' => '',
            'foreign_key_constraints' => env('DB_FOREIGN_KEYS', true),
        ],

        'mysql' => [
            'driver' => 'mysql',
            'url' => env('DATABASE_URL'),
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'forge'),
            'username' => env('DB_USERNAME', 'forge'),
            'password' => env('DB_PASSWORD', ''),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'prefix_indexes' => true,
            'strict' => true,
            'engine' => null,
            'options' => extension_loaded('pdo_mysql') ? array_filter([
                PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
            ]) : [],
        ],

        'pgsql' => [
            'driver' => 'pgsql',
            'url' => env('DATABASE_URL'),
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '5432'),
            'database' => env('DB_DATABASE', 'forge'),
            'username' => env('DB_USERNAME', 'forge'),
            'password' => env('DB_PASSWORD', ''),
            'charset' => 'utf8',
            'prefix' => '',
            'prefix_indexes' => true,
            'schema' => 'public',
            'sslmode' => 'prefer',
        ],

        'sqlsrv' => [
            'driver' => 'sqlsrv',
            'url' => env('DATABASE_URL'),
            'host' => env('DB_HOST', 'localhost'),
            'port' => env('DB_PORT', '1433'),
            'database' => env('DB_DATABASE', 'forge'),
            'username' => env('DB_USERNAME', 'forge'),
            'password' => env('DB_PASSWORD', ''),
            'charset' => 'utf8',
            'prefix' => '',
            'prefix_indexes' => true,
        ],

    ]
```
{% endcode-tabs-item %}
{% endcode-tabs %}

你可以删除或者修改这些连接名,或者创建属于你自己的连接,你可以创建新的连接,然后在其中设置驱动\(MySQL, Postgres，等\)，因此，虽然默认情况下每个驱动程序有一个连接，但这不是必要的，如果你愿意，你可以有五个不同的连接，都使用mysql驱动程序

每个连接允许你为连接和自定义连接类型定义必要的属性

使用多个数据库驱动有这么几个原因,首先"connections"是一个简单的模板,可以轻松启动支持的数据库连接类型的应用程序,你选择你将要的连接，然后填写配置信息,这时你可以删除不想用的链接配置,但是通常我不删,说不准后来又用到了.

但是在某些情况下，您可能需要在同一个应用程序中建立多个连接。 例如，您可以为两种不同类型的数据使用不同的数据库连接，或者您可以从一个数据读取并写入另一个数据。 因此支持多个连接使这成为可能



