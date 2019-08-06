# Laravel Valet

如果你使用的是PHP的内置web server,你可以通过本地URL进行简单的操作即可为站点提供服务,如果你在你的Laravel站点根目录运行 `php -S localhost:8000 -t public` PHP内置的web serve 将会在`http://localhost:8000`运行你的站点,同样你也可以运行`php artisan serve` 轻松的启动等效的服务器

如果你有兴趣为你的每个站点指定特殊的开发域名,你需要熟悉你机器系统的host文件,以及使用类似dnsmasq这类的工具,我们还是换个简单点的吧.

如果你是mac用户,Laravel无需将你的域名链接到你的应用文件夹,Valet安装了dnsmasq和一系列的PHP脚本,输入`laravel new myapp && open myapp.test`就可以正常工作了,使用Homestead的话需要安装一些工具,跟着文档就能安装好,而且安装到启动就几步也很简单.

安装Valet-查看最新的安装说明[文档](https://laravel.com/docs/5.8/valet),并将其指向你站点的一个或者多个目录,我的运行目录是~/Sites,在此目录下我放置了我的所有待开发应用,现在你只需要在目录文件名后面添加.test并在浏览器中访问此名字即可.  


Valet可以使用Valet park 轻松地将给定文件夹中的所有文件夹作为{foldername} .test来访问.使用`valet link`为单个文件夹提供服务,使用`valet open`为文件夹设置域名,使用`valet secure`为站点提供HTTPS,使用`valet share`来共享站点



