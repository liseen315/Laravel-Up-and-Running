# 循环

接下来让我们看下循环结构.

_**@for,@foreach和@while**_

Blade中的@for,@foreach以及@while与它们在PHP中作用相同,查看示例4-4,4-5,以及4-6

{% code-tabs %}
{% code-tabs-item title="Example 4-4. @for and @endfor" %}
```php
@for ($i = 0; $i < $talk->slotsCount(); $i++) 
    The number is {{ $i }}
@endfor
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Example 4-5. @foreach and @endforeach" %}
```php
@foreach ($talks as $talk)
    {{ $talk->title }} ({{ $talk->length }} minutes)<br>
@endforeach
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Example 4-6. @while and @endwhile" %}
```php
@while ($item = array_pop($items)) 
    {{ $item->orSomething() }}
@endwhile
```
{% endcode-tabs-item %}
{% endcode-tabs %}

@forelse和@endforelse

如果你的迭代对象内有空值,@forelse允许你进行后续的操作,我们在本章开始时候看到过类似的例子,现在让我们看另外一个例子4-7

{% code-tabs %}
{% code-tabs-item title="Example 4-7. @forelse" %}
```php
@forelse ($talks as $talk)
    {{ $talk->title }} ({{ $talk->length }} minutes)<br>
@empty
    No talks this day.
@endforelse
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> @foreach与@forelse中的$loop
>
> @foreach与@forelse指令是Laravel 5.3引入的一个特性:$loop,它在原生PHP循环中并不存在这个特性,当在@foreach或@forelse循环中使用时，此变量将返回具有这些属性的stdclass对象
>
> index\(索引\)
>
> 循环中基于0的索引,0代表第一个元素.
>
> iteration\(迭代\)
>
> 循环中基于1的索引,1代表第一个元素
>
> remaining\(剩余\)
>
> 循环中还剩余多少元素
>
> count\(计数\)
>
> 循环长度
>
> first\(第一\) 
>
> 一个布尔值，指示这是否是循环中的第一个项目 
>
> last\(最终\) 一个布尔值，指示这是否是循环中的最后一项
>
> depth\(深度\)
>
>  这个循环深度有多少“级别”：1表示循环，2表示循环内的循环等。 
>
> parent\(父级\) 
>
> 如果此循环位于另一个@foreach循环内，则对父循环项的$ loop变量的引用; 否则，null
>
> 如下例子展示如何使用它

```php
<ul>
    @foreach ($pages as $page)
        <li>
        {{ $loop->iteration }}: {{ $page->title }} 
            @if ($page->hasChildren())
            <ul>
                @foreach ($page->children() as $child)
                    <li>{{ $loop->parent->iteration }}
                        {{ $loop->iteration }}:
                        {{ $child->title }}
                    </li>
                @endforeach
            </ul>
            @endif
        </li>
    @endforeach
</ul>
```

