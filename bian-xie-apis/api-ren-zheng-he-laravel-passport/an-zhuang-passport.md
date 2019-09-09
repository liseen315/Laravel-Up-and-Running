# 安装Passport

passport是一个单独的包，所以第一步是安装它。我将在这里总结这些步骤，但是您可以在文档中获得更深入的安装说明。

首先，使用Composer安装：

```text
composer require laravel/passport
```

如果你使用的是低于5.5的Laravel版本，请添加Laravel\Passport\Pass portServiceProvider::class到config/app.php的providers数组。

Passport导入了一系列迁移，所以运行php artisan migrate来创建这些表，用于OAuth客户端，作用域以及令牌。

接着运行php artisan passport:install进行安装，它将为OAuth服务生成秘钥\(storage/oauth-private.key 和 storage/oauth-public.key\)，将OAuth客户端插入数据库以获取我们的个人和密码授予类型令牌（稍后介绍）。

你需要导入Laravel\Passport\HasApiTokens特性到用户模型中，这将向每个用户添加与OAuth客户端和令牌相关的关系，以及一些与令牌相关的助手方法，接着在AuthServiceProvider的boot方法中添加Laravel\Passport \Passport::routes\(\)，它将添加如下路由：

* oauth/authorize
* oauth/clients
* oauth/clients/client\_id
* oauth/personal-access-tokens
* oauth/personal-access-tokens/token\_id
* oauth/scopes
* oauth/token
* oauth/token/refresh
* oauth/tokens
* oauth/tokens/token\_id

最后，看一下config/auth.php中的API守卫，默认情况下守卫使用令牌驱动\(稍后介绍\)，现在改成passport驱动。

至此你拥有了一个健全的OAuth 2.0服务，现在你可以用php artisan passport:client创建客户端，然后你有一个API用于管理你的客户端和在/oauth下的可用token。

要保护Passport auth后的路由，添加auth:api到路由或者路由组，如13-28所示

```php
// routes/api.php
Route::get('/user', function (Request $request) { 
    return $request->user();
})->middleware('auth:api');
```

为了对这些受保护的路由进行认证，你需要在Authorization头上传递一个token作为Bearer token\(稍后介绍如何获取令牌\)，示例13-29展示使用Guzzle HTTP库发送请求的情况。

```php
$http = new GuzzleHttp\Client;
$response = $http->request('GET', 'http://tweeter.test/api/user', [
    'headers' => [
        'Accept' => 'application/json',
        'Authorization' => 'Bearer ' . $accessToken,
    ], 
]);
```

现在让我们仔细看看它是如何工作的。

