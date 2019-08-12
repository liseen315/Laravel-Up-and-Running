# 输出数据

正如例子4-1所示的,{{and}}用于包装"echo"，{{$variable}}类似于&lt;?=$varible?&gt;这种纯PHP语法

不过有一点不同,可能你已经猜到了,Blade使用PHP的htmlentities\(\)来避免用户插入恶意脚本,这意味着,{{$variable}}等同于&lt;?= htmlentites\($variable\)?&gt;,如果你不想转义,请使用{!! and !!}替代.

> 前端框架的{{and}}
>
> 你可能注意到Laravel的Blade模板语法与许多前端框架语法类似,那么Laravel是怎么知道使用的Blade还是Handlebars呢?
>
> Blade会忽略{{前面的@,所以下面的第一个例子会进行解析，而第二个就会直接输出了.

```text
// Parsed as Blade; the value of $bladeVariable is echoed to the view
{{ $bladeVariable }}
// @ is removed and "{{ handlebarsVariable }}" echoed to the view directly
@{{ handlebarsVariable }}
```

> 你可以使用@verbatim指令包装大部分的脚本内容.

