# Blade检查

Blade还有一个小的便利助手：@can指令。示例9-23说明了它的用法。

{% code-tabs %}
{% code-tabs-item title="Example 9-23. Using Blade’s @can directive" %}
```markup
<nav>
    <a href="/">Home</a> 
    @can('edit-contact', $contact)
    <a href="{{ route('contacts.edit', [$contact->id]) }}">Edit This Contact</a> 
    @endcan
</nav>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

你可以在@can和@endcan之间使用@else,如示例9-24你也可以使用@cannot和@endcannot.

{% code-tabs %}
{% code-tabs-item title="Example 9-24. Using Blade’s @cannot directive" %}
```markup
<h1>{{ $contact->name }}</h1> 
@cannot('edit-contact', $contact)
    LOCKED
@endcannot
```
{% endcode-tabs-item %}
{% endcode-tabs %}

