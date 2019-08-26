# 编写自定义Artisan命令

我们介绍了开箱即用的Artisan命令，让我们看看如何自定义它们。

首先你应该知道，有一条Artisan命令,运行php artisan make:command YourCommandName生成一个新的Artisan命令在app/Console/ Commands/{YourCommandName}.php

> php artisan make:command
>
> 这条命令进行了几次变更，最初是command:make,5.2的时候改成了console:make随后变成了make:console。
>
> 最后在5.3版本解决了这个问题，所有都在make:命名空间下,现在都叫make:command

第一个参数是命令的类名，你可以传递选项--comand参数定义终端命令是什么\(例如，appname:action\)，那么让我们来看看怎么做:

`php artisan make:command WelcomeNewUsers --command=email:newusers`

查看下示例8-1

{% code-tabs %}
{% code-tabs-item title="Example 8-1. The default skeleton of an Artisan command" %}
```php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;

class WelcomeNewUsers extends Command
{
    /**
     * The name and signature of the console command.
     *
     * @var string
     */
    protected $signature = 'email:newusers';

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = 'Command description';

    /**
     * Create a new command instance.
     *
     * @return void
     */
    public function __construct()
    {
        parent::__construct();
    }

    /**
     * Execute the console command.
     *
     * @return mixed
     */
    public function handle()
    {
        //
    }
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

如您所见，很容易定义命令签名、在命令列表中显示的帮助文本以及命令在实例化（\_\_construct\(\)\) 和 执行 \(handle\(\)）时执行的行为。

> 手动绑定命令到Laravel 5.5之前的版本
>
> 在运行5.5之前版本的Laravel的项目中，命令必须手动绑定到app\console\kernel.php中。如果您的应用程序运行的是旧版本的laravel，只需将命令的完全限定类名添加到该文件中的$commands数组中，它就会被注册。

```php
protected $commands = [ 
    \App\Console\Commands\WelcomeNewUsers::class,
];
```

