# 信息包

Web应用程序中另一个常见但又痛苦的功能是在应用程序的各个组件之间传递消息，最终目标是与用户共享它们。 例如，您的控制器可能希望发送验证消息：“电子邮件字段必须是有效的电子邮件地址。”但是，该特定消息不仅需要将其发送到视图层; 它实际上还需要在重定向中存活，然后最终用于不同页面的视图层中。 你如何构建这种消息传递逻辑

Illuminate\Support\MessageBag是一个类，用于存储、分类和返回面向最终用户的消息。它按键对所有消息进行分组，其中键可能是错误和消息，它提供了获取所有存储的消息或仅获取特定键的消息并以各种格式输出这些消息的方便方法。

您可以像例6-18中那样手动启动MessageBag的新实例。 说实话，你可能不会手动这样做 - 这只是一个思考练习来展示它是如何工作的

{% code-tabs %}
{% code-tabs-item title="Example 6-18. Manually creating and using a message bag" %}
```php
$messages = [
    'errors' => [
        'Something went wrong with edit 1!',
    ],
    'messages' => [
        'Edit 2 was successful.',
    ], 
];
$messagebag = new \Illuminate\Support\MessageBag($messages);

// Check for errors; if there are any, decorate and echo
if ($messagebag->has('errors')) {
    echo '<ul id="errors">';
    foreach ($messagebag->get('errors', '<li><b>:message</b></li>') as $error) {
        echo $error; 
    }
    echo '</ul>'; 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

消息包也与Laravel的验证器密切相关（您将在第189页的“验证”中了解更多信息）：当验证器返回错误时，它们实际上会返回MessageBag实例，然后您可以将其传递给视图或附加到重定向\('route'\)-&gt;withErrors\($messagebag\)。

Laravel将MessageBag的空实例传递给每个视图，这些视图被分配给变量$errors；如果您在重定向时使用witherRors\(\)刷新了一个消息包，那么它将被分配给$errors变量，这意味着每个视图总是可以假设它有一个$errors MessageBag来处理验证，这导致了示例6-19成为开发人员在每个页面上放置的一个常见代码片段。

{% code-tabs %}
{% code-tabs-item title="Example 6-19. Error bag snippet" %}
```markup
// partials/errors.blade.php
@if ($errors->any())
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors as $error)
                <li>{{ $error }}</li> 
            @endforeach
        </ul>
    </div>
@endif
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
丢失$erros变量

如果你的路由不位于web中间件路由组下，它们也不会拥有会话中间件，也不会有$erros变量
{% endhint %}

