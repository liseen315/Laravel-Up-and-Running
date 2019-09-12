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

&lt;passport-clients&gt;显示用户创建的所有客户端。这意味着当他们登录到tweeter时，Spacebook的创建者将看到这里列出的Spacebook客户端。

&lt;passport-authorized-clients&gt;展示授权访问账号的所有客户端，这意味着，任何同时拥有SpaceBook和Tweeeter账户的用户都可以看到列出的SpaceBook。

&lt;passport-personal-access-tokens&gt;显示用户创建的个人访问令牌，例如RaceBook的创建者，SpaceBook的竞争者，将看到他们用来测试Tweeter API的个人访问令牌。

如果您正在安装新的laravel，并想测试这些，有如下几个步骤：

1. 按照本章前面的说明安装Passport
2. 在你的命令行执行如下命令

```text
php artisan vendor:publish --tag=passport-components
npm install
npm run dev
php artisan make:auth
```

   3. 打开resources/views/home.blade.php，在&lt;div class="card-body"&gt; 下面添加vue引用（例如&lt;passport-clients&gt;&lt;/passport-clients&gt;）。

如果你愿意的话也可以直接使用这些组件，你也可以把他们作为引用点来理解API使用，并以你喜欢的格式创建前端组件。



