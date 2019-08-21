# 本地化

本地化允许您定义多种语言并将任何字符串作为目标进行翻译。 您可以设置回退语言，甚至可以处理多元变化。

在Laravel中，您需要在页面加载期间的某个时间点设置一个“application locale”，以便本地化助手知道要从哪个翻译桶中提取。每个”locale“通常与翻译连接，就像“en” \(for English\)，你可以通过App::setLocale\($localeName\)完成操作，并将其放到服务提供者中，现在你可以把它放到AppServiceProvider的boot中，但如果最终使用的不仅仅是与区域设置相关的绑定，则需要创建一个LocaleServiceProvider。

> 为每个请求设置本地化
>
> 首先，弄清楚Laravel是如何“了解”用户的语言环境的，或者提供翻译的，可能会让人困惑。大部分的工作都取决于您作为开发人员的能力。让我们来看一个可能的场景。
>
> 您可能拥有一些功能，允许用户选择一个区域设置，或者可能尝试自动检测它。不管怎样，应用程序都将确定区域设置，然后将其存储在URL参数或会话cookie中。然后，您的服务提供商（类似于LocaleServiceProvider）可能会获取该密钥并将其设置为Laravel引导程序的一部分。
>
> 因此，您的用户可能位于[http://myapp.com/es/contacts](http://myapp.com/es/contacts) 您的LocaleServiceProvider将获取该es字符串，然后运行app:：setLocale（“es”）。接下来，每当你要求翻译一个字符串时，Laravel都会寻找该字符串的西班牙语版本（es表示espan\_ol），你需要在某个地方定义它

你可以在config/app.php定义回退，然后你会发现一个fallback\_locale键，这允许您为应用程序定义默认语言，如果Laravel找不到所请求区域设置的翻译，它将使用该语言

**基础本地化**

所以我们如何翻译字符串?有一个\_\_\($key\)辅助函数，它将为所传递的键提取当前区域设置的字符串，或者，如果该字符串不存在，则从默认区域设置中获取该字符串。在blade中，还可以使用@lang\(\)指令。示例6-20演示了基本翻译的工作原理。我们将使用详情页面顶部的“返回仪表板”链接作为示例。

{% code-tabs %}
{% code-tabs-item title="Example 6-20. Basic use of \_\_\(\)" %}
```php
// Normal PHP
<?php echo __('navigation.back'); ?> // Blade
{{ __('navigation.back') }}
// Blade directive
@lang('navigation.back')
```
{% endcode-tabs-item %}
{% endcode-tabs %}

假设我们现在使用的是ES语言环境。Laravel将在resources/lang/es/navigation.php中查找一个文件，并期望返回一个数组。它将在该数组上查找back键，如果它存在，它将返回其值。查看示例6-21中的示例。

{% code-tabs %}
{% code-tabs-item title="Example 6-21. Using a translation" %}
```php
// resources/lang/es/navigation.php
return [
    'back' => 'Volver al panel',
];
// routes/web.php
Route::get('/es/contacts/show/{id}', function () {
    // Setting it manually, for this example, instead of in a service provider App::setLocale('es');
    return view('contacts.show');
});
// resources/views/contacts/show.blade.php
<a href="/contacts">{{ __('navigation.back') }}</a>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> Laravel 5.4之前版本的翻译辅助函数
>
> 在运行5.4之前版本的Laravel的项目中，\_\_\(\)助手不可用。相反，您必须使用trans\(\)助手，它使用旧的翻译系统，该系统的工作原理与我们在这里描述的类似，但不能访问JSON翻译。

**本地化中的参数**

前面的例子相对简单。让我们深入研究一些更复杂的问题。如果我们想定义返回到哪个仪表盘呢？请看示例6-22

{% code-tabs %}
{% code-tabs-item title="Example 6-22. Parameters in translations" %}
```php
// resources/lang/en/navigation.php
return [
    'back' => 'Back to :section dashboard',
];
// resources/views/contacts/show.blade.php
{{ __('navigation.back', ['section' => 'contacts']) }}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

正如你看到的，在单词前面添加冒号\(:section\)用于占位，\_\_\(\)第二个参数用于替换占位符

**本地化的多元化**

我们已经讨论了多元化，所以现在假设你正在定义你自己的多元化规则。有两种方法可以做到这一点；我们将从最简单的方法开始，如示例6-23所示。

{% code-tabs %}
{% code-tabs-item title="Example 6-23. Defining a simple translation with an option for pluralization" %}
```php
// resources/lang/en/messages.php
return [
    'task-deletion' => 'You have deleted a task|You have successfully deleted tasks',
]

// resources/views/dashboard.blade.php
@if ($numTasksDeleted > 0)
{{ trans_choice('messages.task-deletion', $numTasksDeleted) }}
@endif
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如您所见，我们有一个trans\_choice\(\)方法，它第二个参数接收计数；由此决定使用哪段翻译。

您还可以使用与Symfony更复杂的翻译组件兼容的任何翻译定义，如示例6-24

{% code-tabs %}
{% code-tabs-item title="Example 6-24. Using the Symfony’s Translation component" %}
```php
// resources/lang/es/messages.php
return [
        'task-deletion' => "{0} You didn't manage to delete any tasks.|" .
        "[1,4] You deleted a few tasks.|" .
        "[5,Inf] You deleted a whole ton of tasks.",
];
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**使用JSON存储默认字符串作为key**

翻译系统一个难点是没有好的系统来定义key的命名空间，例如一个key被嵌套了三四层，或者哪个key被用了两次。

slug键/字符串值对系统的替代方法是使用主要语言字符串作为键来存储您的翻译，而不是使用虚构的slug。 您可以通过将翻译文件作为JSON存储在resources/ lang目录中，向Laravel表明您正在以这种方式工作，文件名反映了区域设置，如示例6-25

{% code-tabs %}
{% code-tabs-item title="Example 6-25. Using JSON translations and the \_\_\(\) helper" %}
```php
// In Blade
{{ __('View friends list') }}
// resources/lang/es.json
{
  'View friends list': 'Ver lista de amigos'
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这是使用\_\_\(\)助手的优点，如果对于当前语言找不到对应的key，它将显示key，如果key就是你应用的默认语言，它比widgets.friends.title更合理。

> JSON格式的翻译只在Laravel 5.4及之后版本有效

