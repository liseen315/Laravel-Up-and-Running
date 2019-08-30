# 依赖注入与Laravel

正如您在示例11-1中看到的，依赖注入最常见的模式是构造函数注入，或者在对象被实例化（“构造”）时注入它的依赖项。

让我们以示例11-1中的UserMailer类为例。示例11-2显示了创建和使用它的实例的情况。

{% code-tabs %}
{% code-tabs-item title="Example 11-2. Simple manual dependency injection" %}
```php
$mailer = new MailgunMailer($mailgunKey, $mailgunSecret, $mailgunOptions);
$userMailer = new UserMailer($mailer);
$userMailer->welcome($user);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

现在让我们假设我们希望UserMailer类能够记录消息，并在每次发送消息时向Slack通道发送通知。 如例11-3所示。 正如你所看到的，如果我们每次想要创建一个新实例时都必须完成所有这些工作，那将会变得相当笨拙 - 特别是当你考虑到我们必须从某个地方获取所有这些参数时。

{% code-tabs %}
{% code-tabs-item title="Example 11-3. More complex manual dependency injection" %}
```php
$mailer = new MailgunMailer($mailgunKey, $mailgunSecret, $mailgunOptions); 
$logger = new Logger($logPath, $minimumLogLevel);
$slack = new Slack($slackKey, $slackSecret, $channelName, $channelIcon); 
$userMailer = new UserMailer($mailer, $logger, $slack);
$userMailer->welcome($user);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

想象一下，每次你想要得到一个UserMailer实例都的写这些代码，依赖注入很棒，但是这么写一团糟

