# CSRF保护

如果您已经尝试在Laravel应用程序中提交一个表单，包括示例3-35中的表单，那么您可能会遇到TokenMismatchException异常

默认情况下，除了“只读”路由（那些使用GET、HEAD或选项的路由）之外，Laravel中的所有路由都会受到跨站点请求伪造（CSRF）攻击的保护，方法是将名为\_token的输入形式的令牌与每个请求一起传递。此令牌在每个会话开始时生成，并且每个非只读路由将提交的令牌与会话令牌进行比较。

> 什么是CSRF
>
> 跨网站请求伪造是指一个网站假装是另一个网站。目标是有人通过登录用户的浏览器将表单从其网站提交到您的网站，从而劫持您的用户访问您的网站的权限。
>
> 对付CSRF攻击的最佳方法是使用令牌保护所有入站路由-POST，DELETE等，Laravel开箱即用

你有两种方法来处理找个CSRF错误,首选方案是为你的每个提交携带上一个名为"\_token"的input字段,如示例3-36

{% code-tabs %}
{% code-tabs-item title="Example 3-36. CSRF tokens" %}
```php
<form action="/tasks/5" method="post">
    <?php echo csrf_field();?>
    <!-- or: -->
    <input type="hidden" name="_token" value="<?php echo csrf_token(); ?>">
    <!-- or: -->
    @csrf
</form>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> Laravel 5.6版本中的CSRF辅助函数
>
> @csrf Blade指令在Laravel 5.6版本中不可用,你需要使用csrf\_field\(\)辅助函数代替

在JavaScript应用程序中，需要多做点工作，但不多。对于使用javascript框架的站点来说，最常见的解决方案是将令牌存储在像&lt;meta&gt;这样的标签中。

```
<meta name="csrf-token" content="<?php echo csrf_token(); ?>" id="token">
```

在Javascript框架应用中可以把token放到&lt;meta&gt;标签中可以方便的在请求头中添加token,如例子3-37所示

{% code-tabs %}
{% code-tabs-item title="Example 3-37. Globally binding a header for CSRF" %}
```javascript
// In jQuery:
$.ajaxSetup({
    headers: {
        'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
    }
});

// With Axios:
window.axios.defaults.headers.common['X-CSRF-TOKEN'] =
    document.head.querySelector('meta[name="csrf-token"]');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Laravel将在每次请求时检查X-CSRF-TOKEN，通过的有效令牌将标记CSRF保护

请注意，如果您在Laravel安装中使用默认的Vue引导，则本例中的CSRF的Vue语法是不必要的；它已经为您提供了这一功能。

> 用Vue Resource绑定CSRF 令牌
>
> Laravel 5.4以及更早的项目中,你可能需要使用一个叫做Vue Resource的库来创建Ajax请求,引导CSRF token到Vue Resource有些许不同,具体的请查看Vue Resource文档



