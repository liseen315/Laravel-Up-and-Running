# Tinker

Tinker是一个REPL或者read-eval-print循环，如果你曾经在Ruby中使用过IRB,那么你将熟悉REPL。

REPL为您提供类似于命令行提示的提示，它模仿应用程序的“等待”状态。 您可以在REPL中键入命令，单击Return，然后期望评估您键入的内容并打印出响应。

例8-15提供了一个快速示例，让您了解它的工作原理以及它的用途。 我们用php artisan tinker启动REPL，然后显示空白提示符（&gt;&gt;&gt;）; 对命令的每个响应都打印在前面带有=&gt;的行上。

{% code-tabs %}
{% code-tabs-item title="Example 8-15. Using Tinker" %}
```bash
$ php artisan tinker
>>> $user = new App\User;
=> App\User: {}
>>> $user->email = 'matt@mattstauffer.com';
=> "matt@mattstauffer.com"
>>> $user->password = bcrypt('superSecret');
=> "$2y$10$TWPGBC7e8d1bvJ1q5kv.VDUGfYDnE9gANl4mleuB3htIY2dxcQfQ5" 
>>> $user->save();
=> true
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如您所见，我们创建了一个新用户，设置了一些数据（使用bcrypt（）来保护密码以保证安全性），并将其保存到数据库中。 这是真的。 如果这是一个生产应用程序，我们只会在我们的系统中创建一个全新的用户。

这使得Tinker成为简单数据库交互，尝试新想法以及运行代码片段的绝佳工具，因为找到将它们放入应用程序源文件的地方很痛苦。

Tinker由[Psy Shell](https://psysh.org/)提供动力，所以请检查一下，看看你能用Tinker做些什么。

