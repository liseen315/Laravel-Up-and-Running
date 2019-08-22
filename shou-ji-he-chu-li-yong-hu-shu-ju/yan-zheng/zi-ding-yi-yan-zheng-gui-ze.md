# 自定义验证规则

如果您需要的验证规则在Laravel中不存在，您可以创建自己的验证规则。要创建自定义规则，请运行php artisan make:rule rulename，然后在app/rules/rulename.php中编辑该文件。

在文件内有两个方法:passes\(\) 和 message\(\)，passes接收属性名作为第一个参数，用户提供的值作为第二个参数，然后返回一个布尔值指示验证是否通过，message返回验证错误消息，您可以将:attribute用作消息中属性名称的占位符。

看一下示例7-15

{% code-tabs %}
{% code-tabs-item title="Example 7-15. A sample custom rule" %}
```php
class WhitelistedEmailDomain implements Rule {

    public function passes($attribute, $value) {
        return in_array(str_after($value, '@'), ['tighten.co']); 
    }
    public function message() {
        return 'The :attribute field is not from a whitelisted email provider.'; 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

要使用这个规则,只需要将规则对象实例传递给你的验证器

```php
$request->validate([
    'email' => new WhitelistedEmailDomain,
])
```

> Laravel 5.5版本中使用自定义验证,你需要使用Validator::extend\(\)，更多内容查看[文档](https://laravel.com/docs/5.4/validation#custom-validation-rules)

