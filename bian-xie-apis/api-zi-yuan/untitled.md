# 创建一个资源类

让我们来看看这个Dog示例，了解转换API输出的样子。 首先，使用Artisan命令make：resource创建您的第一个资源。

```bash
php artisan make:resource Dog
```

这将在app/Http/Resources/Dog.php创建一个新类，其中包含一个toArray方法。如示例13-17所示。

{% code-tabs %}
{% code-tabs-item title="Example 13-17. Generated API resource" %}
```php
<?php

namespace App\Http\Resources;

use Illuminate\Http\Resources\Json\JsonResource;

class Dog extends JsonResource
{
    /**
     * Transform the resource into an array.
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

我们在这里使用的toArray\(\)方法可以访问两个重要的数据。 首先，它可以访问Illuminate Request对象，因此我们可以根据查询参数和头以及其他任何重要的内容自定义响应。 第二，它可以通过在$ this上调用它的属性和方法来访问整个Eloquent对象，如例13-18所示。

{% code-tabs %}
{% code-tabs-item title="Example 13-18. Simple API resource for the Dog model" %}
```php
<?php

namespace App\Http\Resources;

use Illuminate\Http\Resources\Json\JsonResource;

class Dog extends JsonResource
{
    /**
     * Transform the resource into an array.
     *
     * @param  \Illuminate\Http\Request $request
     * @return array
     */
    public function toArray($request)
    {
        return [
            'id' => $this->id,
            'name' => $this->name,
            'breed' => $this->breed,
        ];
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

要使用此新资源，您需要更新任何返回单个Dog的API端点，以便在新资源中包装响应，如例13-19中所示。

{% code-tabs %}
{% code-tabs-item title="Example 13-19. Using the simple Dog resource" %}
```php
use App\Dog;
use App\Http\Resources\Dog as DogResource;
Route::get('dogs/{dogId}', function ($dogId) { 
    return new DogResource(Dog::find($dogId));
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

