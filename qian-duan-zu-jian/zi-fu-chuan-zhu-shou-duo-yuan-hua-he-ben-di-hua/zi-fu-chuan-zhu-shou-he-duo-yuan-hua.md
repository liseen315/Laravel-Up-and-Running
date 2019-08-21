# 字符串助手和多元化

Laravel有一系列用于处理字符串的帮助程序。它们作为Str类的方法,例如\(Str::plural\(\)\)，还有全局辅助函数\(str\_plural\(\)\)。

[Laravel文档](https://laravel.com/docs/5.8/helpers)说明了这些函数,这里有几个常用的函数说明:

_en\(\)_

html\_entities\(\)的快捷方式,用于安全encodes HTML

_starts\_with\(\), ends\_with\(\), str\_contains\(\)_

检查一个字符串\(第一个参数\)检查是否以另外一个字符串开始,结束或者包含\(第二个参数\)

_str\_is\(\)_

正则匹配字符串，例如foo\* 将匹配 foobar 和 foobaz.

_str\_slug\(\)_

将字符串转换成带连接符的URL

_str\_plural\(word, count\), str\_singular\(\)_

返回复数或者单数

_camel\_case\(\), kebab\_case\(\), snake\_case\(\), studly\_case\(\), title\_case\(\)_

转换大小写

_str\_after\(\), str\_before\(\), str\_limit\(\)_

这些方法用于修剪字符串然后返回子字符串  
  
  


