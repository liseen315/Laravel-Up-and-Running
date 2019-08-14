# 索引和外键

我们已经介绍了如何创建,修改和删除字段,让我们接着介绍索引和如何关联表

如果你不熟悉索引,不用索引的话,也可以正常运行,但是他们对于提升性能有重要作用,我建议你读一下这部分内容,但是你觉得暂时没必要的话,你可以跳过这部分

**添加索引**

例子5-3显示了如何给字段添加索引

{% code-tabs %}
{% code-tabs-item title="Example 5-3. Adding column indexes in migrations" %}
```text
// After columns are created...
$table->primary('primary_id'); // Primary key; unnecessary if used increments() 
$table->primary(['first_name', 'last_name']); // Composite keys 
$table->unique('email'); // Unique index
$table->unique('email', 'optional_custom_index_name'); // Unique index 
$table->index('amount'); // Basic index
$table->index('amount', 'optional_custom_index_name'); // Basic index
```
{% endcode-tabs-item %}
{% endcode-tabs %}

注意第一个例子,primary并不是必须的,如果你使用的是increments\(\) or bigIncrements\(\)来创建索引,那么会自动添加自增主键

**删除索引**

示例5-4显示如何删除索引

{% code-tabs %}
{% code-tabs-item title="Example 5-4. Removing column indexes in migrations" %}
```text
$table->dropPrimary('contacts_id_primary');
$table->dropUnique('contacts_email_unique');
$table->dropIndex('optional_custom_index_name');
// If you pass an array of column names to dropIndex, it will // guess the index names for you based on the generation rules 
$table->dropIndex(['email', 'amount']);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**添加和移除外键**

在Laravel内给字段添加外键是很容易的.

`$table->foreign('user_id')->references('id')->on('users');`  


这里我们给user\_id添加了外键,来引用user表的id字段

如果我们想指定外键约束，也可以使用onDelete\(\)和onUpdate\(\)来实现。例如

```php
$table->foreign('user_id')
        ->references('id')
        ->on('users')
        ->onDelete('cascade')
```

要删除一个外键，我们可以通过引用它的索引名来删除它（索引名是通过组合被引用的列和表的名称自动生成的）。

`$table->dropForeign('contacts_user_id_foreign');`

或者将它在本地表中引用的字段数组传递给它

`$table->dropForeign(['user_id']);`

