# 序列化响应类型

有一些特殊的响应类型用于视图，下载，文件以及JSON，每个都是一个预定义的宏，可以轻松地复用头或内容结构。

**视图响应**

在第4章中，我们使用全局view\(\)函数演示如何返回模板，例如-view\('view.name.here'\)或者类似的内容。但是如果你要自定义头，HTTP状态码，或者其他的内容，可以如示例10-7一样使用view\(\)

{% code-tabs %}
{% code-tabs-item title="Example 10-7. Using the view\(\) response type" %}
```php
Route::get('/', function (XmlGetterService $xml) { 
    $data = $xml->get();
    return response()
        ->view('xml-structure', $data)
        ->header('Content-Type', 'text/xml');
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**下载响应**

有时，您希望应用程序强制用户的浏览器下载文件，无论您是在Laravel中创建文件，还是从数据库或受保护的位置为其提供服务。download（）响应类型简化了这一过程。

所需的第一个参数是希望浏览器下载的文件的路径。如果是生成的文件，则需要将其临时保存到某个位置。

可选的第二个参数是下载文件的文件名（例如export.csv）。如果您不在此处传递字符串，它将自动生成。可选的第三个参数允许您传递一个头数组。示例10-8说明了download\(\)响应类型的使用。

{% code-tabs %}
{% code-tabs-item title="Example 10-8. Using the download\(\) response type" %}
```php
public function export() {
    return response()
        ->download('file.csv', 'export.csv', ['header' => 'value']);
}

