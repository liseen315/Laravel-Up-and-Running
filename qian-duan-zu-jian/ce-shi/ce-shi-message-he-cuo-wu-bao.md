# 测试Message和错误包

有两种方法测试Message和错误包，首先，您可以在应用程序测试中执行一种行为，该行为设置最终将在某处显示消息，然后重定向到页面断言显示的消息。

其次，对于错误（这是最常见的用例），您可以使用$this-&gt;AssertSessionHasErrors\($bindings=\[\]\)断言会话的错误。看看示例6-26。

{% code-tabs %}
{% code-tabs-item title="Example 6-26. Asserting the session has errors" %}
```php
public function test_missing_email_field_errors() {
    $this->post('person/create', ['name' => 'Japheth']);
    $this->assertSessionHasErrors(['email']);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

为了通过示例6-26，您需要向该路由添加输入验证。我们将在第七章讨论这个问题。

