# 控制器组织和JSON返回

Laravel的API资源控制器与普通资源控制器类似（请参见第47页的“资源控制器”），但已修改为与RESTful API路由一致。 例如，它们排除了create\(\)和edit\(\)方法，这两种方法在API中都是无关紧要的。 让我们开始吧。 首先，我们将为我们的资源创建一个新的控制器，我们将在/api/ dogs路由。

```bash
php artisan make:controller Api\DogsController --api
```

> 在Laravel 5.3之前转义Artisan命令中的斜线
>
> 在5.3之前的laravel版本中，你需要在Artisan命令中使用正斜杠来转义命名空间分隔符中的\，就像这样

```bash
 php artisan make:controller Api/\DogsController --api
```

请注意，在运行5.5之前的Laravel版本的项目中，API资源控制器和API资源路由的概念不存在。 您仍然可以使用常规资源控制器和资源路径; 它们几乎完全相同，但有一些未在API中使用的与视图相关的路由。 例13-2显示了我们的API资源控制器

```php
、<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class ApiDogsController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        //
    }

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        //
    }

    /**
     * Display the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function show($id)
    {
        //
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, $id)
    {
        //
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function destroy($id)
    {
        //
    }
}
```

docblocks几乎讲述了这个故事。 index（）列出了所有的狗，show（）列出了一只狗，store（）存储了一只新狗，update（）更新了一只狗，destroy（）删除了一只狗。

要快速的创建模型与迁移，我们可以这样做。

```bash
php artisan make:model Dog --migration
php artisan migrate
```

Great!，接下来我们可以填写控制器方法。

> 这些代码示例的数据库要求
>
> 如果你希望我们在这里编写的代码能够实际工作，需要向名为name的迁移和另一个名为breed的迁移中添加一个string\(\)列，并将这些列添加到后续模型的fillable属性中，或者将该模型的保护属性设置为空数组。\(\[\]\)

我们可以利用Eloquent特性，如果返回一个Eloquent集合，将自动转换成JSON\(如果你好奇，实际用了\_\_toString魔术方法\)，这意味着如果你从路由返回结果集，实际返回了JSON，如示例13-3所示，这是你要编写的简单代码

{% code-tabs %}
{% code-tabs-item title="Example 13-3. A sample API resource controller for the Dog entity" %}
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class ApiDogsController extends Controller
{
    public function index()
    {
        return Dog::all();
    }

    public function store(Request $request)
    {
        return Dog::create($request->only(['name', 'breed']));
    }

    public function show($id)
    {
        return Dog::findOrFail($id);
    }

    public function update(Request $request, $id)
    {
        $dog = Dog::findOrFail($id);
        $dog->update($request->only(['name', 'breed']));
        return $dog;
    }

    public function destroy($id)
    {
        Dog::findOrFail($id)->delete();
    }
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

示例13-4 显示我们如何链接到路由，正如你看到的我们可以使用Route::apiResource\(\)自动将所有这些默认方法映射到它们相应的路由和HTTP verbs。

{% code-tabs %}
{% code-tabs-item title="Example 13-4. Binding the routes for a resource controller" %}
```php
// routes/api.php
Route::namespace('Api')->group(function () { 
    Route::apiResource('dogs', 'DogsController');
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

是的，这是你的第一个RESTful API，当然还有很多细节：分页，排序，认证，定义响应头。但这是这些的基础。

