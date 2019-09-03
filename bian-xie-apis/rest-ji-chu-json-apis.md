# REST基础-JSON APIs

Representational State Transfer（REST）是一种用于构建API的架构风格。 从技术上讲，REST要么是广泛的定义，可以应用于几乎整个互联网，要么是特定的，没有人真正使用它，所以不要让你自己被定义所淹没，或者陷入与学者的争论中 。 当我们在Laravel世界中讨论RESTful或类似REST的API时，我们通常会讨论具有一些共同特征的API。

* 他们围绕"资源"构建，这些资源用URIs来表示，例如/cats表示所有的猫,/cats/15表示ID15的猫
* 与资源交互只要用HTTP verbs\(GET /cats/15 与DELETE /cats/15\)
* 它们是无状态的，这意味着请求之间不存在持久的会话身份验证；每个请求必须唯一地对自身进行身份验证。
* 它们是可缓存且一致的，这意味着每个请求（除少数经过身份验证的用户特定请求之外）都应返回相同的结果，无论请求者是谁。
* 他们返回JSON

最常见的API模式是为每个作为API资源公开的Eloquent模型提供唯一的URL结构，并允许用户使用特定verb与该资源进行交互并获取JSON。 例13-1给出了几个可能的例子。

{% code-tabs %}
{% code-tabs-item title="Example 13-1. Common REST API endpoint structures" %}
```php
GET /api/cats
[
    {
        id: 1,
        name: 'Fluffy'
    },
    {
        id: 2,
        name: 'Killer'
    }
]
GET /api/cats/2
{
    id: 2,
    name: 'Killer'
}
DELETE /api/cats/2
(deletes cat)
POST /api/cats with body:
{
    name: 'Mr Bigglesworth'
}
(creates new cat)
PATCH /api/cats/3 with body:
{
    name: 'Mr. Bigglesworth'
}
(updates cat)
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这让你了解我们可能与API有关的基本交互。 让我们深入了解如何使用Laravel实现它们

