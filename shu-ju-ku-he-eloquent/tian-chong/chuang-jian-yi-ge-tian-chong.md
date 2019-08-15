# 创建一个填充

使用Artisan `make:seeder`命令创建一个填充

`php artisan make:seeder ContactsTableSeeder`

你将会在database/seeds目录下看到创建了一个ContactsTableSeeder类,在我们编辑它之前,先将其添加到DatabaseSeeder类中,如示例5-5所示,这样当我们运行seeders它也会运行.

{% code-tabs %}
{% code-tabs-item title="Example 5-5. Calling a custom seeder from DatabaseSeeder.php" %}
```php
<?php

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    /**
     * Seed the application's database.
     *
     * @return void
     */
    public function run()
    {
        $this->call(ContactsTableSeeder::class);
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

现在让我们编辑它,我们可以使用DB facade手动插入一条记录,如示例5-6

{% code-tabs %}
{% code-tabs-item title="Example 5-6. Inserting database records in a custom seeder" %}
```php
<?php

use Illuminate\Database\Seeder;

class ContactsTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        DB::table('contacts')->insert([
            'name' => 'Lupita Smith',
            'email' => 'lupita@gmail.com'
        ]);
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这样我们在数据库就获得了一条记录,这是一个好的开始,但对于实用性填充来说,你可能希望随机生成填充并多次插入对吧?Laravel提供了一个特性用于此问题

