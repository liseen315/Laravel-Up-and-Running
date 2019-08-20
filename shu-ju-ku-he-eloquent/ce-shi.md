# 测试

Laravel的整个应用程序测试框架可以轻松测试您的数据库 - 不需要针对Eloquent编写单元测试，通过愿意测试整个应用程序。

以这个场景为例，你想要测试以确保特定页面显示一个联系人而不是另一个联系人。 其中一些逻辑与URL与控制器和数据库之间的相互作用有关，因此测试它的最佳方法是应用程序测试。 您可能正在考虑模拟Eloquent调用并尝试避免系统命中数据库。 不要这样做。 请尝试使用示例5-63

{% code-tabs %}
{% code-tabs-item title="Example 5-63. Testing database interactions with simple application tests" %}
```php
public function test_active_page_shows_active_and_not_inactive_contacts() {
    $activeContact = factory(Contact::class)->create();
    $inactiveContact = factory(Contact::class)->states('inactive')->create();
    $this->get('active-contacts')
        ->assertSee($activeContact->name)
        ->assertDontSee($inactiveContact->name);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

可以看出,模型工厂和Laravel的应用程序测试特性用于数据库测试非常赞

或者，您可以直接在数据库中查找该记录，如示例5-64所示。

{% code-tabs %}
{% code-tabs-item title="Example 5-64. Using assertDatabaseHas\(\) to check for certain records in the database" %}
```php
public function test_contact_creation_works() {
    $this->post('contacts', [
        'email' => 'jim@bo.com'
    ]);
    $this->assertDatabaseHas('contacts', [
        'email' => 'jim@bo.com'
    ]); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Eloquent和Laravel的数据库框架得到了广泛的测试,你不用测试它们,你不需要mock它们,如果你想避免命中数据库，你可以使用一个仓库然后返回未保存的Eloquent模型实例,但是重要的信息是，使用你的数据库逻辑测试你的应用

如果您有定制的访问器、赋值、作用域或其他任何东西，您也可以直接测试它们，如示例5-65所示。

{% code-tabs %}
{% code-tabs-item title="Example 5-65. Testing accessors, mutators, and scopes" %}
```php
public function test_full_name_accessor_works()
{
    $contact = factory(Contact::class)->make([
        'first_name' => 'Alphonse',
        'last_name' => 'Cumberbund'
    ]);

    $this->assertEquals('Alphonse Cumberbund', $contact->fullName);
}

public function test_vip_scope_filters_out_non_vips()
{
    $vip = factory(Contact::class)->states('vip')->create();
    $nonVip = factory(Contact::class)->create();
    $vips = Contact::vips()->get();
    $this->assertTrue($vips->contains('id', $vip->id));
    $this->assertFalse($vips->contains('id', $nonVip->id));
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

说了一大堆,总之尽可能的保持测试简洁



