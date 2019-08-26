# 基于闭包的命令

如果你希望简化命令定义过程,你可以在routes/console.php内使用闭包而非类来定义。我们在本章内讨论的都可以用同样方式来定义。示例8-12你可以通过一个步骤定义和注册命令。

{% code-tabs %}
{% code-tabs-item title="Example 8-12. Defining an Artisan command using a closure" %}
```php
// routes/console.php
Artisan::command(
    'password:reset {userId} {--sendEmail}', function ($userId, $sendEmail) {
    $userId = $this->argument('userId'); // Do something...
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

