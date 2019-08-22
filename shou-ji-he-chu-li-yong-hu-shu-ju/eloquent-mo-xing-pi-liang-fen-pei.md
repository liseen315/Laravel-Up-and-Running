# Eloquent 模型批量赋值

目前位置我们一直关注的是控制器级的验证，我们也可以看下模型上的验证。

将表单的全部输入直接传递到数据库模型是一种常见（但不推荐）模式。在Laravel中，这可能类似于示例7-19。

{% code-tabs %}
{% code-tabs-item title="Example 7-19. Passing the entirety of a form to an Eloquent model" %}
```php
Route::post('posts', function (Request $request) { 
    $newPost = Post::create($request->all());
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

我们假设最终用户是善良的，不是恶意的，并且只保留了我们希望他们编辑的字段，可能是文章标题或正文。

但是，如果我们的最终用户能够猜测出，我们这个posts表上有一个author\_id字段，那又会怎样呢？如果他们使用浏览器工具添加作者ID字段并将ID设置为其他人的ID，并通过创建属于他们的假博客文章来模仿其他人，会怎么样？

Eloquent有一个称为"批量赋值"的概念，允许你将可填充字段\(使用模型的$fillable属性\)放到白名单，或者是将不可填充字段\(模型的$guarded属性\)放到黑名单然后通过数组传递给create\(\)和update\(\)，更多查看124页的"批量赋值"

在我们的例子中，我们可能需要像示例7-20那样填写模型，以保证我们的应用程序的安全。

{% code-tabs %}
{% code-tabs-item title="Example 7-20. Guarding an Eloquent model from mischievous mass assignment" %}
```php
class Post extends Model {
    // Disable mass assignment on the author_id field
    protected $guarded = ['author_id']; 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

通过将author\_id设置为guarded，我们确保恶意用户无法再通过手动将其添加到应用的表单来覆盖此字段的值。

> 使用$request-&gt;only\(\)双层保护
>
> 尽管保护我们的模型不受批量赋值很重要，但在赋值结束时还是得小心。比起使用$request-&gt;all\(\)，不如使用$request-&gt;only\(\)来指定哪个字段传递给你的模型。

