# 创建自定义中间件

让我们想象一个中间件使用DELETE HTTP方法拒绝请求，并为每个请求发送cookie。

有一个Artisan命令来创建自定义中间件。让我们试试看。

```text
php artisan make:middleware BanDeleteMethod
```

在app/Http/Middleware/BanDeleteMethod.php打开文件，文件默认内容如下,示例10-17

{% code-tabs %}
{% code-tabs-item title="Example 10-17. Default middleware contents" %}
```php
...
class BanDeleteMethod {
    public function handle($request, Closure $next) {
        return $next($request); 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

handle\(\)方法用于表述传入跟传出，这是中间件最难理解的地方，让我们来看看它。

**理解中间件的handler\(\)方法**

首先，记住中间件是**一层层**的，最后是在应用程序的顶部。注册的第一个中间件在请求进入时首先获得对该请求的访问权，然后该请求依次传递给其他所有中间件，然后传递给应用程序；然后结果响应通过中间件向外传递，最后该最外层的中间件可以访问响应。

让我们假设我们已经注册了BanDeleteMethod作为第一个运行的中间件。 这意味着进入它的$ request是原始请求，不受任何其他中间件的影响。 然后呢？

接着该请求传递给$ next\(\)意味着将其交给其他中间件。 $ next\(\)闭包只接受$ request并将其传递给堆栈中下一个中间件的handle\(\)方法。 然后它一直被传递，直到没有更多的中间件，最终它终止于应用程序。

接下来，如何作出反应？这可能很难理解。应用程序返回一个响应，该响应被传递回中间件链，因为每个中间件都返回其响应。因此，在相同的handle\(\)方法中，中间件可以修饰一个$request并将其传递给$next\(\)闭包，然后在最终将输出返回给最终用户之前对它接收的输出执行一些操作。让我们看一些伪代码来更清楚地说明这一点（示例10-18）

{% code-tabs %}
{% code-tabs-item title="Example 10-18. Pseudocode explaining the middleware call process" %}
```php
class BanDeleteMethod
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request $request
     * @param  \Closure $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        // At this point, $request is the raw request from the user. 
        // Let's do something with it, just for fun.
        if ($request->ip() === '192.168.1.1') {
            return response('BANNED IP ADDRESS!', 403);
        }
        // Now we've decided to accept it. Let's pass it on to the next
        // middleware in the stack. We pass it to $next(), and what is
        // returned is the response after the $request has been passed
        // down the stack of middleware to the application and the
        // application's response has been passed back up the stack.

        $response = $next($request);

        // At this point, we can once again interact with the response
        // just before it is returned to the user $response->cookie('visited-our-site', true);
        $response->cookie('visited-our-site', true);

        // Finally, we can release this response to the end user
        return $response;
    }
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

最后，让我们让中间件做我们实际承诺的事情（示例10-19）。

{% code-tabs %}
{% code-tabs-item title="Example 10-19. Sample middleware banning the delete method" %}
```php
class BanDeleteMethod
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request $request
     * @param  \Closure $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        // Test for the DELETE method
        if ($request->method() === 'DELETE') {
            return response(
                "Get out of here with that delete method",
                405
            );
        }
        $response = $next($request); // Assign cookie
        $response->cookie('visited-our-site', true);
        // Return response
        return $response;
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

