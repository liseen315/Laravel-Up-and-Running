# 控制器授权

Laravel中的App\Http\Controllers\Controller引入了AuthorizesRequests，它为授权提供了三个方法:authorize\(\), authorizeForUser\(\), 和 authorizeResource\(\)。

authorize\(\)将一个键和一个对象（或对象数组）作为参数，如果授权失败，它将使用403（未授权）状态代码退出应用程序。 这意味着此功能可以将三行授权代码转换为一行，如例9-20所示

{% code-tabs %}
{% code-tabs-item title="Example 9-20. Simplifying controller authorization with authorize\(\)" %}
```php
// From this:
public function edit(Contact $contact) {
    if (Gate::cannot('update-contact', $contact)) { 
        abort(403);
    }
    return view('contacts.edit', ['contact' => $contact]); 
}
// To this:
public function edit(Contact $contact) {
    $this->authorize('update-contact', $contact);
    return view('contacts.edit', ['contact' => $contact]);
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

authorizeForUser\(\)是相同的，但允许您传入一个用户对象，而不是默认为当前已验证的用户。

```php
$this->authorizeForUser($user, 'update-contact', $contact);
```

authorizeResource\(\)在控制器构造函数中调用一次，将一组预定义的授权规则映射到该控制器中的每个restful控制器方法-类似于示例9-21

{% code-tabs %}
{% code-tabs-item title="Example 9-21. The authorization-to-method mappings of authorizeResource\(\)" %}
```php
class ContactsController extends Controller
{
    public function __construct()
    {
        // This call does everything you see in the methods below.
        // If you put this here, you can remove all authorize()
        // calls in the individual resource methods here.
        $this->authorizeResource(Contact::class);
    }

    public function index()
    {
        $this->authorize('view', Contact::class);
    }

    public function create()
    {
        $this->authorize('create', Contact::class);
    }

    public function store(Request $request)
    {
        $this->authorize('create', Contact::class);
    }

    public function show(Contact $contact)
    {
        $this->authorize('view', $contact);
    }

    public function edit(Contact $contact)
    {
        $this->authorize('update', $contact);
    }

    public function update(Request $request, Contact $contact)
    {
        $this->authorize('update', $contact);
    }

    public function destroy(Contact $contact)
    {
        $this->authorize('delete', $contact);
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

