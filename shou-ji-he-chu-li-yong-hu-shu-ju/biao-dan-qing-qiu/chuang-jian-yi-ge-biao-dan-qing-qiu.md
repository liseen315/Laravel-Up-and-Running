# 创建一个表单请求

你可以通过命令行创建一个新的表单请求

`php artisan make:request CreateCommentRequest`

现在你可以在app/Http/Requests/ CreateCommentRequest.php中使用表单请求

每个表单请求类都提供一个或两个公共方法。 第一个是rules\(\)，它需要为此请求返回一组验证规则。 第二个（可选）方法是authorize\(\); 如果返回true，则授权用户执行此请求，如果为false，则拒绝用户。 查看示例7-17以查看示例表单请求

{% code-tabs %}
{% code-tabs-item title="Example 7-17. Sample form request" %}
```php
<?php

namespace App\Http\Requests;

use App\BlogPost;
use Illuminate\Foundation\Http\FormRequest;

class CreateCommentRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        $blogPostId = $this->route('blogPost');
        return auth()->check() && BlogPost::where('id', $blogPostId)
                ->where('user_id', auth()->id())->exists();
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            'body' => 'required|max:1000',
        ];
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

示例7-17的rules\(\)部分非常简单，但让我们简单地看一下authorize\(\)。

我们正在从名为blogPost的路由中获取段。这意味着这个路由的路由定义可能看起来有点像这样:route::post\(“blog post s/blogpost”，function\(\)//Do stuff\)。如您所见，我们将路由参数命名为blogPost，这样就可以使用$this-&gt;route\(“blogPost”\)在请求中访问它。

然后，我们查看用户是否已登录，如果是，则查找登录用户的博客帖子。 您已经在第5章中学习了一些更简单的方法来检查所有权，但我们会在此处更加明确地保持其简介。 我们将很快介绍它的含义，但重要的是要知道返回true表示用户有权执行指定的操作（在这种情况下，创建评论），false表示用户未经授权

> Laravel 5.3之前扩展请求
>
> Laravel 5.3之前版本表单请求使用App\Http\Requests\Request代替Illuminate \Foundation\Http\FormRequest

