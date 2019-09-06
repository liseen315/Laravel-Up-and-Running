# 排序你的API 结果

首先看下排序结果，如示例13-10,排序单个字段，然后单方向。

{% code-tabs %}
{% code-tabs-item title="Example 13-10. Simplest API sorting" %}
```php
// Handles /dogs?sort=name
Route::get('dogs', function (Request $request) {
    // Get the sort query parameter (or fall back to default sort "name") 
    $sortColumn = $request->input('sort', 'name');
    return Dog::orderBy($sortColumn)->paginate(20);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

示例13-11我们添加了反转的能力

{% code-tabs %}
{% code-tabs-item title="Example 13-11. Single-column API sorting, with direction control" %}
```php
// Handles /dogs?sort=name and /dogs?sort=-name
Route::get('dogs', function (Request $request) {
    // Get the sort query parameter (or fall back to default sort "name") 
    $sortColumn = $request->input('sort', 'name');
    // Set the sort direction based on whether the key starts with - 
    // using Laravel's starts_with() helper function
    $sortDirection = starts_with($sortColumn, '-') ? 'desc' : 'asc'; 
    $sortColumn = ltrim($sortColumn, '-');
    return Dog::orderBy($sortColumn, $sortDirection) ->paginate(20);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

最后我们对多个字段排序，（例如，?sort=name,-weight）如示例13-12

{% code-tabs %}
{% code-tabs-item title="Example 13-12. JSON API–style sorting" %}
```php
// Handles ?sort=name,-weight
Route::get('dogs', function (Request $request) {
    // Grab the query parameter and turn it into an array exploded by ,
    $sorts = explode(',', $request->input('sort', ''));
    // Create a query
    $query = Dog::query();
    // Add the sorts one by one
    foreach ($sorts as $sortColumn) {
        $sortDirection = starts_with($sortColumn, '-') ? 'desc' : 'asc';
        $sortColumn = ltrim($sortColumn, '-');
        $query->orderBy($sortColumn, $sortDirection);
    }
    // Return
    return $query->paginate(20);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

正如您所看到的，这不是最简单的过程，您可能希望围绕重复过程构建一些辅助工具，但我们正在使用逻辑和简单的功能逐个构建我们的API的可定制性。

