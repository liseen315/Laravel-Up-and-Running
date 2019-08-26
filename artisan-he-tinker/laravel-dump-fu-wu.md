# Laravel Dump 服务

在开发过程中调试数据状态的一种常用方法是使用Laravel的dump\(\)帮助程序，它在传递给它的任何内容上运行一个修饰var\_dump\(\)。 这很好，但它经常会遇到视图问题。

在运行laravel 5.7及更高版本的项目中，现在可以启用laravel dump服务器，该服务器捕获这些dump\(\)语句并在控制台中显示它们，而不是将它们呈现到页面上。

要在本地控制台运行dump server，请导航到项目跟目录并运行php artisan dump server。

```text
$ php artisan dump-server Laravel Var Dump Server
=======================
[OK] Server listening on tcp://127.0.0.1:9912
// Quit the server with CONTROL-C.
```

现在尝试在你的代码内使用dump\(\)辅助函数，测试输出，在routes/web.php内尝试编码。

```text
Route::get('/', function () {
    dump('Dumped Value');
    return 'Hello World';
});
```

如果没有dump服务器，您将看到dump和您的“hello world”。但是dump服务器运行时，您将只在浏览器中看到“hello world”。在控制台中，您将看到dump服务器捕获了dump（），您可以在那里检查它。

```text
GET http://myapp.test/
--------------------
------------ ---------------------------------
 date         Tue, 18 Sep 2018 22:43:10 +0000
 controller   "Closure"
 source       web.php on line 20
 file         routes/web.php
------------ ---------------------------------
"Dumped Value"
```

