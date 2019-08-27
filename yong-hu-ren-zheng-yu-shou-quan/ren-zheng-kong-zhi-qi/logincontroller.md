# LoginController

毫无疑问，LoginController允许用户登录。它引入了AuthenticatesUsers特性，它引入了RedirectsUsers和ThrottlesLogins特征。

与RegistrationController一样，LoginController有一个$RedirectTo属性，允许您自定义用户成功登录后将重定向的路径。其他所有东西都存在于AuthenticatesUsers特性后面。

**AuthenticatesUsers 特性**

AuthenticatesUsers特性负责向用户显示登录表单、验证其登录、阻止失败的登录、处理注销以及在成功登录后重定向用户。

showLoginForm\(\)方法默认显示auth.login视图，如果你想用其他视图你也可以覆盖这个方法。

login\(\)方法用于接收表单提交,validateLogin\(\)用于验证请求，你也可以覆盖他创建自己的验证，然后引入了ThrottlesLogins钩子，用于登录节流,稍后在介绍它，最后重定向用户到预期路径\(如果用户在尝试访问应用程序中的页面时被重定向到登录页面\)，或者用redirectPath定义重定向路径，这方法将返回$redirectTo属性。

成功登录后，特性会调用空的authenticated\(\)方法，因此如果您想要响应成功登录而执行任何类型的行为，只需在LoginController中覆盖此方法即可。 有一个username\(\)方法，用于定义哪些用户列是“用户名”; 默认是电子邮件，但您可以通过覆盖控制器中的username\(\)方法来更改它。

并且，与RegistersUsers特性一样，您可以覆盖guard\(\)方法来定义该控制器应该使用哪个auth守卫（在第236页的“守卫”中了解更多）。

**ThrottlesLogins 特性**

ThrottlesLogins特性是Laravel的Illuminate\Cache \RateLimiter类的接口，该类是一个使用缓存对任何事件进行速率限制的实用程序。 此特征对用户登录应用速率限制，如果用户在一定时间内登录失败，则限制用户使用登录表单。 Laravel 5.1中不存在此功能。

如果导入ThrottlesLogins特征，则其所有方法都受到保护，这意味着它们实际上不能作为路径进行访问。 相反，AuthenticatesUsers特性会查看您是否已导入ThrottlesLogins特征，如果是，它会将其功能附加到您的登录，而无需您做任何工作。 由于默认的LoginController导入了两者，如果您使用auth脚手架，您将免费获得此功能（在第239页的“The Auth Scaffold”中讨论过）

ThrottlesLogins将用户名和IP地址的任何给定组合限制为每60秒5次尝试。 使用缓存，它会增加给定用户名/ IP地址组合的“失败登录”计数，如果任何用户在60秒内达到5次失败的登录尝试，它会将该用户重定向回登录页面并发出相应的错误，直到60 秒结束了



