# 使用输入

现在我们提示了输入，我们如何在命令的handle\(\)方法使用它们？有两种方法检索参数跟选项值。

**argument\(\) 和 arguments\(\)**

$this-&gt;arguments\(\)返回参数数组\(数组第一项是命令名\)，没参数调用$this-&gt;argument\(\)会返回相同的项，我推荐使用复数方法，它提高了可读性，并只在Laravel 5.3之后版本有效。

要获取单个参数值，传递参数值给$this-&gt;argument\(\)，如示例8-6

{% code-tabs %}
{% code-tabs-item title="Example 8-6. Using $this->arguments\(\) in an Artisan command" %}
```bash
// With definition "password:reset {userId}"
php artisan password:reset 5
// $this->arguments() returns this array
[
    "command": "password:reset",
    "userId": "5",
]
// $this->argument('userId') returns this string
"5"
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**option\(\) 和 options\(\)**

$this-&gt;options\(\)返回所有选项的数组，包括一些默认为false或null的选项。$this-&gt;option\(\)在没有参数的情况下调用时返回相同的响应；同样，我更喜欢复数方法，它只是为了更好的可读性而可用，并且只在laravel 5.3之后可用。

要只获取单个选项的值，请将参数名作为参数传递给$this-&gt;option\(\)，如示例8-7所示。

{% code-tabs %}
{% code-tabs-item title="Example 8-7. Using $this->options\(\) in an Artisan command" %}
```bash
// With definition "password:reset {--userId=}"
php artisan password:reset --userId=5
// $this->options() returns this array
[
    "userId" => "5",
    "help" => false,
    "quiet" => false,
    "verbose" => false,
    "version" => false,
    "ansi" => false,
    "no-ansi" => false,
    "no-interaction" => false,
    "env" => null,
]
// $this->option('userId') returns this string
"5"
```
{% endcode-tabs-item %}
{% endcode-tabs %}

示例8-8显示在handler\(\)方法内使用argument\(\)和option\(\)

{% code-tabs %}
{% code-tabs-item title="Example 8-8. Getting input from an Artisan command" %}
```php
public function handle() {
    // All arguments, including the command name
    $arguments = $this->arguments(); // Just the 'userId' argument
    $userid = $this->argument('userId');
    // All options, including some defaults like 'no-interaction' and 'env'
    $options = $this->options();
    // Just the 'sendEmail' option
    $sendEmail = $this->option('sendEmail');
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

