# 分页

对于web应用，分页仍然非常复杂，值得庆幸的是，Laravel内置了分页，而且它默认可以hook进Eloquent和路由

**数据库结果分页**

分页常用于显示数据库查询结果，Eloquent和查询器从当前页面请求读取分页参数，然后将参数传给paginate\(\)方法，传递的参数用于你想每页显示多少条数据。如示例6-15所示

{% code-tabs %}
{% code-tabs-item title="Example 6-15. Paginating a query builder response" %}
```php
// PostsController
public function index() {
    return view('posts.index', ['posts' => DB::table('posts')->paginate(20)]); 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

示例6-15指定此路由应每页返回20个帖子，并将根据URL的页面查询参数（如果有）来定义当前用户所在的结果“页面”，Eloquent模型都有paginate\(\)方法。

当您在视图中显示结果时，集合上现在将有一个links\(\)方法（或laravel 5.1的render\(\)），该方法将输出分页控件

默认情况下，将使用Bootstrap库分页组件（参见示例6-16）

{% code-tabs %}
{% code-tabs-item title="Example 6-16. Rendering pagination links in a template" %}
```text
// posts/index.blade.php
<table>
@foreach ($posts as $post)
    <tr><td>{{ $post->title }}</td></tr> 
@endforeach
</table>

{{ $posts->links() }}

// By default, $posts->links() will output something like this:
<ul class="pagination">
    <li class="page-item disabled"><span>&laquo;</span></li>
    <li class="page-item active"><span>1</span></li>
    <li class="page-item">
        <a class="page-link" href="http://myapp.com/posts?page=2">2</a>
    </li>
    <li class="page-item">
        <a class="page-link" href="http://myapp.com/posts?page=3">3</a>
        </li>
    <li class="page-item">
        <a class="page-link" href="http://myapp.com/posts?page=2" rel="next">
&raquo; </a>
    </li>
</ul>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> Laravel 5.7及之后版本自定义分页连接数字
>
> 如果你想控制当前页左右两边的分页数量,在Laravel 5.7之后可以通过调用onEachSide\(\)实现

```php
DB::table('posts')->paginate(10)->onEachSide(3); 
// Outputs:
// 5 6 7 [8] 9 10 11
```

**手动创建分页**

如果你不使用Eloquent或者是查询器,或者你使用复杂的查询\(例如,groupBy\),你需要手动创建分页，你可以使用Illuminate\Pagination \Paginator或者Illuminate\Pagination\LengthAwarePaginator类

这两个类的区别是，Paginator只有上一页下一页，不会提供每页的跳转，LengthAwarePaginator知道结果的完整长度。

Paginator 和 LengthAwarePaginator 需要你手写分页逻辑然后传递给视图,如示例6-17

{% code-tabs %}
{% code-tabs-item title="Example 6-17. Manually creating a paginator" %}
```php
use Illuminate\Http\Request;
use Illuminate\Pagination\Paginator;
Route::get('people', function (Request $request) {
    $people = [...]; // huge list of people
    $perPage = 15;
    $offsetPages = $request->input('page', 1) - 1;
    // The Paginator will not slice your array for you
    $people = array_slice(
        $people,
        $offsetPages * $perPage,
        $perPage
    );
    return new Paginator($people,
        $perPage
    ); 
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

在新版中Paginator语法已经变更,如果你使用的是Laravel 5.1请翻阅[文档](https://laravel.com/docs/5.1/pagination)

