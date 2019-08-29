# 绑定中间件

我们还没完成。 我们需要以两种方式之一注册中间件：全局或特定路由。

全局中间件应用于每条路由，路由中间件按路由应用。

**绑定全局中间件**

这两个绑定在app/http/kernel.php中。要将中间件添加为全局中间件，请将其类名添加到$middleware属性中，如示例10-20所示。

{% code-tabs %}
{% code-tabs-item title="Example 10-20. Binding global middleware" %}
```php
// app/Http/Kernel.php
protected $middleware = [ 
    \Illuminate\Foundation\Http\Middleware\CheckForMaintenanceMode::class,
    \App\Http\Middleware\BanDeleteMethod::class,
];
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**绑定路由中间件**

针对特定路由的中间件可以作为路由中间件添加，也可以作为中间件组的一部分添加。让我先看前者。

路由中间件添加到app/http/kernel.php中的$route middleware数组中。它类似于将它们添加到$middleware，只是我们必须给一个key，在将这个中间件应用到一个特定的路由时将使用这个key，如示例10-21所示。

{% code-tabs %}
{% code-tabs-item title="Example 10-21. Binding route middleware" %}
```php
// app/Http/Kernel.php
protected $routeMiddleware = [
    'auth' => \App\Http\Middleware\Authenticate::class,
    ...
    'ban-delete' => \App\Http\Middleware\BanDeleteMethod::class,
];
```
{% endcode-tabs-item %}
{% endcode-tabs %}

至此，我们可以在路由定义内使用中间件了，如示例10-22

{% code-tabs %}
{% code-tabs-item title="Example 10-22. Applying route middleware in route definitions" %}
```php
// Doesn't make much sense for our current example...
Route::get('contacts', 'ContactsController@index')->middleware('ban-delete');
// Makes more sense for our current example...
Route::prefix('api')->middleware('ban-delete')->group(function () { 
    // All routes related to an API
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**使用中间件组**

Laravel5.2引入了中间件组的概念。它们本质上是预先打包的中间件捆绑包，在特定的上下文中放在一起是有意义的。

开箱即用有两个组：Web和API。Web拥有所有的中间件，几乎在每个Laravel页面请求上都很有用，包括用于cookie、会话和CSRF保护的中间件。API没有节流中间件和路由模型绑定中间件，就是这样。这些都在app/http/kernel.php中定义。

您可以使用middleware**\(\)**fluent方法将中间件组应用于路由，就像将路由中间件应用于路由一样。

```text
Route::get('/', 'HomeController@index')->middleware('web');
```

您还可以创建自己的中间件组，并在预先存在的中间件组之间添加和删除路由中间件。 它的工作方式就像正常添加路由中间件一样，你可以将它们添加到$ middlewareGroups数组中的键内。

您可能想知道这些中间件组如何与两个默认路由文件匹配。不出意料，routes/web.php文件用web中间件组，routes/api.php文件用api中间件组。

routes/\*在RouteServiceProvider被加载。查看示例10-23的map方法，你将会发现mapWebRoutes和mapApiRoutes，他们都将加载已经包装在适当的中间件组中的相应文件。

{% code-tabs %}
{% code-tabs-item title="Example 10-23. Default route service provider in Laravel 5.3+" %}
```php
// App\Providers\RouteServiceProvider
public function map() {
    $this->mapApiRoutes();
    $this->mapWebRoutes();
}
protected function mapApiRoutes() {
    Route::prefix('api')
         ->middleware('api')
         ->namespace($this->namespace)
         ->group(base_path('routes/api.php'));
}
protected function mapWebRoutes() {
    Route::middleware('web')
         ->namespace($this->namespace)
         ->group(base_path('routes/web.php'));
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如您在示例10-23中所看到的，我们使用路由器在默认名称空间（app\http\controllers）和Web中间件组下加载路由组，在API中间件组下加载路由组。

