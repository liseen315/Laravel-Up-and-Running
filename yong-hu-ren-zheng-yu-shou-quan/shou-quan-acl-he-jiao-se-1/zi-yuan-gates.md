# 资源Gates

ACL最常见的用途是定义对”资源“的访问权限\(想象一个Eloquent 模型或者是是否允许在管理面板进行管理\)。

resource\(\)有四种常见的gates, view，create，update，和delete。

```php
Gate::resource('photos', 'App\Policies\PhotoPolicy');
```

这相当于定义以下内容：

```php
Gate::define('photos.view', 'App\Policies\PhotoPolicy@view');
Gate::define('photos.create', 'App\Policies\PhotoPolicy@create');
Gate::define('photos.update', 'App\Policies\PhotoPolicy@update');
Gate::define('photos.delete', 'App\Policies\PhotoPolicy@delete');
```



