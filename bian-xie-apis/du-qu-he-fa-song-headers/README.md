# 读取和发送Headers

REST API通常使用头读取和发送非内容信息。例如，对GitHub的API的任何请求都将返回详细描述当前用户速率限制状态的头。

```text
X-RateLimit-Limit: 5000
X-RateLimit-Remaining: 4987
X-RateLimit-Reset: 1350085394
```

**X-\* Headers**

您可能想知道为什么Github速率限制头的前缀是x-，特别是当您在同一请求返回的其他头的上下文中看到它们时。

```text
HTTP/1.1 200 OK
Server: nginx
Date: Fri, 12 Oct 2012 23:33:14 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Status: 200 OK
ETag: "a00049ba79152d03380c34652f2cb612"
X-GitHub-Media-Type: github.v3
X-RateLimit-Limit: 5000
X-RateLimit-Remaining: 4987
X-RateLimit-Reset: 1350085394
Content-Length: 5
Cache-Control: max-age=0, private, must-revalidate
X-Content-Type-Options: nosniff
```

> 以X开头的头并不在HTTP标准中，它可能是完全捏造的\(例如，X-How-Much-Matt-Loves-This-Page\)，或者尚未成为规范的一部分\(例如，X-Requested-With\)。

同样需要API允许开发人员使用头自定义请求，例如，GitHub的API可以很容易地定义你要使用Accept标头的API的哪个版本。

```text
Accept: application/vnd.github.v3+json
```

如果你想更改v3到v2，Github将传递你的请求到第二版本。

让我们学习如何在Laravel中快速的实现这两种

