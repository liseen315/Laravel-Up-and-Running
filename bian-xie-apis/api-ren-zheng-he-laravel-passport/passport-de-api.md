# Passport的API

Passport在应用程序中以/oauth路由前缀公开api。API提供两个主要功能：第一，使用oauth 2.0授权流（/oauth/authorize和/oauth/token）授权用户；第二，允许用户管理其客户端和令牌（其余路由）。

有一个重要区别，如果你对OAuth不熟悉，我们的OAuth服务需要为用户公开认证的能力，但是Passport还公开了一个API用于管理你的OAuth服务器的客户端以及token，这意味着你你可以轻松的构建一个前端，用于你的用户管理他们的信息，以及Passport实际上集成了Vue管理组件，你可以使用这些组件或者用于灵感。

我们介绍了API路由用于管理客户端和token，Vue组件和Passport使之变的容易。但首先让我们深入了解用户使用受Passport保护的API进行身份验证的各种方式。



