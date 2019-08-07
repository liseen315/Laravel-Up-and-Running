# 其他零散文件

根目录包含如下文件.

_.editorconfig_

为你的IDE/文本编辑器提供Laravel编码标准说明\(例如缩进大小、字符集以及是否修剪尾随空格\),你将会在5.5版本以及以后的Laravel应用内看到此文件

._env和.env.example_

环境变量配置\(不用的环境变量配置也不尽相同,因此不受版本控制\),.env.example是一个模板应该复制一份来覆盖掉.env文件,并且它也是被git忽略的

_.gitignore和gitattributes_

Git配置文件

_artisan_

用于运行Artisan命令更多细节查看第8章节

_package.json_

类似composer.json但是是用于前端构建系统来管理前端资源以及依赖的.它标识NPM引入基于javascript的依赖项

_phpunit.xml_

用于PHP单元测试,是Laravel开箱测试工具

_readme.md_

介绍Laravel的Markdown文件,如果你用Laravel安装器安装的那么你将看不到此文件

_server.php_

允许功能较少的服务器仍然可以预览Laravel应用程序的备份服务器

webpack.mix.js

用于Mix的可选配置文件,如果你使用的Elixir,你将会看到gulpfile.js来替代此文件,这个文件用于编译和处理前端资源

