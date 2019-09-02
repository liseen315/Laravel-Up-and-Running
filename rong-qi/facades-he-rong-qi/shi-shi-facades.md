# 实时Facades

Laravel 5.4引入了一个新概念称为实时facades。比起创建一个新类来创建类实例方法作为静态方法，你可以简单地在你的类的FQCN前面加上Facades \，就好像它是一个facade。如示例11-16说明了是如何工作的。

{% code-tabs %}
{% code-tabs-item title="Example 11-16. Using real-time facades" %}
```php
namespace App;
class Charts {
    public function burndown() {
        // ...
    } 
}
<h2>Burndown Chart</h2>
{{ Facades\App\Charts::burndown() }}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

正如您在这里看到的，非静态方法burndown\(\)可以作为实时facade上的静态方法进行访问，我们通过在类的全名前面加上Facades\来创建它。

