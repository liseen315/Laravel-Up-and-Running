# DB facade的基础用法

在我们使用fluent方法进行复杂查询之前,让我们先看一下几个简单的查询器命令,DB facade用于查询器链接，也用于简单的原始查询，示例5-15

{% code-tabs %}
{% code-tabs-item title="Example 5-15. Sample raw SQL and query builder usage" %}
```php
// Basic statement
DB::statement('drop table users');

// Raw select, and parameter binding
DB::select('select * from contacts where validated = ?', [true]);

// Select using the fluent builder
$users = DB::table('users')->get();

// Joins and other complex calls
DB::table('users')
    ->join('contacts', function ($join) {
        $join->on('users.id', '=', 'contacts.user_id')
            ->where('contacts.type', 'donor');
    })->get();
```
{% endcode-tabs-item %}
{% endcode-tabs %}

