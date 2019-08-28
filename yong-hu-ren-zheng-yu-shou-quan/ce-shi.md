# 测试

应用程序测试通常模拟特定用户执行特定的行为。因此，我们需要能够在应用程序测试中以用户进行身份验证，并且我们需要测试授权规则和身份验证路由。

当然，可以编写一个应用程序测试，手动访问登录页面，然后填写表单并提交，但这不是必需的。相反，最简单的选项是使用-&gt;be\(\)方法模拟以用户身份登录。请看示例9-30

{% code-tabs %}
{% code-tabs-item title="Example 9-30. Authenticating as a user in application tests" %}
```php
public function test_it_creates_a_new_contact() {
    $user = factory(User::class)->create();
    $this->be($user);
    $this->post('contacts', [
        'email' => 'my@email.com',
    ]);
    $this->assertDatabaseHas('contacts', [
        'email' => 'my@email.com',
        'user_id' => $user->id,
    ]); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

你可以采用链式写法和actingAs\(\)来代替be\(\)。

```php
public function test_it_creates_a_new_contact() {
    $user = factory(User::class)->create();
    $this->actingAs($user)->post('contacts', [
        'email' => 'my@email.com',
    ]);
    $this->assertDatabaseHas('contacts', [
        'email' => 'my@email.com',
        'user_id' => $user->id,
    ]); 
}
```

我们还可以像示例9-31中那样测试授权

{% code-tabs %}
{% code-tabs-item title="Example 9-31. Testing authorization rules" %}
```php
public function test_non_admins_cant_create_users() {
    $user = factory(User::class)->create([ 
        'admin' => false,
    ]);
    $this->be($user);
    $this->post('users', ['email' => 'my@email.com']);
    $this->assertDatabaseMissing('users', [
        'email' => 'my@email.com',
    ]); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

或者可以像示例9-32一样来测试403响应

{% code-tabs %}
{% code-tabs-item title="Example 9-32. Testing authorization rules by checking status code" %}
```php
public function test_non_admins_cant_create_users() {
    $user = factory(User::class)->create([ 
        'admin' => false,
    ]);
    $this->be($user);
    $response = $this->post('users', ['email' => 'my@email.com']);
    $response->assertStatus(403);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

我们需要测试身份验证\(登录和登出\)，可以如示例9-33一样

{% code-tabs %}
{% code-tabs-item title="Example 9-33. Testing authentication routes" %}
```php
public function test_users_can_register() {
    $this->post('register', [
        'name' => 'Sal Leibowitz',
        'email' => 'sal@leibs.net',
        'password' => 'abcdefg123',
        'password_confirmation' => 'abcdefg123',
    ]);
    $this->assertDatabaseHas('users', [
        'name' => 'Sal Leibowitz',
        'email' => 'sal@leibs.net',
    ]); 
}

public function test_users_can_log_in() {
    $user = factory(User::class)->create([
        'password' => Hash::make('abcdefg123')
    ]);
    $this->post('login', [
        'email' => $user->email,
        'password' => 'abcdefg123',
    ]);
    $this->assertTrue(auth()->check());
    $this->assertTrue($user->is(auth()->user()));
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

我们还可以使用集成测试特性来指导测试“单击”我们的验证字段并“提交”字段来测试整个流程。我们将在第12章中进一步讨论这个问题。

> 与5.4之前版本测试方法名的不同
>
> 在运行5.4之前版本的laravel的项目中，assertDatabaseHas\(\)应该被替换成seeInDatabase\(\)，assertDatabaseMissing\(\)应该被替换成dontSeeInDatabase\(\)，assertDatabaseHas\(\)应被替换成seeInDatabase\(\)，应该在$ this而不是$ response上调用assertStatus\(\)

