# 使用Passport API和vue组件管理客户端和令牌

现在我们介绍了如何手动创建客户端和令牌，以及如何作为使用者进行授权，让我们看看Passport API一些方面，它可以构建用户界面元素从而允许用户管理他们的客户端和令牌。

**路由** 

深入研究API路由的最简单方法是查看示例提供的Vue组件是如何工作的以及它们依赖哪些路由，因此我将简要概述一下：

```text
/oauth/clients (GET, POST)
/oauth/clients/{id} (DELETE, PUT)
/oauth/personal-access-tokens (GET, POST)
/oauth/personal-access-tokens/{id} (DELETE)
/oauth/scopes (GET)
/oauth/tokens (GET)
/oauth/tokens/{id} (DELETE)
```

如您所见，我们在这里有几个实体（客户端，个人访问令牌，作用域和令牌）。 我们列出了所有这些; 我们可以创建（你不能创建作用域，因为它们是在代码中定义的，你不能创建令牌，因为它们是在授权流程中创建的）; 我们也可以删除和更新（PUT）。

**Vue 组件** 

Passport 附带了一组Vue组件使得用户可以方便的管理他们的客户端\(他们创建的\)，授权客户端（允许访问他们的账户）以及个人访问令牌\(用于他们自己的测试目的\)。 要将这些组件发布到应用程序中，请运行以下命令

```bash
php artisan vendor:publish --tag=passport-components
```

现在你在resources/js/components/passport有3个Vue 组件，添加他们到Vue 引导以便在模板中可以访问他们，可以注册到resources/js/app.js文件中，如示例13-36

```javascript
require('./bootstrap');
Vue.component(
    'passport-clients',
    require('./components/passport/Clients.vue')
);
Vue.component(
    'passport-authorized-clients',
    require('./components/passport/AuthorizedClients.vue')
);
Vue.component(
    'passport-personal-access-tokens',
    require('./components/passport/PersonalAccessTokens.vue')
);
const app = new Vue({ el: '#app'});
```

现在你获取了一个可以在应用任何地方都可用的三个组件

```markup
<passport-clients></passport-clients>
<passport-authorized-clients></passport-authorized-clients>
<passport-personal-access-tokens></passport-personal-access-tokens>
```

