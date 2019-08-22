# 测试

如果你对与用户交互测试感兴趣，那么你可能最感兴趣的是模拟有效和无效的用户输入，如果输入无效，那么用户将被重定向，如果有效，则正常运行。

Laravel的端对端使得这一切很简单。

> Laravel 5.4版本后需要BrowserKit
>
> 如果你想在页面的表单与用户交互测试，并且运行在Laravel 5.4或更高版本，你需要拉取BrowserKit测试包。
>
> `composer require laravel/browser-kit-testing --dev`
>
> 然后修改你应用基础TestCase类，继承Laravel\BrowserKitTesting\TestCase，而不是Illuminate \Foundation\Testing\TestCase.

让我们从一个我们期望被拒绝的无效路由开始，如例7-21所示。

{% code-tabs %}
{% code-tabs-item title="Example 7-21. Testing that invalid input is rejected" %}
```php
public function test_input_missing_a_title_is_rejected() {
    $response = $this->post('posts', ['body' => 'This is the body of my post']);
    $response->assertRedirect();
    $response->assertSessionHasErrors();
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

在这里，我们断言无效输入之后，用户将被重定向，并附加错误。你可以看到，我们使用了一些自定义的PHP单元测试断言。

> Laravel 5.4之前版本测试方法不同的名字
>
> Laravel 5.4之前版本assertRedirect\(\)断言方法命名为assertRedirectedTo\(\)

所以，我们如何测试成功的情况？如示例7-22

{% code-tabs %}
{% code-tabs-item title="Example 7-22. Testing that valid input is processed" %}
```php
public function test_valid_input_should_create_a_post_in_the_database() {
    $this->post('posts', ['title' => 'Post Title', 'body' => 'This is the body']);
    $this->assertDatabaseHas('posts', ['title' => 'Post Title']);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

请注意，如果您正在使用数据库测试某些内容，则需要了解有关数据库迁移和事务的更多信息。在第12章中有更多的内容



