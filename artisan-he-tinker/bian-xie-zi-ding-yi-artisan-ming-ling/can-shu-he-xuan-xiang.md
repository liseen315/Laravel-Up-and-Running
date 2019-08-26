# 参数和选项

$signature似乎只包含命令名，这个属性也定义参数跟选项，有一个简单的语法可以用于添加参数跟选项到你的Artisan命令。

在我们输入语法前，先看简单的例子。

`protected $signature = 'password:reset {userId} {--sendEmail}';`

**参数-必选，可选，默认**

要定义必选参数，使用大括号括起来。

```text
password:reset {userId}
```

使参数可选，添加问号

```text
 password:reset {userId?}
```

使可选并提供默认参数

```text
password:reset {userId=1}
```

**选项-必须值，默认值，以及快捷方式**

选项类似于参数，使用--并且必须带值，添加基础选项，使用大括号包裹。

```text
password:reset {userId} {--sendEmail}
```

如果选项需要参数，给签名添加=号

```text
password:reset {userId} {--password=}
```

如果你想传递默认值，将其添加到=号之后

```text
 password:reset {userId} {--queue=default}
```

**数组参数和数组选项**

对于参数和选项，如果要接收数组，请使用\*字符

```text
 password:reset {userIds*}
 password:reset {--ids=*}
```

使用数组参数,如示例8-4

{% code-tabs %}
{% code-tabs-item title="Example 8-4. Using array syntax with Artisan commands" %}
```text
// Argument
php artisan password:reset 1 2 3 // Option
php artisan password:reset --ids=1 --ids=2 --ids=3
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> 数组参数必须是最后一个参数
>
> 由于数组参数在定义后捕获每个参数并将它们添加为数组项，因此数组参数必须是Artisan命令签名中的最后一个参数

**输入说明**

还记得如果我们使用Artisan帮助，内置的Artisan命令如何为我们提供有关其参数的更多信息吗？ 我们可以给自定义命令提供相同信息。 只需在花括号中添加冒号和描述文本即可，如示例8-5

{% code-tabs %}
{% code-tabs-item title="Example 8-5. Defining description text for Artisan arguments and options" %}
```php
protected $signature = 'password:reset
                        {userId : The ID of the user}
                        {--sendEmail : Whether to send user an email}';
```
{% endcode-tabs-item %}
{% endcode-tabs %}



