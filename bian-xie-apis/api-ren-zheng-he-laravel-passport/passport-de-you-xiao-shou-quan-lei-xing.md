# Passport的有效授权类型

Passport使您能够以四种不同的方式对用户进行身份验证。两个是传统的oauth 2.0授权（密码授权和授权码授权），两个是passport特有的便利方法（个人令牌和同步器令牌）。

**密码授权**

密码授权虽然不如授权码授权常见，但要简单得多。如果您希望用户能够使用其用户名和密码直接使用您的api进行身份验证，例如，如果您的公司有一个移动应用程序正在使用您自己的api，则可以使用密码授权。

> 创建一个密码授权客户端
>
> 为了使用密码授予流，您需要数据库中的密码授予客户端。这是因为对oauth服务器的每个请求都需要由客户端发出。通常，客户端会识别用户正在针对哪个应用程序或站点进行身份验证，例如，如果您使用Facebook登录到第三方网站，则该网站将是客户端。
>
> 对于密码授权，伴随着请求没有客户端进来，因此您必须创建一个，这就是密码授予客户机。在运行php artisan passport:install时会添加一个，但是如果出于任何原因需要生成新的密码授予客户机，可以执行以下操作。

```bash
$php artisan passport:client --password
What should we name the password grant client?
[My Application Password Grant Client]:
> Client_name
Password grant client created successfully.
Client ID: 3
Client Secret: Pg1EEzt18JAnFoUIM9n38Nqewg1aekB4rvFk2Pma
```

对于密码授予类型，获得令牌只有一个步骤：将用户凭据发送到/oauth/token路由，如例13-30所示。

