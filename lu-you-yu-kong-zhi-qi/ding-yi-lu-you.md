# 定义路由

在Laravel中,routes/web.php定义web路由,routes/api.php定义API路由,web路由用于终端用户访问,API路由用于你的API,现在我们主要看web路由

{% hint style="info" %}
Laravel 5.3版本路由文件

5.3版本的Laravel只有一个路由文件,它位于app/Http/routes.php
{% endhint %}

定义路由最简单的方式是用"/"加闭包函数,看例子3-1

{% code-tabs %}
{% code-tabs-item title="routes/web.php" %}
```php
<?php

Route::get('/',function () {
    return 'Hello World!';
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

