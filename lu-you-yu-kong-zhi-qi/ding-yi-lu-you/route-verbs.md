# Route Verbs

你可能注意到我们在路由定义中使用了Route::get\(\),这意味着告诉Laravel只能在HTTP请求使用get操作的时候才能匹配到路由,但是如果是个表单,或者是一些javascript发送的PUT或是DELETE请求呢?对于调用路由定义的方法,还有其他的一些选项,如例子3-3所示:

```php
Route::get('/', function () { return 'Hello, World!';
});
Route::post('/', function () {
// Handle someone sending a POST request to this route
});
Route::put('/', function () {
// Handle someone sending a PUT request to this route
});
Route::delete('/', function () {
// Handle someone sending a DELETE request to this route
});
Route::any('/', function () {
// Handle any verb request to this route
});
Route::match(['get', 'post'], '/', function () { // Handle GET or POST requests to this route
});
```

