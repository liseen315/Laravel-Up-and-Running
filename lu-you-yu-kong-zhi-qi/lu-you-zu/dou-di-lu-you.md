# 兜底路由

在Laravel5.6之前,你可以定义兜底路由用来捕获匹配不到的路径

```php
Route::any('{anything}', 'CatchAllController')->where('anything', '*');
```

在Laravel 5.6以后的版本,你可以使用Route::fallback\(\)方法替代

```php
Route::fallback(function () {
    
});
```

