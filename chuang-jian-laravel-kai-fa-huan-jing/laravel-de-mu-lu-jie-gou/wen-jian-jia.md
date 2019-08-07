# 文件夹

默认情况下根目录包含如下文件夹:

_app_

存放应用程序核心代码,比如,模型,控制器,命令,以及你的PHP domain code 都将存放在这个目录下

_bootstrap_

包含Laravel框架每次启动的引导文件

_config_

_配置文件都会存放在这个目录_

_database_

存放数据迁移,数据填充,以及模型工厂文件

_public_

服务器为网站提供服务时指向的目录.其中包含了index.php文件,它用于引导程序以及路由所有请求的前置控制器,同时也包含一些对外资源文件像图片,样式,脚本,以及下载等

_resources_

其他脚本文件存放的目录,视图,语言文件,以及\(可选\)Sass/Less/css源文件,javascript源文件等

_routes_

路由定义存放于此,像HTTP路由以及"控制台路由"或者是Artisan命令

_storage_

存放缓存,日志,以及系统编译文件

_tests_

存放单元测试以及集成测试

_vendor_

Composer安装的依赖都在这,这个文件夹会被git版本忽略掉,Composer应该作为你远程部署的一部分

