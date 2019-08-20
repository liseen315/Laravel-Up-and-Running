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

