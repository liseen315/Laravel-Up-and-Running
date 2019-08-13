# 使用视图composers绑定数据到视图

谢天谢地，有一种简单的方式,它被称为view composers,它允许你在特定视图加载时定义,然后将数据传递给视图,而不必在显示的在路由定义时传递数据.

假设你在网站的每个页面都有一个侧边栏,然后命名为partials.sidebar,位于\(resources/views/partials/sidebar.blade.php\),然后所有页面都会包含它,这个侧边栏显示你网站最后发送的7条文章列表,如果定义在每个页面上,那么每个路由定义通常需要抓取列表数据然后将其传入视图,如示例4-21所示

{% code-tabs %}
{% code-tabs-item title="Example 4-21. Passing sidebar data in from every route" %}
```php
Route::get('home', function () { 
        return view('home')
        ->with('posts', Post::recent());
});
Route::get('about', function () { 
        return view('about')
        ->with('posts', Post::recent());
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这么干是很崩溃的。 相反，我们将使用视图Composer与规定的一组视图“共享”该变量。 我们可以通过几种方式实现这一目标，所以让我们从简单开始吧

_**全局共享变量**_

首先,像示例4-22一样在所有视图内共享一个全局变量是很容易的.

{% code-tabs %}
{% code-tabs-item title="Example 4-22. Sharing a variable globally" %}
```php
// Some service provider
public function boot() {
    ...
    view()->share('recentPosts', Post::recent());
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果你想使用view\(\)-&gt;share\(\)，最好把它放到服务提供商的boot\(\)方法内,这样就可以在页面加载时运行,你可以创建自定义ViewComposerServiceProvider（有关服务提供商的更多信息，请参阅第11章），但是现在只需要将它放到App\Providers\AppServiceProvider的boot方法内.

使用view\(\)-&gt;share\(\)可以在整个应用的所有页面共享变量,但是它可能有点矫枉过正了。

_**用闭包控制视图作用域**_

接下来的操作使用基于闭包的视图composer给单个视图共享变量,如示例4-23所示

{% code-tabs %}
{% code-tabs-item title="Example 4-23. Creating a closure-based view composer" %}
```php
view()->composer('partials.sidebar', function ($view) { 
    $view->with('recentPosts', Post::recent());
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

正如你所看到的,我们通过第一个参数定义了想要共享变量的视图名字,第二个参数传递了一个闭包函数,闭包内我们使用$view-&gt;with\(\)共享变量.

> 多视图View Composers
>
> 一个视图composer绑定到一个特定的视图\(如示例4-23绑定到了partials.sidebar\),你也可以传递一个视图名数组用于绑定多个视图.
>
> 你也可以在视图路径中使用\*号,例如partials.\* or tasks.\*

```php
view()->composer(
    ['partials.header', 'partials.footer'], 
    function () {
       $view->with('recentPosts', Post::recent());
    }
);
view()->composer('partials.*', function () { 
    $view->with('recentPosts', Post::recent());
});
```

_**使用类控制作用域**_

最后，最灵活也是最复杂的是给视图Composer创建一个专门的类

首先创建一个View Composer，文档推荐存放在App\Http\ViewComposers路径下,于是我们如示例4-24创建App\Http\ViewComposers\RecentPostsComposer类

{% code-tabs %}
{% code-tabs-item title="Example 4-24. A view composer" %}
```php
<?php

namespace App\Http\ViewComposers;

use App\Post;
use Illuminate\Contracts\View\View;

class RecentPostsComposer
{
    public function compose(View $view)
    {
        $view->with('recentPosts', Post::recent());
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

正如你所看到的,当composer被调用的时候,它会运行compose方法,然后会绑定Post::recent\(\)模型的结果到'recentPosts'

就像其他共享变量方法一样,view composer需要绑定在某个地方,所以你现在需要创建一个ViewComposerServiceProvider,如示例4-25，我们将它放到App \Providers\AppServiceProvider的boot方法内.

{% code-tabs %}
{% code-tabs-item title="Example 4-25. Registering a view composer in AppServiceProvider" %}
```text
public function boot()
{
    view()->composer('partials.sidebar',\App\Http\ViewComposers\RecentPostsComposer::class);
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

请注意，此绑定与基于闭包的视图composer相同，但我们传递的不是闭包，而是视图composer的类名,现在每次Blade渲染partials.sidebar视图,都会自动运行我们的Provider，然后会把模型数据结果传递给视图.



