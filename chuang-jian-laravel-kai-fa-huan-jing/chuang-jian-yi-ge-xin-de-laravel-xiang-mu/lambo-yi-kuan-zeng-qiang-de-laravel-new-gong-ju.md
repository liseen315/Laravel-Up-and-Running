# Lambo:一款增强的"Laravel New"工具

因为我经常创建新的Laravel项目,而项目创建都是同样的步骤,所以我制作了一个名为Lambo的简单脚本,它可以在每次创建新项目时自动执行这些步骤.

Lambo 运行 laravel new 然后提交代码到Git,给.env配置合理的默认值.在浏览器中打开项目,可选择是否在编辑器中打开代码,并且执行一些有用的构建步骤.

你可以使用Composer的全局命令安装Lambo:

```text
composer global require tightenco/lambo
```

然后你就可以像Laravel new一样使用了:

```text
cd Sites
lambo my-new-project
```

