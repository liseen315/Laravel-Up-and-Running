# Laravel 混合

Mix是一个构建工具，它在Webpack上提供简单的用户界面和一系列约定。 Mix的核心价值是通过更清晰的API和一系列命名和应用程序结构约定来简化最常见的构建和编译Webpack任务

> Webpack简介
>
>  Webpack是一个专门为编译静态资源而设计的javascript工具；Webpack团队将其目的描述为将“具有依赖关系的模块”捆绑在一起并生成“静态资源”。
>
> Webpack类似于Gulp或Grunt，与Webpack一样，这些工具通常用于处理和绑定网站的依赖项。运行一个CSS预处理器，比如sass或更少或postss，复制文件，链接和压缩javascript。
>
> 与其他不同，Webpack专注于将模块与依赖项捆绑在一起，从而生成静态资源。 Gulp和Grunt是任务运行者，与之前的Make和Rake一样，可用于自动执行任何可编程和可重复的活动。 它们都可以用来捆绑资产，但这不是它们的核心关注点，他们将复杂资源绑定输出 - 例如，确定哪些生成的资产不会被使用， 将它们从最终输出中丢弃

核心是,Mix是Webpack工具箱的一个工具,"Mix file"是一个简单的Webpack配置文件位于项目根目录，名字为webpack.mix.js，但是它的配置比其他的Webpack配置简单的多，你只需要极少的工作就可以编译资源了。

现在让我们看个例子,运行Sass预处理CSS文件，在正常的Webpack环境中,它的配置可能类似示例6-1

```javascript
var path = require('path');
var MiniCssExtractPlugin = require("mini-css-extract-plugin");
module.exports = {
    entry: './src/sass/app.scss',
    module: {
        rules: [{
            test: /\.s[ac]ss$/,
            use: [
                MiniCssExtractPlugin.loader,
                "css-loader",
                "sass-loader"
            ]
        }
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({
            path: path.resolve(__dirname, './dist'),
            filename: 'app.css'
        })
    ]
}
```

这看起来有些糟糕，属性配置还不算多，结构也算清晰，这是从一个项目复制过来的代码，但是这种代码写起来不舒服，像这样的工作会让人感觉到困惑。

让我们看Mix内同样的任务\(示例6-2\)

{% code-tabs %}
{% code-tabs-item title="Example 6-2. Compiling a Sass file in Mix" %}
```javascript
let mix = require('laravel-mix');
mix.sass('resources/sass/app.scss', 'public/css');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

就这样。它不仅非常简单，还包括文件监视、浏览器同步、通知、指定的文件夹结构、自动重新编译、URL处理等等。

