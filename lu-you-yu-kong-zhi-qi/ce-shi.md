# 测试

在其他一些社区,控制器方法单元测试是很常见的,但是在Laravel\(以及大多数PHP社区\),通常依赖application testing来测试路由功能

例如,为了验证POST路由是否正常,我们可以这样写测试:

{% code-tabs %}
{% code-tabs-item title="Example 3-47. Writing a simple POST route test" %}
```php
// tests/Feature/AssignmentTest.php
public function test_post_creates_new_assignment()
{
    $this->post('/assignments', [
        'title' => 'My great assignment',
    ]);
    $this->assertDatabaseHas('assignments', [
        'title' => 'My great assignment',
    ]);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

我们是否直接调用了控制器方法?没有,但还是我们验证了路由目标-接收一个POST请求,然后把数据保存到了数据库.

你还可以使用类似的语法访问路由，并验证某些文本是否显示在页面上，或者单击某些按钮是否执行某些操作（参见示例3-48）。

{% code-tabs %}
{% code-tabs-item title="Example 3-48. Writing a simple GET route test" %}
```php
// AssignmentTest.php
public function test_list_page_shows_all_assignments()
{
    $assignment = Assignment::create([
        'title' => 'My great assignment',
    ]);
    $this->get('/assignments')
        ->assertSee('My great assignment');
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> Laravel 5.4版本之前测试方法名字的不同
>
> 在Laravel 5.4版本之前应该把assertDatabaseHas\(\)替换成seeInDatabase\(\)，get\(\)和assertSee\(\)应该替换成visit\(\) 和 see\(\)

