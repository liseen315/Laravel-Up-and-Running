# 其他重定向方法

redirect服务提供了其他不太常用的方法,但是仍然有效:

_home\(\)_

重定向到名为home的路由。`refresh()::`重定向到用户当前所在的页面。`away()::`允许在不进行默认URL验证的情况下重定向到外部URL

_secure\(\)_

就像to\(\)携带一个为”true“的安全参数一样

_action\(\)_

允许您通过以下两种方式之一链接到控制器和方法：作为字符串\(redirect\(\)-&gt;action\('mycontroller@mymethod'\)\)或作为元组\(redirect\(\)-&gt;action\(\[mycontroller::class，'mymethod'\]\)。

_guest\(\)_

用于内部认证系统（在第9章中讨论）; 当用户访问他们未经过身份验证的路由时，会捕获“预期”路由，然后重定向用户（通常会重定向到登录页面）

intended\(\)

也由auth系统内部使用；在成功进行身份验证之后，这将获取guest\(\)方法存储的“预期”URL，并将用户重定向到该URL。

