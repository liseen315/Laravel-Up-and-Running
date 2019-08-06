# 是如何运作的

到目前为止,我之前分享的内容都很抽象,你可能会问,代码是什么样的?现在让我们深入看一个例子\(示例1-1\),这样一来你就可以了解平时是怎么写Laravel的

_Example 1-1. “Hello, World” in routes/web.php_

{% code-tabs %}
{% code-tabs-item title="web.php" %}
```php
<?php

Route::get('/',function () {
    return 'Hello,World!';
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

在Laravel应用程序中,你可以采取最简单的操作是定义一个路由,并在任何时候一旦有人访问该路由则会返回一个结果.如果你在机器上初始化一个新的Laravel应用,定义一个路由像示例1-1一样,然后从公共目录为该站点提供服务,那么您将拥有一个功能完整的"Hello,World"示例\(参加图1-1\)





![&#x53C2;&#x8003;&#x56FE; 1-1. Returning &#x201C;Hello, World!&#x201D; with Laravel](../.gitbook/assets/helloword.png)

使用控制器写法来复写例子与之很类似,查看示例Example 1-2

_Example 1-2. “Hello, World” with controllers_

{% code-tabs %}
{% code-tabs-item title="routes/web.php" %}
```php
<?php

Route::get('/','WelcomeController@index');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="app/Http/Controllers/WelcomeController.php" %}
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class WelcomeController extends Controller
{
    public function index() {
        return 'Hello,World';
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果您将问候语存储在数据库中,它看起来也非常相似（参见示例1-3）

_Example 1-3. Multigreeting “Hello, World” with database access_

_未完待续..._

