# 一个简单的命令

我们在本章中还没有介绍mail或Eloquent（mail见第15章，Eloquent见第5章），但是示例8-2中的示例handle\(\)方法应该非常容易理解

{% code-tabs %}
{% code-tabs-item title="Example 8-2. A sample Artisan command handle\(\) method" %}
```php
class WelcomeNewUsers extends Command {
    public function handle() {
        User::signedUpThisWeek()->each(function ($user) { 
            Mail::to($user)->send(new WelcomeEmail);
        }); 
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

现在，每当您运行php artisan email:newusers时，这个命令将抓取本周注册的每个用户，并向他们发送欢迎电子邮件。

如果您希望注入邮件和用户依赖项而不是使用facade，可以在命令构造函数中键入它们，Laravel的容器将在实例化命令时为您注入它们。

让我们看下示例8-3采用依赖注入提取行为到服务类。

{% code-tabs %}
{% code-tabs-item title="Example 8-3. The same command, refactored" %}
```php
...
class WelcomeNewUsers extends Command {
        public function __construct(UserMailer $userMailer) {
                parent::__construct();
                $this->userMailer = $userMailer
        }
        public function handle() {
                $this->userMailer->welcomeNewUsers();
        }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> 保持简单
>
> 可以从余下代码调用命令，因此你可以使用它们封装应用逻辑。
>
> 然而，Laravel文档建议将应用程序逻辑打包到服务类中，并将该服务注入到您的命令中。控制台命令被视为类似于控制器：它们不是域类；它们只传递请求到正确的行为。

