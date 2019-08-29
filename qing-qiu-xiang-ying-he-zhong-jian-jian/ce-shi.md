# 测试

除了在您自己的测试中使用请求，响应和中间件以外，Laravel本身实际上做了很多。

当你使用$this-&gt;get\('/'\)这样的调用进行应用程序测试时，您将指示Laravel的应用程序测试框架生成表示您所描述的交互的请求对象。然后将这些请求对象作为实际访问传递到应用程序。这就是应用程序测试如此准确的原因：您的应用程序实际上并不“知道”与之交互的不是真正的用户。

在这种情况下，您所做的许多断言，比如assertResponseOK\(\)，都是针对由应用程序测试框架生成的响应对象的断言。assertResponseOK\(\)方法查看响应对象，并断言其isOk\(\)方法返回true，检查其状态代码是否为200。最后，应用程序测试中的所有内容都表现得好像这是一个真正的页面请求。

在测试中发现自己需要一个请求来处理上下文？您始终可以使用$request=request\(\)从容器中提取一个。或者您可以为请求类创建自己的构造函数参数，所有这些参数都是可选的，如下所示

```php
$request = new Illuminate\Http\Request(
    $query, // GET array
    $request, // POST array
    $attributes, // "attributes" array; empty is fine
    $cookies,// Cookies array
    $files,// Files array
    $server,// Servers array
    $content// Raw body data
);
```

如果你对示例感兴趣，你可以查看Symfony方法从全局PHP创建一个新请求:Symfony\Component\HttpFoundation \Request@createFromGlobals\(\)

如果需要，手动创建响应对象更简单。带有可选参数：

```php
$response = new Illuminate\Http\Response( 
    $content, // response content 
    $status, // HTTP status, default 200 
    $headers // array headers array
);
```

最后，如果您需要在应用程序测试期间禁用中间件，请将without中间件特性导入到该测试中。您还可以使用$this-&gt;withoutMiddle ware\(\)方法禁用中间件，只针对单个测试方法。

