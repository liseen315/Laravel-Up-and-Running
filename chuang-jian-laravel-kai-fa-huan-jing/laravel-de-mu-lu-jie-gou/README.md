# Laravel的目录结构

当你打开一个包含Laravel程序的目录,你就会看到如下的文件跟目录:

```text
├── app
├── artisan
├── bootstrap
├── composer.json
├── composer.lock
├── config
├── database
├── package.json
├── phpunit.xml
├── public
├── resources
├── routes
├── server.php
├── storage
├── tests
├── vendor
├── webpack.mix.js
```

{% hint style="info" %}
与5.4版本的不同

用5.4版本Laravel创建的项目,你会发现有个叫gulpfile.js的文件而却找不到webpack.mix.js文件,这说明项目是以Laravel Elixir运行的而不是Laravel Mix
{% endhint %}

让我们逐一的熟悉一下这些目录.

