# 上传文件

我们已经讨论了用户的文本输入，但是文件上传也要考虑。Request对象使用$request-&gt;file\(\)方法访问上传的文件，将文件名作为参数，然后返回Symfony\Component\HttpFoundation\File\Uploaded实例。让我们看个例子，首先我们的表单如示例7-10

{% code-tabs %}
{% code-tabs-item title="Example 7-10. A form to upload files" %}
```markup
<form method="post" enctype="multipart/form-data"> 
    @csrf
    <input type="text" name="name">
    <input type="file" name="profile_picture">
    <input type="submit">
</form>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

让我们看看运行$request-&gt;all\(\)得到的结果，如示例7-11，注意用$request-&gt;input\('profile\_picture'\)将返回空值，请使用$request-&gt;file\('profile\_picture'\)代替。

{% code-tabs %}
{% code-tabs-item title="Example 7-11. The output from submitting the form in Example 7-10" %}
```php
Route::post('form', function (Request $request) { 
    var_dump($request->all());
});

// Output:
// [
//     "_token" => "token here",
//     "name" => "asdf",
//     "profile_picture" => UploadedFile {},
// ]

Route::post('form', function (Request $request) { 
    if ($request->hasFile('profile_picture')) {
        var_dump($request->file('profile_picture'));
    }
});

// Output:
// UploadedFile (details)
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> 验证文件上传
>
> 如例7-11所示，我们可以访问$request-&gt;hasfile\(\)，查看用户是否上传了一个文件。我们还可以通过对文件本身使用isvalid\(\)来检查文件上载是否成功。

```php
if ($request->file('profile_picture')->isValid()) { 
    //
}
```

> 因为isValid是在文件自身上调用的，所以如果用户没有上载文件，它将出错。因此，要同时检查这两个条件，您需要首先检查文件是否存在。

```php
if ($request->hasFile('profile_picture') 
    && $request->file('profile_picture')->isValid()) { 
        //
}
```

Symfony的UploadedFile类扩展了原生PHP SplFileInfo，使你能够轻松地检查和操作文件。这份清单并不详尽，但它能让你领略到你能做什么。

* guessExtension\(\)
* getMimeType\(\)
* store\($path, $storageDisk = default disk\)
* storeAs\($path, $newName, $storageDisk = default disk\)
* storePublicly\($path, $storageDisk = default disk\)
* storePubliclyAs\($path, $newName, $storageDisk = default disk\)
* move\($directory, $newName = null\)
* getClientOriginalName\(\)
* getClientOriginalExtension\(\)
* getClientMimeType\(\)
* guessClientExtension\(\)
* getClientSize\(\)
* getError\(\)
* isValid\(\)

如您所见，大多数方法都与获取上传文件的信息有关，但有一种方法你可能比所有其他方法使用得更频繁:store\(\)（从laravel 5.3开始提供），它获取随请求上传的文件并将其存储在指定目录中。在服务器上。它的第一个参数是目标目录，可选的第二个参数是用于存储文件的存储磁盘（s3、local等）。如示例7-12

{% code-tabs %}
{% code-tabs-item title="Example 7-12. Common file upload workflow" %}
```php
if ($request->hasFile('profile_picture')) {
    $path = $request->profile_picture->store('profiles', 's3');
    auth()->user()->profile_picture = $path;
    auth()->user()->save();
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果你想要指定文件名，你可以使用storeAs\(\)替换store\(\)，第一个参数仍然是目录，第二个参数是名字，可选的第三个参数是存储磁盘。

{% hint style="info" %}
文件上传表单编码

如果试图从请求中获取文件内容时得到空值，则可能忘记了在表单上设置编码类型。确保在表单上添加属性enctype=“multipart/form-data”

&lt;form method="post" enctype="multipart/form-data"&gt;
{% endhint %}

