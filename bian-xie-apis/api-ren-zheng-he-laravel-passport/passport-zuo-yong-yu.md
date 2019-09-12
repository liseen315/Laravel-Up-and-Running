# Passport 作用域

如果您熟悉oauth，您可能已经注意到我们还没有讨论过很多关于作用域的内容。到目前为止，我们所讨论的所有内容都可以按作用域定制，但在讨论之前，让我们先快速介绍一下作用域是什么。

在oauth中，作用域是定义的特权集，而不是“无所不能”的。例如，如果您以前获得过github api令牌，您可能会注意到应用程序想访问您的姓名和电子邮件地址，有些应用程序想访问您的所有仓库，等等。我想知道你的信息。每一个都是一个“范围”，作用域允许用户跟消费者定义消费者需要的权限。

如例13-37所示，您可以在authServiceProvider的boot\(\)方法中定义应用程序的作用域.

{% code-tabs %}
{% code-tabs-item title="Example 13-37. Defining Passport scopes" %}
```php
// AuthServiceProvider
use Laravel\Passport\Passport; ...
public function boot() {
    ...
    Passport::tokensCan([
        'list-clips' => 'List sound clips',
        'add-delete-clips' => 'Add new and delete old sound clips',
        'admin-account' => 'Administer account details',
    ]); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

一旦定义了作用域，消费者应用程序就可以定义它请求访问的作用域。只需在初始重定向的scope字段中添加一个用空格分隔的令牌列表，如示例13-38所示。

{% code-tabs %}
{% code-tabs-item title="Example 13-38. Requesting authorization to access specific scopes" %}
```php
// In SpaceBook's routes/web.php:
Route::get('tweeter/redirect', function () { 
        $query = http_build_query([
                'client_id' => config('tweeter.id'),
                'redirect_uri' => url('tweeter/callback'),
                'response_type' => 'code',
                'scope' => 'list-clips add-delete-clips',
        ]);
        return redirect('http://tweeter.test/oauth/authorize?' . $query); 
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

当用户尝试使用此应用进行授权时，它将显示请求的作用域列表。这样，用户将知道是“spacebook正在请求查看您的电子邮件地址”还是“spacebook正在请求访问您的post，并删除您的post和给您的朋友发送消息”。

您可以使用中间件或在用户实例上检查作用域。示例13-39显示了如何检查用户。

{% code-tabs %}
{% code-tabs-item title="Example 13-39. Checking whether the token a user authenticated with can perform a given action" %}
```php
Route::get('/events', function () {
    if (auth()->user()->tokenCan('add-delete-clips')) {
        //
    } 
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

您也可以使用两个中间件：scope和scopes。要在应用程序中使用它们，请将它们添加到app/http/kernel.php文件中的$routeMiddleware中：

```text
'scopes' => \Laravel\Passport\Http\Middleware\CheckScopes::class,
'scope' => \Laravel\Passport\Http\Middleware\CheckForAnyScope::class,
```

现在可以使用示例13-40中所示的中间件。作用域要求所有定义的作用域都在用户令牌上，以便用户访问路由，而作用域要求至少有一个定义的作用域在用户令牌上。

{% code-tabs %}
{% code-tabs-item title="Example 13-40. Using middleware to restrict access based on token scopes" %}
```text
// routes/api.php
Route::get('clips', function () {
// Access token has both the "list-clips" and "add-delete-clips" scopes
})->middleware('scopes:list-clips,add-delete-clips');
// or
Route::get('clips', function () {
// Access token has at least one of the listed scopes
})->middleware('scope:list-clips,add-delete-clips')
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果您没有定义任何作用域，则应用程序将像不存在一样工作。但是，在使用作用域时，您的消费者应用程序将必须显式定义它们请求访问的作用域。此规则的一个例外是，如果您使用的是密码授予类型，则您的消费者应用程序可以请求\*作用域，该作用域允许令牌访问所有内容。



