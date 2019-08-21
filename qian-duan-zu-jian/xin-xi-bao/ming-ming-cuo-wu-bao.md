# 命名错误包

有时，您不仅需要按键（通知与错误）区分消息包，还需要按组件区分消息包。也许你在同一个页面上有一个登录表单和一个注册表单；你如何区分它们？

使用withErrors\(\)发送错误和重定向时，第二个参数是包的名称：redirect\('dashboard'\)-&gt;withErrors\($validator, 'login'\)。然后，在仪表板上，您可以使用$errors-&gt;login调用以前看到的所有方法：any\(\)、count\(\)等等。

