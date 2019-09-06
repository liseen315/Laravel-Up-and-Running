# 嵌套关系

API相对复杂的一方面是处理嵌套关系，使用API​​资源的最简单方法是向返回的数组添加一个键，该键设置为API资源集合，如例13-23所示。

{% code-tabs %}
{% code-tabs-item title="Example 13-23. A simple included API relationship" %}
```php
public function toArray() {
    return [
        'name' => $this->name,
        'breed' => $this->breed,
        'friends' => DogResource::collection($this->friends),
    ]; 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

你可能也想添加判断属性，你可以在请求时才嵌套或者只有在已经传入的Eloquent对象上已经加载了时候嵌套，如示例13-24。

{% code-tabs %}
{% code-tabs-item title="Example 13-24. Conditionally loading API relationship" %}
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
            'name' => $this->name,
            'breed' => $this->breed,
            // Only load this relationship if it's been eager-loaded
            'bones' => BoneResource::collection($this->whenLoaded('bones')),
            // Or only load this relationship if the URL asks for it
            'bones' => $this->when(
                $request->get('include') == 'bones',
                BoneResource::collection($this->bones)
            ),
        ];
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

