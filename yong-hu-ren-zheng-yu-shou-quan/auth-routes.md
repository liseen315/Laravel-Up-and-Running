# Auth::routes\(\)

现在我们让auth控制器为一系列预定义的路由提供了一些方法，我们希望我们的用户能够实际能够访问这些路由。 我们可以手动将所有路由添加到routes/web.php，但是已经有一个方便的工具，叫做Auth::routes\(\)

```php
// routes/web.php
Auth::routes();
```

正如您可能猜到的那样，Auth::routes\(\)会将一组预定义路由引入到您的路由文件中。 在例9-5中，您可以看到这些定义的路由。

{% code-tabs %}
{% code-tabs-item title="Example 9-5. The routes provided by Auth::routes\(\)" %}
```php
// Authentication routes
$this->get('login', 'Auth\LoginController@showLoginForm')->name('login');
$this->post('login', 'Auth\LoginController@login');
$this->post('logout', 'Auth\LoginController@logout')->name('logout');
// Registration routes
$this->get('register', 'Auth\RegisterController@showRegistrationForm')
    ->name('register');
$this->post('register', 'Auth\RegisterController@register');
// Password reset routes
$this->get('password/reset', 'Auth\ForgotPasswordController@showLinkRequestForm')
    ->name('password.request');
$this->post('password/email', 'Auth\ForgotPasswordController@sendResetLinkEmail')
    ->name('password.email');
$this->get('password/reset/{token}', 'Auth\ResetPasswordController@showResetForm')
    ->name('password.reset');
$this->post('password/reset', 'Auth\ResetPasswordController@reset');
// If email verification is enabled
$this->get('email/verify', 'Auth\VerificationController@show')
    ->name('verification.notice');
$this->get('email/verify/{id}', 'Auth\VerificationController@verify')
    ->name('verification.verify');
$this->get('email/resend', 'Auth\VerificationController@resend')
    ->name('verification.resend');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

基本上，Auth::routes\(\)包括用于身份验证，注册和密码重置的路由。 如您所见，还有可选的电子邮件验证路由，这是Laravel 5.7中引入的一项功能。

要启用Laravel的电子邮件验证服务，该服务要求新用户验证他们是否可以访问他们注册的电子邮件地址，请更新您的Auth::routes\(\)调用以启用它，如此处所示。

```php
Auth::routes(['verify' => true]);
```

我们将在第235页的“电子邮件验证”中进一步讨论这个问题。

> 在Laravel 5.7+,您可以给Auth::routes\(\)方法传递“register” 和 “reset”键数组来信用注册和重置密码

> Auth::routes\(\['register' =&gt; false, 'reset' =&gt; false\]\);