public function otherExport() {
    return response()->download('file.pdf'); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果要在返回下载响应后从磁盘中删除原始文件，可以在download\(\)方法之后链式调用deleteFileAfterSend\(\)方法：

```php
public function export() {
    return response()
        ->download('file.csv', 'export.csv')
        ->deleteFileAfterSend();
}
```

**文件响应**

文件响应类似于下载响应，但它允许浏览器显示文件而不是强制下载。这在图像和PDF中最常见。

所需的第一个参数是文件名，可选的第二个参数可以是一个头数组（参见示例10-9）。

{% code-tabs %}
{% code-tabs-item title="Example 10-9. Using the file\(\) response type" %}
```php
public function invoice($id) {
    return response()->file("./invoices/{$id}.pdf", ['header' => 'value']); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**JSON响应**

JSON响应非常常见，即使它们对程序来说并不是特别复杂，但它们也有一个定制的响应。

JSON响应将传递的数据转换为JSON（使用json\_encode\(\)）并将内容类型设置为application/json。您也可以选择使用setCallback\(\)方法来创建JSONP响应，而不是JSON，如示例10-10所示。

{% code-tabs %}
{% code-tabs-item title="Example 10-10. Using the json\(\) response type" %}
```php
public function contacts() {
    return response()->json(Contact::all()); 
}

public function jsonpContacts(Request $request) {
    return response()
        ->json(Contact::all())
        ->setCallback($request->input('callback'));
}

public function nonEloquentContacts() {
    return response()->json(['Tom', 'Jerry']); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**重定向响应**

重定向通常不是在response\(\)帮助程序上调用的，因此它们与我们已经讨论过的其他自定义响应类型略有不同，但它们仍然只是一种不同的响应。重定向，从Laravel路由返回，向用户发送重定向（通常是301）到另一页或返回到上一页。

从技术上讲，您可以从response\(\)调用重定向，如返回response\(\)-&gt;redirectTo\('/'\)。 但更常见的是，您将使用特定于重定向的全局帮助程序。

有一个全局redirect\(\)函数可用于创建重定向响应，还有一个全局back（）函数是redirect\(\)-&gt;back\(\)的快捷方式。

就像大多数全局帮助程序一样，redirect\(\)全局函数既可以传递参数，也可以用于获取其类的实例，然后链式调用该实例方法。 如果你不链式调用，只是传递参数，redirect\(\)的执行方式与redirect\(\)-&gt; to\(\)相同; 它需要一个字符串并重定向到该字符串URL。 例10-11显示了它的一些使用例子.

{% code-tabs %}
{% code-tabs-item title="Example 10-11. Examples of using the redirect\(\) global helper" %}
```php
return redirect('account/payment');
return redirect()->to('account/payment');
return redirect()->route('account.payment');
return redirect()->action('AccountController@showPayment');
// If redirecting to an external domain
return redirect()->away('https://tighten.co');
// If named route or controller needs parameters
return redirect()->route('contacts.edit', ['id' => 15]);
return redirect()->action('ContactsController@edit', ['id' => 15]);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

你也可以重定向”回“之前的页面，当处理和验证用户输入特别有用，如示例10-12显示了验证例子。

{% code-tabs %}
{% code-tabs-item title="Example 10-12. Redirect back with input" %}
```php
public function store() {
    // If validation fails...
    return back()->withInput(); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

最后，你可以在重定向的同时闪存数据到session，通常携带错误或者成功消息，如示例10-13。

{% code-tabs %}
{% code-tabs-item title="Example 10-13. Redirect with flashed data" %}
```php
Route::post('contacts', function () { 
    // Store the contact
    return redirect('dashboard')->with('message', 'Contact created!');
});
Route::get('dashboard', function () {
    // Get the flashed data from session--usually handled in Blade template 
    echo session('message');
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**自定义响应宏**

还可以使用宏创建自己的自定义响应类型。这允许您定义对响应及其提供的内容进行的一系列修改。

让我们重新创建json\(\)自定义响应类型，只是为了看它是如何工作的。 与往常一样，您应该为这些类型的绑定创建自定义服务提供程序，但是现在我们只需将它放在AppServiceProvider中，如例10-14所示。

{% code-tabs %}
{% code-tabs-item title="Example 10-14. Creating a custom response macro" %}
```php
class AppServiceProvider {
    public function boot() {
        Response::macro('myJson', function ($content) { 
        return response(json_encode($content))
            ->withHeaders(['Content-Type' => 'application/json']);
         }); 
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

然后，我们可以像之前json\(\)一样使用宏:

```php
return response()->myJson(['name' => 'Sangeetha']);
```

这将返回一编码过的json数组，然后带有JSON头。

**响应式接口**

如果你想自定义发送的响应并且宏不能满足需求，或者如果你希望任何对象都能够以“响应”的形式返回，并具有自己的逻辑用于显示，响应式接口正用于此\(Laravel 5.5可用\)。

响应式接口，Illuminate\Contracts\Support\Responsable指示其实现者必须要有toResponse方法，需要返回一个Illuminate对象。示例10-15说明如何创建响应式对象。

{% code-tabs %}
{% code-tabs-item title="Example 10-15. Creating a simple Responsable object" %}
```php
use Illuminate\Contracts\Support\Responsable;
class MyJson implements Responsable {
    public function __construct($content) {
        $this->content = $content;
    }
    public function toResponse() {
        return response(json_encode($this->content))
            ->withHeaders(['Content-Type' => 'application/json']);
    }

```
{% endcode-tabs-item %}
{% endcode-tabs %}

然后我们像宏一样调用。

```php
return new MyJson(['name' => 'Sangeetha']);
```

这可能看起来像是相对于响应宏的大量工作。但是，当您处理更复杂的控制器操作时，可响应的界面确实很好用。一个常见的示例是使用它来创建视图模型（或视图对象），如示例10-16中所示。

{% code-tabs %}
{% code-tabs-item title="Example 10-16. Using Responsable to create a view object" %}
```php
use Illuminate\Contracts\Support\Responsable;
class GroupDonationDashboard implements Responsable {
    public function __construct($group) {
        $this->group = $group;
    }
    public function budgetThisYear() {
        // ...
    }
    public function giftsThisYear() {
        // ...
    }
    public function toResponse() {
        return view('groups.dashboard')
            ->with('annual_budget', $this->budgetThisYear())
            ->with('annual_gifts_received', $this->giftsThisYear());
    }

```
{% endcode-tabs-item %}
{% endcode-tabs %}

在这种情况下，它开始变得更有意义，将复杂的视图移动到一个专用的、可测试的对象中，并保持控制器干净。这是一个控制器，它使用了那个响应对象.

```php
class GroupController {
    public function index(Group $group) {
        return new GroupDonationsDashboard($group); 
    }
```



