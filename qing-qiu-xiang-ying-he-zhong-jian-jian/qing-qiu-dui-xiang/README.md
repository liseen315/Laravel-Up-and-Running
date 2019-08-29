# 请求对象

Illuminate\Http\Request 是Symfony’s HttpFoundation\Request的特殊扩展。

> Symfony HttpFoundation
>
> 如果你不熟悉的话, 其实Symfony’s HttpFoundation 现在几乎支持所有的PHP框架; 它是PHP中最流行的一组抽象，用于表示HTTP请求，响应，头，cookies等

请求对象代表你关心的用户HTTP请求。

在原生PHP代码中，你可能会发现$\_SERVER, $\_GET, $\_POST，以及其他全局的组合用于获取当前用户的请求，用户上传了什么文件？他们的IP地址是？他们提交了什么字段？这一堆充斥在整个代码中，而且难以理解。

Symfony 的请求对象将HTTP请求封装成一个单独的对象，然后附加了一些方便的方法用于获取信息，Illuminate请求对象添加了更多的方法用于方便获取信息。

> 捕获请求
>
> 在Laravel应用中你可能永远不需要这样做，但如果你要在PHP全局捕获Illuminate请求，你可以使用capture\(\)方法。

> $request = Illuminate\Http\Request::capture\(\);