{% code-tabs %}
{% code-tabs-item title="Example 13-30. Making a request with the password grant type" %}
```php
// Routes/web.php in the *consuming application*
Route::get('tweeter/password-grant-auth', function () {
    $http = new GuzzleHttp\Client;
    // Make call to "Tweeter," our Passport-powered OAuth server
    $response = $http->post('http://tweeter.test/oauth/token', [
        'form_params' => [
            'grant_type' => 'password',
            'client_id' => config('tweeter.id'),
            'client_secret' => config('tweeter.secret'),
            'username' => 'matt@mattstauffer.co',
            'password' => 'my-tweeter-password',
            'scope' => '',
        ],]);
    $thisUsersTokens = json_decode((string)$response->getBody(), true);
    // Do stuff with the tokens
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

此路由将返回访问令牌和刷新令牌。现在，您可以保存这些令牌，以便使用api（访问令牌）进行身份验证，并在以后请求更多令牌（刷新令牌）。

请注意，用于密码授权类型的id和secret将是passport应用程序的oauth\_clients数据库表中与passport授予客户端名称匹配的行中的id和secret。当你运行passport:install你将在数据表中看到默认生成两个客户端：“Laravel Personal Access Client” 和 “Laravel Password Grant Client.”

**授权码授权**

最常见的OAuth 2.0 auth工作流也是Passport支持的最复杂的工作流，假设我们正在开发一个类似于twitter但用于声音剪辑的应用程序；我们称之为tweeter。我们可以想象另一个网站，一个科幻迷的社交网络，叫做SpaceBook，SpaceBook开发者想要人们嵌入Tweeter数据到他们的SpaceBook newsfeeds中，我们将安装Passport在我们的Tweeter app中，可以允许他们的用户使用他们的tweeter信息进行身份验证。

在授权码授权中，每个网站-示例中的SpaceBook网站需要创建一个客户端，在大多数情况下，其他网站的管理员需要在Tweeter注册有账号，然后我们需要为他们创建客户端构建工具，对于开发商了来说，我们需要手动创建客户端用于SpaceBook管理。

```bash
$php artisan passport:client
Which user ID should the client be assigned to?:
>1
What should we name the client?:
> SpaceBook
Where should we redirect the request after authorization?
[http://tweeter.test/auth/callback]: > http://spacebook.test/tweeter/callback
New client created successfully.
Client ID: 4
Client secret: 5rzqKpeCjIgz3MXpi3tjQ37HBnLLykrgWgmc18uH
```

每个客户端都需要分配给应用程序中的用户。假设用户1正在编写SpaceBook；他们将是我们正在创建的这个客户端的“所有者”。

现在我们有了SpaceBook客户端的ID和秘钥。 此时，SpaceBook可以使用此ID和秘钥来构建工具，允许单个SpaceBook用户（也是Tweeter用户）从Tweeter获取auth令牌，以便SpaceBook对该用户的Tweeter的API进行调用。 例13-31说明了这一点。 （此示例和以下示例假设SpaceBook也是Laravel应用程序;他们还假设Spacebook的开发人员在config / tweeter.php创建了一个文件，该文件返回我们刚创建的ID和秘钥。）

{% code-tabs %}
{% code-tabs-item title="Example 13-31. A consumer app redirecting a user to our OAuth server" %}
```php
// In SpaceBook's routes/web.php:
Route::get('tweeter/redirect', function () { 
        $query = http_build_query([
                'client_id' => config('tweeter.id'),
                'redirect_uri' => url('tweeter/callback'),
                'response_type' => 'code',
                'scope' => '',
        ]);
        // Builds a string like:
        // client_id={$client_id}&redirect_uri={$redirect_uri}&response_type=code
        return redirect('http://tweeter.test/oauth/authorize?' . $query); 
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

当用户在SpaceBook点击该路由，他们将被重定向到Tweeter 的/oauth/ 授权Passport路由，此时将会看到一个确认页，你可以通过运行以下命令使用默认的passport确认页。

```bash
php artisan vendor:publish --tag=passport-views
```

这将发布视图到resources/views/vendor/passport/authorize.blade.php，然后用户将看到如图13-1所示的页面。

![Figure 13-1. OAuth authorization code approval page](../../.gitbook/assets/oauth.png)

一旦用户选择接受或者拒绝授权，用户将被重定向到提供的redirect\_uri，示例13-31展示了设置url\('tweeter/callback'\)的redirect\_uri，所以用户将被重定向到[http://space‐](http://space‐) book.test/tweeter/callback。

批准请求将包含一个code，我们的消费者应用程序的回调路由现在可以使用该code从我们支持Passport的应用程序tweeter中获取令牌。拒绝请求将包含错误。spacebook的回调路径可能类似于示例13-32。

{% code-tabs %}
{% code-tabs-item title="Example 13-32. The authorization callback route in the sample consuming app" %}
```php
// In SpaceBook's routes/web.php:
Route::get('tweeter/callback', function (Request $request) {
    if ($request->has('error')) {
        // Handle error condition
    }
    $http = new GuzzleHttp\Client;
    $response = $http->post('http://tweeter.test/oauth/token', [
        'form_params' => [
            'grant_type' => 'authorization_code',
            'client_id' => config('tweeter.id'),
            'client_secret' => config('tweeter.secret'),
            'redirect_uri' => url('tweeter/callback'),
            'code' => $request->code,
        ],]);
    $thisUsersTokens = json_decode((string)$response->getBody(), true);
    // Do stuff with the tokens
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

SpaceBook开发者已经Tweeter上构建了一个Guzzle HTTP到/oauth/token Passport 的请求。然后，他们发送一个POST请求，其中包含用户批准访问时收到的授权代码，Tweeter将返回包含几个密钥的JSON响应。

access\_token

SpaceBook令牌将为这个用户保存。 此令牌是用户在将来对Tweeter的请求中使用的身份验证（使用Authorization标头）。

refresh\_token

Spacebook 令牌用于设置过期，默认，Passport的令牌时效是一年。

expires\_in

access\_token过期前描述\(需要刷新\)。

token\_type

获取的令牌是不记名的，意味着在未来的请求中传递头，它将包含认证和不记名的值。

**使用刷新令牌**

如果要强制用户频繁地重新验证，则需要在令牌上设置较短的刷新时间，然后可以在需要时使用刷新令牌请求新的访问令牌，最有可能的情况是每当你从API调用收到401（未经授权）响应时。

示例13-33展示如何设置较短的刷新时间。

{% code-tabs %}
{% code-tabs-item title="Example 13-33. Defining token refresh times" %}
```php
// AuthServiceProvider's boot() method
public function boot() {
  $this->registerPolicies();
  Passport::routes();
  // How long a token lasts before needing refreshing
  Passport::tokensExpireIn(
    now()->addDays(15)
  );
  // How long a refresh token will last before re-auth
  Passport::refreshTokensExpireIn(
      now()->addDays(30)
  ); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

要使用刷新令牌请求新令牌，消费应用程序需要首先从示例13-32中的初始身份验证响应中保存refresh\_token。 一旦刷新了，它将进行类似于该示例的调用，但稍微修改，如例13-34所示

{% code-tabs %}
{% code-tabs-item title="Example 13-34. Requesting a new token using a refresh token" %}
```php
// In SpaceBook's routes/web.php:
Route::get('tweeter/request-refresh', function (Request $request) {
    $http = new GuzzleHttp\Client;
    $params = [
        'grant_type' => 'refresh_token',
        'client_id' => config('tweeter.id'),
        'client_secret' => config('tweeter.secret'),
        'redirect_uri' => url('tweeter/callback'),
        'refresh_token' => $theTokenYouSavedEarlier,
        'scope' => '',
    ];
    $response = $http->post(
        'http://tweeter.test/oauth/token',
        ['form_params' => $params]
    );
    $thisUsersTokens = json_decode((string)$response->getBody(), true);
    // Do stuff with the tokens
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

在响应中，消费应用程序将收到一组新的令牌以保存给用户

现在您已经拥有了执行基本授权代码流所需的所有工具。稍后我们将介绍如何为你的客户机和令牌构建管理面板，但首先，让我们快速查看其他授权类型。

**个人访问令牌**

授权码授权非常适合你的用户的应用，密码授权非常适合你自己的应用，但是如果你的用户想要为自己创建代币以测试您的API或在他们开发应用时使用该代码会怎样？ 这就是个人令牌的用途

> 创建一个个人访问客户端
>
> 为了创建个人令牌，您需要在数据库中使用个人访问客户端。运行php artisan passport:install已经添加了一个，但是如果出于任何原因需要生成一个新的个人访问客户端，可以运行php artism passport:client--personal

```text
$php artisan passport:client --personal
What should we name the personal access client?
[My Application Personal Access Client]:
> My Application Personal Access Client 
Personal access client created successfully.
```

个人访问令牌不是一种“授权”类型; 这里没有OAuth规定的流程。 相反，它们是Passport添加的一种便捷方法，可以让您在系统中注册单个客户端变得容易，只是为了方便为开发人员用户创建便利令牌。

例如，你有一个用户想要开发一个名为Racebook的鱼SpaceBook的竞争者，他们想在开发之前先试玩一下Tweeter API，看看是如何工作的，开发人员是否要使用授权码流来创建令牌？不需要，他们都还没开始编写代码呢，这就是个人令牌的用途。

你可以通过JSON API创建个人访问令牌，稍后我们将介绍，但是您也可以在代码中直接为用户创建一个。

```php
 // Creating a token without scopes
$token = $user->createToken('Token Name')->accessToken; 
// Creating a token with scopes
$token = $user->createToken('My Token', ['place-orders'])->accessToken;
```

您的用户可以使用这些令牌，就像使用授权码授权流创建的令牌一样。 我们将在第371页的“Passport Scopes”中详细讨论作用域。

来自Laravel会话认证的令牌\(同步令牌\)

用户可以通过最后一种方式获取令牌来访问您的API，这是Passport添加的另一种便捷方法，但普通OAuth服务器不提供这种方法。 此方法适用于您的用户已经过身份验证的情况，因为他们已正常登录您的Laravel应用，并且您希望应用的JavaScript能够访问API。 使用授权代码或密码授权流程重新验证用户是一件痛苦的事情，因此Laravel为此提供了帮助

如果将Laravel \Passport\Http\Middleware\CreateFreshApiToken中间件添加到Web中间件组（在app / Http / Kernel.php中），Laravel发送给经过身份验证的用户的每个响应都会附加一个名为laravel\_token的cookie。 此cookie是JSON Web令牌（JWT），其中包含有关CSRF令牌的编码信息，现在如果发送带有X-CSRF-TOKEN头和X-Requested-头的Javascript请求，API会将您的CSRF令牌与此cookie进行比较，这将像使用任何其他令牌一样对API用户进行身份验证。

> JSON Web Token \(JWT\)
>
> JWT是一种相对较新的“代表两方之间安全索赔”的格式，在过去几年中已经占据了突出地位。 JSON Web Token是一个JSON对象，包含确定用户身份验证所需的所有信息状态和访问权限。 此JSON对象使用密钥哈希消息身份验证代码（HMAC）或RSA进行数字签名，这使其值得信赖。
>
> 令牌通常是经过编码的，然后通过url或post请求或在头中传递。一旦用户以某种方式向系统进行身份验证，之后的每个http请求都将包含令牌，描述用户的身份和授权。
>
> son web令牌由三个由点（.）分隔的base64编码字符串组成；类似于xxx.yyy.zzz。第一部分是一个base64编码的json对象，包含使用哪种散列算法的信息；第二部分是一系列关于用户授权和身份的“声明”；第三部分是签名，或者第一和第二部分是加密和签名的。使用第一节中指定的算法。
>
> 要了解更多关于JWT的信息，请查看[JWT.IO](https://jwt.io)或[jwt-auth](https://github.com/tymondesigns/jwt-auth)包。

laravel附带的默认javascript引导设置为您设置了这个头文件，但是如果您使用的是不同的框架，则需要手动设置它。示例13-35展示了使用jquery的用法。

{% code-tabs %}
{% code-tabs-item title="Example 13-35. Setting jQuery to pass Laravel’s CSRF tokens and the X-Requested-With header with all Ajax requests" %}
```javascript
$.ajaxSetup({
    headers: {
        'X-CSRF-TOKEN': "{{ csrf_token() }}",
        'X-Requested-With': 'XMLHttpRequest'
    }
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果您将CreateFreshApiToken中间件添加到Web中间件组并在每个JavaScript请求中传递这些头，您的JavaScript请求将能够访问受Passport保护的API路由，而不必担心授权代码或密码授予的任何复杂性。

