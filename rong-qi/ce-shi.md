# 测试

在Laravel中使用控制反转和依赖注入测试非常常见，例如你可以绑定不同的logger，这取决于应用程序是处于活动状态还是测试中。或者你可以将事务性电子邮件服务从Mailgun更改为本地，以便于检查。这两种交换都非常常见，使用Laravel的.env配置更容易实现，但是你可以使用任何接口或者是类。

最简单的方法是在需要它们时直接在测试中重新绑定类和接口，如示例11-17。

{% code-tabs %}
{% code-tabs-item title="Example 11-17. Overriding a binding in tests" %}
```php
public function test_it_does_something() {
    app()->bind(Interfaces\Logger, function () { 
        return new DevNullLogger;
    });
    // Do stuff
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果你需要为测试重新绑定类或者接口\(这不常见\)，您可以在测试类的setUp\(\)方法或Laravel的TestCase基础测试的setUp\(\)方法中执行此操作，如示例11-18

{% code-tabs %}
{% code-tabs-item title="Example 11-18. Overriding a binding for all tests" %}
```php
class TestCase extends \Illuminate\Foundation\Testing\TestCase {
    public function setUp() {
        parent::setUp();
        app()->bind('whatever', 'whatever else');
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

当使用类似Mockery的东西时，通常会创建一个类的mock或spy或stub，然后将其重新绑定到容器中以代替其引用。

