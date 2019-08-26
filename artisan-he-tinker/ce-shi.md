# 测试

由于您知道如何从代码中调用Artisan命令，因此在测试中很容易实现，并确保您正在执行的任何行为都已正确执行，如例8-16所示。 在我们的测试中，我们使用$this-&gt; artisan\(\)而不是Artisan::call\(\)，因为它具有相同的语法，但添加了一些与测试相关的断言.

{% code-tabs %}
{% code-tabs-item title="Example 8-16. Calling Artisan commands from a test" %}
```php
public function test_empty_log_command_empties_logs_table() {
    DB::table('logs')->insert(['message' => 'Did something']);
    $this->assertCount(1, DB::table('logs')->get());
    $this->artisan('logs:empty'); // Same as Artisan::call('logs:empty');
    $this->assertCount(0, DB::table('logs')->get());
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

在运行Laravel 5.7及更高版本的项目中，您可以将一些新断言链接到$this-&gt;artisan\(\)调用，这使得测试Artisan命令更加容易，不仅可以测试它们对应用程序其余部分的影响，还可以测试它们的实际操作方式。请看示例8-17，以了解此语法的示例。

{% code-tabs %}
{% code-tabs-item title="Example 8-17. Making assertions against the input and output of Artisan commands" %}
```php
public function testItCreatesANewUser() {
    $this->artisan('myapp:create-user')
        ->expectsQuestion("What's the name of the new user?", "Wilbur Powery")
        ->expectsQuestion("What's the email of the new user?", "wilbur@thisbook.co")
        ->expectsQuestion("What's the password of the new user?", "secret")
        ->expectsOutput("User Wilbur Powery created!");
        $this->assertDatabaseHas('users', [
        'email' => 'wilbur@thisbook.co'
    ]); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

