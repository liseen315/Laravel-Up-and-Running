# 有条件地应用属性

您还可以指定响应中的某些属性只在满足特定测试时应用，如示例13-27所示。

{% code-tabs %}
{% code-tabs-item title="Example 13-27. Conditionally applying attributes" %}
```php
public function toArray($request) {
    return [
        'name' => $this->name,
        'breed' => $this->breed,
        'rating' => $this->when(Auth::user()->canSeeRatings(), 12),
    ]; 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

