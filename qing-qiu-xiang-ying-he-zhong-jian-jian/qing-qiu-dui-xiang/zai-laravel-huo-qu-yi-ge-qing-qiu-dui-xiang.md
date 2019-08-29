# 在Laravel获取一个请求对象

Laravel为每个请求创建了一个内置的请求对象，有如下方法可以访问到它。

首先，我们会在第11章介绍更多,我们可以在容器或者是构造器内对类进行类型约束,这意味着你可以在控制器方法或者服务提供者中类型约束它，如示例10-2

{% code-tabs %}
{% code-tabs-item title="Example 10-2. Typehinting in a container-resolved method to receive a Request object" %}
```php
...
use Illuminate\Http\Request;
class PeopleController extends Controller {
public function index(Request $request) {
        $allInput = $request->all();
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

或者你可以使用request\(\)全局函数然后调用其方法\(例如request\(\)-&gt;input\(\)\),或者调用其示例方法。

```php
$request = request(); 
$allInput = $request->all(); 
// or
$allInput = request()->all();
```

最后，你可以使用app\(\)全局方法获取请求实例，你可以传递完全限定类名或者是快捷请求

```php
$request = app(Illuminate\Http\Request::class);
$request = app('request');
```

