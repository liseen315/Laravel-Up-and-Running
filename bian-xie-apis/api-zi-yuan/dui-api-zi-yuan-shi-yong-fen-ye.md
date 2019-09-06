# 对API资源使用分页

就像你可以传递一个Eloquent模型集合到资源一样，你也可以传递一个分页器实例，如示例13-25

{% code-tabs %}
{% code-tabs-item title="Example 13-25. Passing a paginator instance to an API resource collection" %}
```php
Route::get('dogs', function () {
    return new DogCollection(Dog::paginate(20));
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果传递一个分页器实例，则转换后的结果将包含分页信息（第一页、最后一页、上一页和下一页）和有关整个集合的元信息。

你可以看一下示例13-26，看看这些信息是什么样子的。在这个例子中，我通过调用Dog::paginate\(2\)将每页的项目数设置为2，这样可以更容易地看出是如何工作的。

{% code-tabs %}
{% code-tabs-item title="Example 13-26. A sample paginated resource response with pagination links" %}
```php
{
    "data": [
        {
        "name": "Pickles", "breed": "Chorkie",
        }, 
        {
        "breed": "Golden Retriever Mix", 
        }
    ],
    "links": {
        "first": "http://gooddogbrant.com/api/dogs?page=1",
        "last": "http://gooddogbrant.com/api/dogs?page=3",
        "prev": null,
        "next": "http://gooddogbrant.com/api/dogs?page=2"
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 3,
        "path": "http://gooddogbrant.com/api/dogs", "per_page": 2,
        "to": 2,
        "total": 5 
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

