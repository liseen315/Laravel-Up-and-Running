# 条件

首先让我们看下逻辑控制结构

_**@if**_

Blades的 @if\(条件\)会被编译成&lt;?php if \($condition\) : ?&gt;。@else,@elseif,以及@endif 也会被编译成PHP语法结构样式,如示例4-2所示

{% code-tabs %}
{% code-tabs-item title="Example 4-2. @if, @else, @elseif, and @endif" %}
```php
@if (count($talks) === 1)
There is one talk at this time period.
@elseif (count($talks) === 0)
There are no talks at this time period.
@else
There are {{ count($talks) }} talks at this time period.
@endif
```
{% endcode-tabs-item %}
{% endcode-tabs %}

就像原生的PHP条件判断一样,你可以混合跟匹配你想要的.它们没有任何特殊的逻辑,实际上，解析器会查找@if（$ condition）这样的东西，并用适当的PHP代码替换它

_**@unless 和 @endunless**_

@unless是一种新的语法,在PHP中没有直接的等价语法对应,它是@if的反转,@unless\(@condition\)与&lt;?php if\(!$condition\)&gt;相同,你可以在示例4-3中查看

{% code-tabs %}
{% code-tabs-item title="Example 4-3. @unless and @endunless" %}
```php
@unless ($user->hasPaid())
    You can complete your payment by switching to the payment tab.
@endunless
```
{% endcode-tabs-item %}
{% endcode-tabs %}



