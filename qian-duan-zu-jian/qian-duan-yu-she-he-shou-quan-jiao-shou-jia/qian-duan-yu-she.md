# 前端预设

通过查看package.json、webpack.mix.js（或laravel旧版本中的gulpfile.js）以及resources目录中的视图、javascript文件和css文件，您可以了解每个新laravel安装时附带的前端工具。这些默认的组件和文件集被称为Vue预设，并且每个新的laravel项目都带这些预设。

但是，如果你更愿意使用React呢，如果你想用Bootstrap而非全部用Javascript,或者你想要抛弃全部，Laravel 5.5的前端预设脚本用于修改或删除默认预设，你可以使用现成的预设，也可以从Github获取第三方预设。

使用内置预设，只需要运行 php artisan present preset\_name

```bash
php artisan preset react
php artisan preset bootstrap
php artisan preset none
```

vue预设是每个新安装的程序默认的预设。

**第三方预设**

如果您有兴趣创建自己的预设，或者使用由其他社区成员创建的预设，那么前端预设系统也可以这样做。有一个Github组织，旨在使它很容易找到伟大的第三方前端预设，他们很容易安装。大多数情况下，步骤如下

1. 安装包（例如,composer require laravel-frontend-presets/tail windcss）
2. 安装预设 （php artisan preset tailwindcss）
3. 就像内置预设 运行npm install 和 npm run dev

如果你想创建自己的预设,你可以fork这个[仓库](https://github.com/laravel-frontend-presets/skeleton)

授权脚手架

虽然它们在技术上不是前端预设的一部分，但Laravel有一系列称为auth脚手架的路由和视图，本质还是前端预设。 如果您运行php artisan make:auth，您将获得一个登录页面，一个注册页面，一个用于"app"的模板等等。 请参阅第9章以了解更多信息



