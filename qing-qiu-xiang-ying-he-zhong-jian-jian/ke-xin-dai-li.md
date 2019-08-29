# 可信代理

如果您使用Laravel工具在应用程序中生成URL，您会注意到Laravel检测当前请求是通过HTTP还是HTTPS，并使用适当的协议生成链接。

但是，当您在应用程序前面有代理（例如，负载均衡器或其他基于Web的代理）时，这并不总是有效。 许多代理将非标准头文件（如X\_FORWARDED\_PORT和X\_FORWARDED\_PROTO）发送到您的应用程序，并期望您的应用程序“信任”它们，解释它们，并将它们用作解释HTTP请求的过程的一部分。 为了使Laravel正确处理代理的HTTPS调用（如安全调用），并且为了让Laravel处理来自代理请求的其他头，您需要定义它应该如何执行。

您可能不只是想允许代理向您的应用发送流量; 相反，您希望将您的应用锁定为仅信任某些代理，甚至从这些代理中您可能只想信任某些转发的代码。

从Laravel 5.6开始，每次安装Laravel时，默认情况下包含TrustedProxy包，  但如果你使用的是旧版本，你仍需要将它放入你的包中。 TrustedProxy使您可以将某些流量来源列入白名单并将其标记为“受信任”，并标记您希望从这些来源信任的转发标头以及如何将它们映射到普通头。

要配置应用程序可信任的代理，可以编辑App\Http \Middleware\TrustProxies中间件，并将负载均衡器或代理的IP地址添加到$proxies数组，如例10-25所示

{% code-tabs %}
{% code-tabs-item title="Example 10-25. Configuring the TrustProxies middleware" %}
```php
 /**
 * The trusted proxies for this application
 *
 * @var array
 */
protected $proxies = [ '192.168.1.1', '192.168.1.2'];
/**
 * The headers that should be used to detect proxies
 *
 * @var string
 */
protected $headers = Request::HEADER_X_FORWARDED_ALL;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如您所见，$ headers数组默认信任来自受信任代理的所有转发头; 如果要自定义此列表，请查看信任代理上的[Symfony文档](https://symfony.com/doc/current/deployment/proxies.html)

