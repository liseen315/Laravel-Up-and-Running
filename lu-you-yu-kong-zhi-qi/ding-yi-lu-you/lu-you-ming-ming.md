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

这个例子阐述了一些新概念,我们在get\(\)方法后通过name\(\)并为其提供一个简短的别名，以便于在其他地方引用。

> Laravel 5.1版本没有这个功能,使用如下方式定义,具体请查看[文档](https://laravel.com/docs/5.1/routing)

```php
Route::get('members/{id}', [
    'as' => 'members.show',
    'uses' => 'MembersController@show',
]);
```

在我们的示例中，我们将此路由命名为members.show；使用资源.动作的方式是Laravel中路由和视图名称的常见约定

```text
路由命名约定
您可以随意命名您的路由，但通常的惯例是使用资源名称的复数形式,
然后是句点，然后是动作。因此，下面是名为photo的资源最常见的路由
photos.index
photos.create
photos.store
photos.show
photos.edit
photos.update
photos.destroy
更多内容查看 47页的“Resource Controllers” 
```

这个例子还介绍了route\(\)辅助函数,就像url\(\)辅助函数一样,它用于简化视图到命名路由之间的链接,如果路由没有参数,只需要传递名称\(route\('members.show'\)\)就会收到一个路由字符串\(http://myapp.com/members\),如果有参数把参数作为数组传递给route\(\)第二个参数,具体参见例子3-9

通常，我建议使用路由名而不是路径来引用您的路由，因此使用route\(\)而不是url\(\)。 有时它会变得有点笨拙 - 例如，如果你正在使用多个子域 - 但它提供了令人难以置信的灵活性，以便以后更改应用程序的路由结构

> 通过route\(\)辅助函数访问路由参数
>
> 当你的路由带有参数\(例如:users/id\),当您使用route\(\)辅助函数生成到路由的链接时，需要定义这些参数
>
> 传递这些参数有几种不同的方法。让我们设想一个定义为users/userid/comments/commentid的路由。如果用户ID为1，而注释ID为2，那么让我们看看我们可以使用的一些选项：

```php
方法一:
route('users.comments.show', [1, 2]) 
// http://myapp.com/users/1/comments/2
方法二:
route('users.comments.show', ['userId' => 1, 'commentId' => 2]) 
// http://myapp.com/users/1/comments/2
方法三:
route('users.comments.show', ['commentId' => 2, 'userId' => 1]) 
// http://myapp.com/users/1/comments/2
方法四:
route('users.comments.show', ['userId' => 1, 'commentId' => 2, 'opt' => 'a']) 
// http://myapp.com/users/1/comments/2?opt=a
```

可以看出非键值对的数组是按照顺序传递的,键值对的可以不用考虑顺序,多余的参数按照查询参数添加

