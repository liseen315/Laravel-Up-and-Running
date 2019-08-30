# 绑定具体实例到接口

就像我们可以绑定一个类到另外一个类，或者一个类到快捷方式，我们也可以绑定到一个接口，这非常强大，因为我们可以用类型限定接口代替类名，如示例11-10

{% code-tabs %}
{% code-tabs-item title="Example 11-10. Typehinting and binding to an interface" %}
```php
use Interfaces\Mailer as MailerInterface;
class UserMailer {
    protected $mailer;
    public function __construct(MailerInterface $mailer) {
        $this->mailer = $mailer;
    }
}
// Service provider
public function register() {
    $this->app->bind(\Interfaces\Mailer::class, function () { 
        return new MailgunMailer(...);
    }); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

您现在可以在整个代码中类型限定Mailer或Logger接口，然后在服务提供商中选择一个您想要在任何地方使用的特定邮件程序或记录程序。 这是控制的倒置。

使用此模式可以获得的一个主要好处是，如果您选择使用与Mailgun不同的邮件提供程序，只要您拥有实现Mailer接口的新提供程序的邮件程序类，就可以将其在您的服务提供商中交换，其余代码中的所有内容都可以正常运行。



