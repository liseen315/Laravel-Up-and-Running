# 测试

幸运的是，测试API实际上比在laravel中测试任何东西都简单。

我们将在第12章中更深入地介绍这一点，但是有一系列方法可以对JSON进行断言。 将该功能与全栈应用程序测试的简单性相结合，您可以快速轻松地组合API测试。 看一下例13-44中的常见API测试模式。

{% code-tabs %}
{% code-tabs-item title="Example 13-44. A common API testing pattern" %}
```php
class DogsApiTest extends TestCase
{
    use WithoutMiddleware, RefreshDatabase;
    public function test_it_gets_all_dogs() {
        $dog1 = factory(Dog::class)->create();
        $dog2 = factory(Dog::class)->create();
        $response = $this->getJson('api/dogs');
        $response->assertJsonFragment(['name' => $dog1->name]);
        $response->assertJsonFragment(['name' => $dog2->name]);
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

请注意，我们使用WithoutMiddleware以避免担心身份验证。 如果需要的话，您将需要单独测试（有关身份验证的更多信息，请参阅第9章）

在这个测试中，我们将两个dog插入数据库，然后访问api路径以列出所有dog，并确保输出中都存在这两个dog。

在这里，您可以简单方便地覆盖所有api路由，包括修改post和patch等操作。

**测试Passport**

你也可以在Passport facacde上使用actingAs\(\)来测试你的作用域，请看示例13-45，以查看Passport中测试作用域的通用模式。

{% code-tabs %}
{% code-tabs-item title="Example 13-45. Testing scoped access" %}
```php
public function test_it_lists_all_clips_for_those_with_list_clips_scope() {
    Passport::actingAs(
        factory(User::class)->create(),
        ['list-clips']
    );
    $response = $this->getJson('api/clips');
    $response->assertStatus(200);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

