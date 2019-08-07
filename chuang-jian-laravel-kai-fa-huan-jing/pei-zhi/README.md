# 配置

Laravel核心配置目录包含数据库连接,队列,邮件设置,这些文件都会返回一个PHP数组，数组中的每个值都可以由一个config键访问，该键由文件名和所有子代键组成，用点（.）分隔。

所以你可以在config/services.php键入如下内容

{% code-tabs %}
{% code-tabs-item title="config/services.php" %}
```php
<?php return [
        'sparkpost' => [
            'secret' => 'abcdefg',
], ];
```
{% endcode-tabs-item %}
{% endcode-tabs %}

你可以使用\('services.sparkpost.secret'\)来访问这个配置变量

不同的环境应该有不同的配置\(因此不提交版本控制管理\),这些配置将位于.env文件内,假设您想为每个环境使用不同的bugsnag API密钥,你应该吧配置放到.env内然后从中读取.

{% code-tabs %}
{% code-tabs-item title="config/services.php" %}
```php
<?php return [
        'bugsnag' => [
            'api_key' => env('BUGSNAG_API_KEY'),
], ];
```
{% endcode-tabs-item %}
{% endcode-tabs %}

env\(\)辅助函数可以帮助你使用相同的键值从.env文件内访问配置，现在把键值对放到.env\(当前环境配置\)以及.env.example\(所有环境变量模板\)内

{% code-tabs %}
{% code-tabs-item title=".env" %}
```text
BUGSNAG_API_KEY=oinfp9813410942
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title=".env.example" %}
```text
BUGSNAG_API_KEY=
```
{% endcode-tabs-item %}
{% endcode-tabs %}

.env包含了相当多的配置,类似邮箱驱动,以及基础的数据库设置

{% hint style="info" %}
在配置文件外使用env\(\)辅助函数

如果在配置文件外使用env\(\)函数,那么Laravel的某些功能\(包括缓存和一些优化调用\)将不能调用

引入环境变量的最佳方法是将配置放到config内,然后在应用内通过config变量访问配置
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="config/services.php" %}
```php
return [ 'bugsnag' => [
    'key' => env('BUGSNAG_API_KEY'),
    ],
];
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="controller or 任何地方" %}
```php
$bugsnag = new Bugsnag(config('services.bugsnag.key'));
```
{% endcode-tabs-item %}
{% endcode-tabs %}

