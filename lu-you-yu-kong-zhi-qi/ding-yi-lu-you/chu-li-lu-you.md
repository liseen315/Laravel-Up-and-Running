# 处理路由

你可能已经猜到,通过闭包来处理路由不是唯一的一种方式,闭包虽然快而且简单,但是随着应用越来越庞大,将路由的逻辑都放在一个文件内就显得很笨拙了,此外使用路由闭包不能利用Laravel的路由缓存\(稍后会详细介绍\),这会使每个请求都增加数百毫秒的响应时间

另外一种操作是通过控制器名和方法字符串替换闭包函数,如示例3-4

{% code-tabs %}
{% code-tabs-item title="Example 3-4. Routes calling controller methods" %}
```php
<?php

Route::get('/','WelcomeController@index');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这会告诉Laravel将请求传递给`app\http\controllers\welcomecontroller`的`index`方法,这个方法将接收相同的参数并与闭包函数处理方式一致.

> Laravel控制器/方法引用语法
>
> Laravel对于如何引用给定控制器中的特定方法有一个约定:controllername@methodname。有时，这只是一种非正式的通信约定，但它也用于实际绑定中，如示例3-4中所示。Laravel解析@之前和之后的内容，并使用这些段来标识控制器和方法.Laravel 5.7还引入了"tuple"语法,\(Route::get\('/', \[WelcomeController::class, 'index'\]\)\),但通常都用controllername@methodname语法

