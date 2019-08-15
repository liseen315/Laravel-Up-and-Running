# 模型工厂

模型工厂用于为数据库创建fake\(伪\)数据，默认情况下，每个工厂都以Eloquent类命名，如果不打算用Eloquent类,你可以用表名.如示例5-7，显示用两种方式创建模型工厂.

{% code-tabs %}
{% code-tabs-item title="Example 5-7. Defining model factories with Eloquent class and table name keys" %}
```php
$factory->define(User::class, function (Faker\Generator $faker) {
    return [
        'name' => $faker->name,
    ];
});

$factory->define('users', function (Faker\Generator $faker) {
    return [
        'name' => $faker->name,
    ];
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

理论来讲,你可以随意给工厂改名字,但用Eloquent class命名工厂是最惯用的方法

**创建模型工厂**

\*\*\*\*

