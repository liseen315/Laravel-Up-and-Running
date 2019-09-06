# 资源集合

现在，让我们来谈谈如果你从给定的API返回多个实体，会发生什么。 这可以使用API​​资源的collection\(\)方法，如例13-20中所示。

{% code-tabs %}
{% code-tabs-item title="Example 13-20. Using the default API resource collection method" %}
```php
use App\Dog;
use App\Http\Resources\Dog as DogResource;
Route::get('dogs', function () {
    return DogResource::collection(Dog::all());
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这个方法遍历传入的每个条目，然后用DogResource API资源转换它，然后返回集合。

这对于许多api来说可能已经足够了，但是如果需要自定义任何结构或将元数据添加到集合响应中，则需要创建自定义API资源集合。

为了做到这一点，我们再次使用make:resource命令行工具，这次我们命名为DogCollection，这将告诉Laravel 创建一个API 资源结合，而不仅仅是API 资源。

```bash
php artisan make:resource DogCollection
```

这将生成一个与api资源文件非常相似的新文件，位于app/Http/ Resources/DogCollection.php，其中再次包含一个方法：toArray\(\)。

{% code-tabs %}
{% code-tabs-item title="Example 13-21. Generated API resource collection" %}
```php
<?php

namespace App\Http\Resources;

use Illuminate\Http\Resources\Json\ResourceCollection;

class DogCollection extends ResourceCollection
{
    /**
     * Transform the resource collection into an array.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return array
     */
    public function toArray($request)
    {
        return parent::toArray($request);
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

就像API资源一样，我们可以访问请求和底层数据。 但与API资源不同，我们处理的是项目集合而不仅仅是一个项目，因此我们将以$this- &gt;collection的形式访问已经转换的集合。 以示例13-22为例.

{% code-tabs %}
{% code-tabs-item title="Example 13-22. A simple API resource collection for the Dog model" %}
```php
<?php

namespace App\Http\Resources;

use Illuminate\Http\Resources\Json\ResourceCollection;

class DogCollection extends ResourceCollection
{
    /**
     * Transform the resource collection into an array.
     *
     * @param  \Illuminate\Http\Request $request
     * @return array
     */
    public function toArray($request)
    {
        return [
            'data' => $this->collection,
            'links' => [
                'self' => route('dogs.index'),
            ],
        ];
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

