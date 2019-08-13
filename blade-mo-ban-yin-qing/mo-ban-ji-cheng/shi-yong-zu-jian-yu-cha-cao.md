# 使用组件与插槽

Laravel提供了另一种在视图之间包含内容的模式:组件与插槽,当把大量的内容作为变量传递给视图时,你会发现使用组件将非常有意义,请看示例4-14中的模态弹窗,该示例用于在用户接到响应时提醒用户错误

{% code-tabs %}
{% code-tabs-item title="Example 4-14. A modal as an awkward view partial" %}
```php
<!-- resources/views/partials/modal.blade.php -->
<div class="modal">
<div>{{ $content }}</div>
<div class="close button etc">...</div>
</div>

<!-- in another template -->
@include('partials.modal', [
'body' => '<p>The password you have provided is not valid. Here are the rules for valid passwords: [...]</p>
<p><a href="#">...</a></p>'
])
```
{% endcode-tabs-item %}
{% endcode-tabs %}

要传递的变量太多了,因此组件更适合这种情况.

带插槽的组件被设计为一个大块的"slot"用于从包含的模板中获取内容,查看示例4-15，了解如何使用组件和插槽重构示例4-14

{% code-tabs %}
{% code-tabs-item title="Example 4-15. A modal as a more appropriate component with slots" %}
```php
<!-- resources/views/partials/modal.blade.php -->
<div class="modal">
    <div>{{ $slot }}</div>
    <div class="close button etc">...</div>
</div>

<!-- in another template -->
@component('partials.modal')
<p>The password you have provided is not valid. Here are the rules for valid passwords: [...]</p>
<p><a href="#">...</a></p> 
@endcomponent
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如示例4-15所示,@component指令可以让我们将HTML内容回填到插槽内.模板内的$slot用于接收从@component传递的任何内容.

_**多插槽**_

 我们在示例4-15中使用的方法称为“默认”槽；无论您在@component和@endcomponent之间传递什么，都会传递给$slot变量。但是你也可以设定多个槽位。让我们想象一个带有标题的模态窗，如示例4-16中所示

{% code-tabs %}
{% code-tabs-item title="Example 4-16. A modal view partial with two variables" %}
```php
<!-- resources/views/partials/modal.blade.php -->
<div class="modal">
    <div class="modal-header">{{ $title }}</div> 
    <div>{{ $slot }}</div>
    <div class="close button etc">...</div>
</div>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

然后你可以在@component内调用@slot指令传递相应的内容,如示例4-17所示

{% code-tabs %}
{% code-tabs-item title="Example 4-17. Passing more than one slot to a component" %}
```text
@component('partials.modal')
    @slot('title')
        Password validation failure
    @endslot
    <p>The password you have provided is not valid. Here are the rules for valid passwords: [...]</p>
    <p><a href="#">...</a></p> 
@endcomponent
```
{% endcode-tabs-item %}
{% endcode-tabs %}

然后如果你还有其他不想作为插槽传递的变量,你依然可以将变量作为数组传递给@component的第二个变量,就像使用@include一样,如示例4-18

{% code-tabs %}
{% code-tabs-item title="Example 4-18. Passing data to a component without slots" %}
```php
@component('partials.modal', ['class' => 'danger'])
    ...
@endcomponent
```
{% endcode-tabs-item %}
{% endcode-tabs %}

_**别名组件为指令**_

有个小技巧能让组件更易用,这称为别名,简单来说是在Blade facade上调用Balde::component\(\)-它通常位于AppserviceProvider的boot\(\)方法内,然后第一个参数传递组件路径,第个参数传入指令名称,如示例4-19

{% code-tabs %}
{% code-tabs-item title="Example 4-19. Aliasing a component to be a directive" %}
```text
// AppServiceProvider@boot
Blade::component('partials.modal', 'modal');

<!-- in a template -->
@modal
    Modal content here
@endmodal
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> 导入Facades
>
> 这是我们第一次在一个命名空间类下使用facade.我们将在后面详细介绍它,但要了解,我们在命名空间类下使用facade,它是Laravel近期版本内的类,你可能会被提示facade没被找到，这是因为facade只是具有普通名称空间的普通类，但是Laravel做了一些小技巧，使它们可以从根名称空间中使用
>
> 所以示例4-19我们需要在文件头导入Illuminate\Support \Facades\Blade facade

