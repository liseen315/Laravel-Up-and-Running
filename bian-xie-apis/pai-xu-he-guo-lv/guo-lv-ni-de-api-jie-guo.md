# 过滤你的API结果

构建API的另一个常见任务是过滤掉除了某个数据子集之外的所有数据。 例如，客户可能会要求列出吉娃娃狗。

JSON API在这里没有给出任何关于语法的好主意，除此之外我们应该使用过滤器查询参数。 让我们按照排序语法的思路思考，我们将所有内容放在一个单一的键中 - 也许？?filter=breed:chihuahua 你可以在示例13-13看到如何做到这一点。

{% code-tabs %}
{% code-tabs-item title="Example 13-13. Single filter on API results" %}
```php
Route::get('dogs', function () {
    $query = Dog::query();
    $query->when(request()->filled('filter'), function ($query) {
        [$criteria, $value] = explode(':', request('filter'));
        return $query->where($criteria, $value);
    });
    return $query->paginate(20);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

注意，在示例13-13中，我们使用的是request\(\)助手，而不是注入$request的实例。两者的工作原理相同，但有时当您在一个闭包中工作时，request\(\)助手会更容易，因为不必手动传递变量。

> Laravel 5.2之前版本的查询条件修改
>
> 如果你的项目运行在Laravel 5.2之前版本，你需要使用PHP if 语句替代$query-&gt;when\(\)

示例13-14我们运行多层过滤。例如?filter=breed:chihuahua,color:brown

{% code-tabs %}
{% code-tabs-item title="Example 13-14. Multiple filters on API results" %}
```php
Route::get('dogs', function (Request $request) {
    $query = Dog::query();
    $query->when(request()->filled('filter'), function ($query) {
        $filters = explode(',', request('filter'));
        foreach ($filters as $filter) {
            [$criteria, $value] = explode(':', $filter);
            $query->where($criteria, $value);
        }
        return $query;
    });
    return $query->paginate(20);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}



