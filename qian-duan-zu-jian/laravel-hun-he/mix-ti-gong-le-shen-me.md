# Mix提供了什么

我已经提到Mix可以使用sass、less和/或postcss预处理您的css。它还可以连接任何类型的文件，压缩它们，重命名它们，并复制它们，还可以复制整个目录或单个文件

此外，Mix可以处理现代风格javascript，并提供自动重新编译、连接和压缩JavaScript。它使browsersync、hmr和版本控制变得容易，并且有许多插件可用于其他常见的构建场景。

混合文档涵盖了所有这些选项以及更多选项，但是我们将在下面的部分中讨论一些特定的用例。

**源代码图**

如果你不熟悉源代码视图，它实际是告知浏览器可以检查你的源代码

默认情况下，mix不会为文件生成源代码图。但是您可以通过在Mix调用之后链式调用sourcemaps\(\)方法来启用它们，如例6-3所示。

{% code-tabs %}
{% code-tabs-item title="Example 6-3. Enabling source maps in Mix" %}
```javascript
let mix = require('laravel-mix');
mix.js('resources/js/app.js', 'public/js').sourceMaps();
```
{% endcode-tabs-item %}
{% endcode-tabs %}

一旦你这么配置了Mix,你可以看到在每个源文件旁生成了一个名为{file‐name}.map的文件

如果没有源代码映射，如果您使用浏览器的开发工具检查特定的CSS规则或JavaScript操作，您将看到一大堆已编译的代码。使用源映射，您的浏览器可以精确定位源文件的确切行，无论是sass、javascript还是其他什么，都可以生成您要检查的规则。

**前置和后置处理器**

我们已经介绍过Sass和Less，Mix也可以处理Sytlus（如示例6-4）你也可以在其他样式后链式调用PostCSS（示例6-5）

{% code-tabs %}
{% code-tabs-item title="Example 6-4. Preprocessing CSS with Stylus" %}
```javascript
mix.stylus('resources/stylus/app.styl', 'public/css');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Example 6-5. Post-processing CSS with PostCSS" %}
```javascript
mix.sass('resources/sass/app.scss', 'public/css')
   .options({
        postCss: [
            require('postcss-css-variables')()
] });
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**预处理CSS**

如果对预处理不了解,有这么一个命令，它会抓取你的CSS文件，链接他们然后输出到public/css目录，有这么几种操作，你可以查看示例6-6

{% code-tabs %}
{% code-tabs-item title="Example 6-6. Combining stylesheets with Mix" %}
```javascript
// Combines all files from resources/css
mix.styles('resources/css', 'public/css/all.css');
// Combines files from resources/css
mix.styles([
    'resources/css/normalize.css',
    'resources/css/app.css'
], 'public/css/all.css');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**链接javascript**

处理javascript与处理css非常类似,可以查看示例6-7

{% code-tabs %}
{% code-tabs-item title="Example 6-7. Combining JavaScript files with Mix" %}
```javascript
let mix = require('laravel-mix');
// Combines all files from resources/js
mix.scripts('resources/js', 'public/js/all.js');
// Combines files from resources/js
mix.scripts([
    'resources/js/normalize.js',
    'resources/js/app.js'
], 'public/js/all.js');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**预处理javascript**

如果你想预处理Javascript-如例子,为将ES6代码编译成纯Javascript-Mix可以使用Webpack很轻松的做到.如示例6-8

{% code-tabs %}
{% code-tabs-item title="Example 6-8. Processing JavaScript files in Mix with Webpack" %}
```javascript
let mix = require('laravel-mix'); 
mix.js('resources/js/app.js', 'public/js');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这段脚本会把resource/js下面的js文件输出到public/js下面。

通过在项目根目录中创建webpack.config.js文件，可以使用webpack更多特性。

**拷贝文件或者目录**

想要拷贝文件或者目录，可以使用copy\(\)或者Directory\(\)方法。

```javascript
 mix.copy('node_modules/pkgname/dist/style.css', 'public/css/pkgname.css');
 mix.copyDirectory('source/images', 'public/images');
```

**版本**

来自Steve Souders的”快速网站响应“已经深入到我们的日常开发中。我们将脚本移到页脚，减少HTTP请求以及更多。但是通常并没有认识到这些想法的起源。

Steve的一个提示仍然很少实现，这就是在资源（脚本，样式和图像）上设置非常长的缓存时间。 这样做意味着您的服务器获取最新版本资源的请求将会减少。 但这也意味着用户极有可能使用的是缓存版本的资源，这是过时的。

解决方案是版本控制。每次运行构建脚本时，在每个资产的文件名中附加一个唯一的哈希，然后该唯一文件将被无限期缓存，或者至少在下一个构建之前缓存。

怎么做？首先，您需要得到生成的哈希并将其附加到文件名中。您还需要更新每个构建的视图，以引用新的文件名。

你可能猜到了，Mix为了做了这些，这是难以置信的简单。有两个组件：Mix的版本控制和mix\(\)php辅助函数。首先，您可以通过运行mix.version（）来对资产进行版本设置，如示例6-9所示。

{% code-tabs %}
{% code-tabs-item title="Example 6-9. mix.version" %}
```javascript
let mix = require('laravel-mix');
mix.sass('resources/sass/app.scss', 'public/css')
    .version();
