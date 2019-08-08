# 路由命名

在应用程序其他地方引用路由的最简单办法是通过路径，有一个url\(\)全局帮助函数来简化视图中的链接,参见示例3-8，help\(\)将为你的路由加上站点的完整域前缀

{% code-tabs %}
{% code-tabs-item title="Example 3-8. The url\(\) helper" %}
```php
<a href="<?php echo url('/'); ?>">main</a>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Laravel还允许命名路由,这使你可以在不显示引用URL的情况下引用路由,这是很有帮助的,这意味着你可以给复杂的路由提供简单的昵称,也意味着如果路径发生变化,你不用去重写前端的链接地址\(参加示例3-9\)

{% code-tabs %}
{% code-tabs-item title="Example 3-9. Defining route names" %}
```php
Route::get('members/{id}','MemberController@show')->name('members.show');
<a href="<?php echo route('members.show',['id'=>14]);?>">members</a>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

