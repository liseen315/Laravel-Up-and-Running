# 认证脚手架

此时，你已拥有迁移，模型，控制器，以及一个用于认证的路由，但是视图呢？

Laravel通过提供auth脚手架（自5.2开始提供），这些脚手架旨在在新应用程序上运行，并为您提供更多的框架代码，以使您的auth系统快速运行。

脚手架添加Auth::routes\(\)到路由文件，从而为每个路由创建视图，并且创建一个HomeController用于登录页。然后会路由到HomeController的index\(\)方法URI是/home。

运行php artisan make:auth，你将的到如下文件。

```text
app/Http/Controllers/HomeController.php
resources/views/auth/login.blade.php
resources/views/auth/register.blade.php
resources/views/auth/verify.blade.php
resources/views/auth/passwords/email.blade.php
resources/views/auth/passwords/reset.blade.php
resources/views/layouts/app.blade.php
resources/views/home.blade.php
```

此时，/返回welcome视图，/ home返回home视图，以及一系列用于登录，注销，注册和密码重置的auth路由，指向auth控制器。 每个种子视图都有基于Bootstrap的布局和表单字段，用于登录，注册和密码重置所需的所有字段，并且它们已指向正确的路由。

现在你已经为普通用户的注册跟认证准备好了所有步骤，你可以随意调整，但是你已经完全准备好注册和认证用户了。

让我们快速回顾从新站点到完整身份验证系统的步骤：

```text
laravel new MyApp
cd MyApp
# Edit your .env file to specify the correct database connection details
php artisan make:auth
php artisan migrate
```

是的，运行这些命令你就可以获取一个落地页以及一个基于Bootstrap的用户注册系统，包含登录，登出，密码重置。

