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

模型工厂位于database/factories，Laravel 5.5及更高版本通常在其自己类内定义，键和闭包定义如何创建类实例,$factory-&gt;define\(\)将工厂名作为第一个参数,闭包作为第二个参数.

> Laravel 5.4与之前版本的模型工厂文件
>
> 在Laravel 5.5之前的版本,所有工厂都位于database/ factories/ModelFactory.php,直到5.5版本每个工厂都没有指定单独的类

要生成新的工厂类,请使用Artisan make:factory命令行,就像命名工厂键名一样,通常用Eloquent模型的实例名来命名.

```text
php artisan make:factory ContactFactory
```

这将生成一个名为ContactFactory.php并位于database/factories目录的文件,如示例5-8我们为联系人创建了

{% code-tabs %}
{% code-tabs-item title="Example 5-8. The simplest possible factory definition" %}
```php
$factory->define(Contact::class, function (Faker $faker) {
    return [
        'name' => 'Lupita Smith',
        'email' => 'lupita@gmail.com'
    ];
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

现在我们可以使用全局factory\(\)辅助函数创建联系人实例用于填充数据跟测试了

```php
// Create one
$contact = factory(Contact::class)->create(); 
// Create many
factory(Contact::class, 20)->create();
```

然而,我们用这个工厂创建20条联系人记录,所有的20条信息都一样,这没啥用.

我们可以从传入闭包的Faker实例获取模型工厂的更多益处,Faker创建随机伪数据很容易,示例5-9将之前的代码改造成如下

{% code-tabs %}
{% code-tabs-item title="Example 5-9. A simple factory, modified to use Faker" %}
```php
$factory->define(Contact::class, function (Faker $faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->email,
    ];
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

现在，我们每次使用模型工厂创建contact都变成了随机生成

> 保证随机数据的唯一性
>
> 如果你想生成的数据都是唯一的,你可以使用Faker的unique\(\)方法

> return \['email' =&gt; $faker-&gt;unique\(\)-&gt;email\];

**使用模型工厂**

使用模型工厂主要包含两方面内容:测试,我们将在12章讨论,以及填充数据,让我们看下使用模型工厂填充数据,查看示例5-10

{% code-tabs %}
{% code-tabs-item title="Example 5-10. Using model factories" %}
```php
factory(Post::class)->create([
    'title' => 'My greatest post ever',
]);
// Pro-level factory; but don't get overwhelmed!
factory(User::class, 20)->create()->each(function ($u) use ($post) { 
    $post->comments()->save(factory(Comment::class)->make([
        'user_id' => $u->id,
    ]));
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

为了创建一个对象,我们使用全局函数factory,给它传递了一个工厂名字,正如你看到的,它是我们要生成Eloquent类实例的名字,函数返回了一个工厂实例,然后我们调用实例上面的create\(\)或者是make\(\)方法

这俩方法生成指定模型的实例,不同处在于make\(\)生成实例但是不会塞数据到数据库，create\(\)会立即保存数据到数据库，示例5-10这俩方法都用到了.

在本章后面讨论Eloquent关系时，第二个例子更有说明意义.

**调用模型工厂覆盖属性**

如果你给make\(\)或者是create\(\)传递一个数组,你可以通过工厂覆盖指定的键,就像示例5-10所示在评论上设置user\_id以及手动设置post标题

**使用模型工厂生成多个实例**

如果你给factory第二个参数传递一个数字,你将创建多个实例，它不会返回单个实例，而是返回实例集合，这意味着您可以将结果视为数组，可以将其每个实例与另一个实例相关联，也可以在每个实例上调用方法，如示例5-10，我们给每个新创建的用户添加评论

**更高级的模型工厂**

既然我们已经讨论了模型工厂的日常使用方法,现在让我们深入讨论一些更深入的用法

**定义模型工厂时附加关系**

有时你需要在对象之间创建关系,你可以在一个属性上使用闭包,创建关系并提取ID，如示例5-11

{% code-tabs %}
{% code-tabs-item title="Example 5-11. Creating a related term item in a seeder" %}
```php
$factory->define(Contact::class, function (Faker\Generator $faker) { 
    return [
        'name' => 'Lupita Smith',
        'email' => 'lupita@gmail.com',
        'company_id' => function () {
            return factory(App\Company::class)->create()->id; 
        },
    ]; 
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

每个闭包函数都可以接收一个参数,如示例5-12

{% code-tabs %}
{% code-tabs-item title="Example 5-12. Using values from other parameters in a seeder" %}
```php
$factory->define(Contact::class, function (Faker\Generator $faker) {
    return [
        'name' => 'Lupita Smith',
        'email' => 'lupita@gmail.com',
        'company_id' => function () {
            return factory(App\Company::class)->create()->id;
        },
        'company_size' => function ($contact) {
            // Uses the "company_id" property generated above return App\Company::find($contact['company_id'])->size;
        },
    ];
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**定义和访问多个模型工厂状态**

让我们回顾一下ContactFactory.php，我们定义了一个基础的联系人工厂:

```php
$factory->define(Contact::class, function (Faker $faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->email,
    ];
});
```

有时一个对象类需要多个工厂，有时候我们需要添加非常重要的联系人\(VIP\)应该怎么做,我们可以使用state\(\)方法为此定义第二个工厂状态,如示例5-13，第一个参数仍然是要生成实例的名字,第二个参数是状态名字,第三个参数是状态的属性数组.

{% code-tabs %}
{% code-tabs-item title="Example 5-13. Defining multiple factory states for the same model" %}
```php
$factory->define(Contact::class, function (Faker $faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->email,
    ];
});

$factory->state(Contact::class,'vip',[
    'vip'=>true,
]);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果属性需要一个以上的静态值,你可以用闭包代替数组,然后返回一个你想要修改的属性的数组,如示例5-14

{% code-tabs %}
{% code-tabs-item title="Example 5-14. Specifying a factory state with a closure" %}
```php
$factory->state(Contact::class, 'vip', function (Faker\Generator $faker) {
    return [
        'vip' => true,
        'company' => $faker->company,
    ];
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

下面我们可以创建实例

```php
 $vip = factory(Contact::class, 'vip')->create();
 $vips = factory(Contact::class, 'vip', 3)->create();
```

> Laravel 5.3之前的工厂状态
>
> 在运行5.3之前的Laravel版本的项目中，工厂状态被称为工厂类型，您需要使用$factory-&gt;defineas\(\)而不是$factory-&gt;state\(\)。您可以在[5.2文档](https://laravel.com/docs/5.2/testing#model-factories)中了解更多信息。

嗯，说了不少了，到此是最后一点高级货。让我们回归基础,讨论关于Laravel的核心数据库工具-查询器

