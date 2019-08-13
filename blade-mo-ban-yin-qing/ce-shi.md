# 测试

测试视图最常见的方法是通过应用程序测试,这意味着你实际在调用路由来显示视图,并确保视图显示指定的内容\(参见示例4-34\),你可以单击按钮或者是提交表单来确保重定向到了某个页面,或者是看到某个错误.\(你将会在12章了解更多关于测试的内容\)

{% code-tabs %}
{% code-tabs-item title="Example 4-34. Testing that a view displays certain content" %}
```php
// EventsTest.php
public function test_list_page_shows_all_events() {
    $event1 = factory(Event::class)->create();
    $event2 = factory(Event::class)->create();
    $this->get('events')
        ->assertSee($event1->title)
        ->assertSee($event2->title);
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

你也可以用一堆数据测试视图，如果它实现了您的测试目标，那么总比你在页面上看一堆错误文本来的容易。示例4-35演示了这种方法

{% code-tabs %}
{% code-tabs-item title="Example 4-35. Testing that a view was passed certain content" %}
```php
// EventsTest.php
public function test_list_page_shows_all_events() {
    $event1 = factory(Event::class)->create();
    $event2 = factory(Event::class)->create();
    $response = $this->get('events');
    $response->assertViewHas('events', Event::all());
    $response->assertViewHasAll([
        'events' => Event::all(),
        'title' => 'Events Page',
    ]);
    $response->assertViewMissing('dogs');
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

> Laravel 5.4之前版本测试方法的名字不同之处
>
> Laravel 5.4之前版本应该把get与assertSee\(\)替换为visit\(\)和see\(\)

在5.3中，我们获得了将闭包传递给assertViewHas\(\)的能力，这意味着我们可以自定义检查更复杂的数据结构的能力。 例4-36说明了我们如何使用它

{% code-tabs %}
{% code-tabs-item title="Example 4-36. Passing a closure to assertViewHas\(\)" %}
```php
// EventsTest.php
public function test_list_page_shows_all_events() {
    $event1 = factory(Event::class)->create();
    $response = $this->get("events/{ $event->id }");
    $response->assertViewHas('event', function ($event) use ($event1) { return $event->id === $event1->id;
}); }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

