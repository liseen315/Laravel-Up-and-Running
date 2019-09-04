# Eloquent 分页

分页是大多数API需要首要考虑的。Eloquent有一个现成的分页系统，它直接与每个页面请求挂钩，我们已经在第6章介绍了分页组件，我们进行一些复习。

Eloquent提供了paginate\(\)方法，你可以向其传递每页的条目数，然后Eloquent会检查页面的查询参数，如果设置了参数，则将其视为用户进入分页列表的页面数的指示器。

要让API路由可以自动分页，请使用paginate\(\)而不是all\(\)或者是get\(\)来调用Eloquent查询，类似示例13-7

```php
Route::get('dogs', function () { 
    return Dog::paginate(20);
});
```

我们已经定义了Eloquent应该从数据库中获得20个结果。 根据页面查询参数的设置，Laravel将准确知道为我们提取20个结果。

```text
GET /dogs - Return results 1-20 
GET /dogs?page=1 - Return results 1-20 
GET /dogs?page=2 - Return results 21-40
```

注意paginate\(\)方法也可以用于查询生成器调用，如示例13-8所示。

```text
Route::get('dogs', function () {
    return DB::table('dogs')->paginate(20);
});
```

这里有一些有趣的事情：当你将它转换为JSON时，这不仅仅会返回20个结果。 相反，它将构建一个响应对象，该对象会自动将一些有用的与分页相关的详细信息与分页数据一起传递给最终用户。示例13-9显示了我们调用后的响应，为了节省空间，它被截断为只有三条记录。

```php
{
   "current_page": 1,
   "data": [
      {
        'name': 'Fido'
      },
      {
         'name': 'Pickles'
      },
      {
         'name': 'Spot'
      }
   ]
   "first_page_url": "http://myapp.com/api/dogs?page=1",
   "from": 1,
   "last_page": 2,
   "last_page_url": "http://myapp.com/api/dogs?page=2",
   "next_page_url": "http://myapp.com/api/dogs?page=2",
   "path": "http://myapp.com/api/dogs",
   "per_page": 2,
   "prev_page_url": null,
   "to": 2,
   "total": 4
}
```