```
{% endcode-tabs-item %}
{% endcode-tabs %}

生成的文件版本没有什么不同，只是名为app.css并存放在public/css中。

> 使用查询参数进行版本控制
>
> 在Laravel中处理版本控制的方式与传统版本控制略有不同，因为版本控制附加了查询参数，而不是通过修改文件名。 它仍然以相同的方式运行，因为浏览器将其作为“新”文件读取，但它处理一些带有缓存和负载平衡器的边缘情况

接着，在视图内使用PHP mix\(\)辅助函数引用资源，如示例6-10

{% code-tabs %}
{% code-tabs-item title="Example 6-10. Using the mix\(\) helper in views" %}
```markup
<link rel="stylesheet" href="{{ mix("css/app.css") }}">
// Will output something like:
<link rel="stylesheet" href="/css/app.css?id=5ee7141a759a5fb7377a">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
Mix 版本控制的背后

Mix生成名为public/mix-manifest.json的文件,它存储了mix\(\)用于查找的信息,如下是mix-manifest.json示例
{% endhint %}

```javascript
{
"/css/app.css": "/css/app.css?id=4151cf6261b95f07227e"
}
```

**Vue和React**

Mix可以处理Vue和React组件,Mix默认调用Vue,你也可以替换成React

```text
mix.react('resources/js/app.js', 'public/js');
```

如果您查看默认的Laravel示例app.js及其导入的组件（示例6-11），您将看到您不必执行任何特殊操作来使用Vue组件。 在你的app.js中简单调用mix即可

{% code-tabs %}
{% code-tabs-item title="Example 6-11. App.js configured to work with Vue" %}
```javascript
window.Vue = require('vue');
Vue.component('example-component', require('./components/ExampleComponent.vue'));
const app = new Vue({ el: '#app'
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果你想切换到react\(\),你可以在组件内这么写

`require('./components/Example');`

预设还引入了Axios, Lodash, 和 Popper.js，因此你不需要在Vue或者React搭建上浪费时间。

**热模块替换**

如果你正在使用Vue或者React编写组件,你希望在每次构建工具重新编译组件时刷新页面，如果你正在使用Mix，它会调用Browsersync来重载页面。

这很好，但是如果您正在使用单页应用程序（SPA），这意味着您已经重新启动应用程序; 当你浏览应用程序时，刷新会清除你建立的状态。

热模块更换（HMR，有时称为热重新加载）解决了这个问题。设置起来并不总是那么容易，但是Mix是可以实现的。HMR的工作原理就好像你教过browsersync不要重新加载整个文件，而是只重新加载你更改的代码。这意味着您可以将更新后的代码注入到浏览器中，但仍然可以保留之前的状态。

要使用HMR，你需要运行npm run hot，而不是npm run watch，为了保证正常运行，你需要确认&lt;script&gt;引用正确版本的javascript，实际上,Mix会在localhost:8080启动一个Node服务，所以如果你的&lt;script&gt;指向错误版本，HMR将不会运行。

要实现请使用mix\(\)辅助函数引用脚本，辅助函数会帮你处理在localhost:8080下的HMR和正常的开发模式的引用问题。

```markup
<body>
    <div id="app"></div>
    <script src="{{ mix('js/app.js') }}"></script>
</body>
```

如果你在HTTPS上开发你的应用,例如你运行valet secure，你的资源必须通过HTTPS链接，这有些棘手，最好查看[HMR docs](https://github.com/JeffreyWay/laravel-mix/blob/master/docs/hot-module-replacement.md)

**Vendor抽取**

常见的前端模式最终会生成一个CSS文件和一个JavaScript文件，其中包含项目的应用程序特定代码和所有依赖项的代码。

这意味着vendor文件更新需要将整个文件都更新或者缓存,这将浪费加载时间。

Mix可以很容易的从应用依赖中提取依赖成一个vendor.js,只需要在js\(\)后调用extract\(\)方法，如示例6-12

{% code-tabs %}
{% code-tabs-item title="Example 6-12. Extracting a vendor library into a separate file" %}
```javascript
mix.js('resources/js/app.js', 'public/js')
    .extract(['vue'])
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这将输出两个文件:manifest.js用于描述如何加载依赖,vendor.js,它包含特定的vendor代码。

按照循序加载这些文件很重要，首先是manifest.js, 然后是vendor.js, 最后是 app.js

> 在mix 4.0+中使用extract\(\)提取所有依赖项
>
> 如果项目使用的是LaravelMix4.0或更高版本，则可以不带参数调用extract\(\)方法。这将提取应用程序的整个依赖项列表

```markup
<script src="{{ mix('js/manifest.js') }}"></script>
<script src="{{ mix('js/vendor.js') }}"></script>
<script src="{{ mix('js/app.js') }}"></script>
```

**Mix中的环境变量**

如示例6-13,如果你在.env内添加以MIX\_\_开头的环境变量,那么你可以在Mix-compiled文件中通过process.env.ENV\_VAR\_NAME来访问这些变量.

{% code-tabs %}
{% code-tabs-item title="Example 6-13. Using .env variables in Mix-compiled JavaScript" %}
```javascript
# In your .env file
MIX_BUGSNAG_KEY=lj12389g08bq1234
MIX_APP_NAME="Your Best App Now"
// In Mix-compiled files
process.env.MIX_BUGSNAG_KEY // For example, this code:
console.log("Welcome to " + process.env.MIX_APP_NAME); 
// Will compile down to this:
console.log("Welcome to " + "Your Best App Now");
```
{% endcode-tabs-item %}
{% endcode-tabs %}

你也可以使用Node的_dotnev_包在Webpack配置内访问这些变量,如示例6-14

{% code-tabs %}
{% code-tabs-item title="Example 6-14. Using .env variables in Webpack configuration files" %}
```javascript
// webpack.mix.js
let mix = require('laravel-mix'); 
require('dotenv').config();
let isProduction = process.env.MIX_ENV === "production";
```
{% endcode-tabs-item %}
{% endcode-tabs %}

