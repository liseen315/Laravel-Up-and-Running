# 获取请求的基本信息

既然您知道了如何获取请求实例，那么您可以用它做什么呢？请求对象的主要目的是表示当前的HTTP请求，因此请求类提供的主要功能是方便获取有关当前请求的有用信息。

我已经对这里描述的方法进行了分类，但请注意，类别之间肯定存在重叠，并且类别有点随意，希望这些类别可以让您轻松了解可用的内容，随后你就可以抛弃这些分类了。

另外，请注意，请求对象上还有许多可用的方法；这些方法只是最常用的方法

**基础用户输入**

基础用户输入使得通过表单提交或者Ajax请求获取信息变得简单，当我引用"用户的输入"时，我讨论的是查询参数\(GET\)，表单提交\(POST\)或者是JSON，基础的用户输入包含以下：

_all\(\)_

将用户提供的所有输入作为数组返回。

_input\(fieldName\)_

返回用户输入的单个字段

_only\(fieldName\|\[array,of,field,names\]\)_

返回用户特定输入字段作为数组返回

_except\(fieldName\|\[array,of,field,names\]\)_

返回排除后的输入

_exists\(fieldName\)_

返回一个布尔值，指示输入中是否存在该字段。has\(\)是别名

_filled\(fieldName\)_  
  
返回一个布尔值，指示该字段是否存在于输入中且不为空（即具有值）。

_json\(\)_

如果页面发送了JSON，则返回参数包。

json\(keyName\)

从发送到页面的JSON返回给定键的值.

> ParameterBag
>
> 在Laravel中，有时会遇到ParameterBag对象。这个类有点像一个关联数组。您可以使用get\(\)获取特定的key.

> echo $bag-&gt;get\('name'\);
>
> 您还可以使用has\(\)来检查键的存在，使用all\(\)来获取所有键和值的数组，使用count\(\)来计算数量，使用keys\(\)来获取键对应的数组

示例10-3展示如何在请求方法上使用用户提供的信息

{% code-tabs %}
{% code-tabs-item title="Example 10-3. Getting basic user-provided information from the request" %}
```php
// form
<form method="POST" action="/form"> 
    @csrf
    <input name="name"> Name<br>
    <input type="submit"> 
</form>
// Route receiving the form
Route::post('form', function (Request $request) {
    echo 'name is ' . $request->input('name') . '<br>';
    echo 'all input is ' . print_r($request->all()) . '<br>';
    echo 'user provided email address: ' . $request->has('email') ? 'true' : 'false';
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**用户和请求状态**

用户和请求状态方法包括用户未通过表单明确提供的输入

_method\(\)_

返回通过路由的方法\(GET,POST,PATCH等等\)

_path\(\)_

返回不带域的页面路径，例如,[http://www.myapp.com/abc/de](http://www.myapp.com/abc/def)会返回/abc/de

_url\(\)_

返回带域的页面路径，例如[http://www.myapp.com/abc](http://www.myapp.com/abc/def) 会返回 [http://www.myapp.com/abc](http://www.myapp.com/abc/def)

_is\(\)_

返回一个布尔值，指示当前页面请求是否与提供的字符串匹配（例如，/a/b/c将匹配$ $request- &gt;is\('\*_b\*_'\)，其中\*代表任何字符） ; 在Str::is\(\)中使用自定义正则表达式解析器

_ip\(\)_

返回用户ip地址

_header\(\)_

返回头数组\(例如，\['accept-language' =&gt; \['en- US,en;q=0.8'\]\]\)，或者，如果将头名称作为参数传递，则仅返回该头。

_server\(\)_

返回传统上存储在$ \_SERVER中的变量数组\(例如，REMOTE\_ADDR\)，或者传递一个$\_SERVER变量则返回其值。

_secure\(\)_

返回一个布尔值，该值指示此页是否使用HTTPS加载。

_pjax\(\)_

返回一个布尔值，指示页面是否使用pjax\(\)。

_wantsJson\(\)_

返回一个布尔值，指示此请求在其接受头中是否有any /json内容类型。

_isJson\(\)_

返回一个布尔值，指示此页请求的Content-Type头中是否有任何/json内容类型。

_accepts\(\)_

返回一个布尔值，指示此页请求是否接受给定的内容类型_。_

**文件**

到目前为止，我们所涵盖的所有输入都是显式的（通过all\(\)，input\(\)等方法检索）或由浏览器或引用站点定义（通过pjax\(\)等方法检索）。 文件输入类似于显式用户输入，但它们的处理方式大不相同。

_file\(\)_

返回上传文件的数组，如果传递一个键\(上传文件的名字\),则返回文件。

_allFiles\(\)_

返回所有上传文件的数组；由于命名更清晰，与file\(\)相比很有用。

_hasFile\(\)_

返回一个布尔值，指示文件是否以指定的键上传

每个File都是Symfony\Component\HttpFoundation \File\UploadedFile实例，它提供了一套用于验证、处理和存储上传文件的工具。

查看第14章更多示例看如何处理上传。

**持久化**

请求还可以提供与会话交互的功能。 大多数会话功能都存在于其他地方，但有一些方法与当前页面请求相关。

_flash\(\)_

将当前请求的用户输入闪存到稍后要检索的会话，这意味着它已保存到会话但在下一个请求后消失。

_flashOnly\(\)_

为提供的数组键闪存当前用户请求。

_flashExcept\(\)_

闪存当前用户请求,排除提供的数组键。

_old\(\)_

返回以前闪存的所有用户输入的数组，或者，如果传递了一个键，则返回该键的值（如果以前闪存过）。

_flush\(\)_

擦除以前闪存过的用户输入。

_cookie\(\)_

从请求中检索所有cookie，或者，如果提供了_key_，则仅检索该cookie。

_hasCookie\(\)_

返回一个布尔值，指示请求是否具有给定键的cookie。

flash\*\(\)和old\(\)方法用于存储用户输入并在以后检索它，通常在输入验证和拒绝后使用。

