# 定义迁移

迁移是一个单独的文件,它定义了两件事:运行up迁移执行的修改,以及运行down迁移执行的修改.

> 迁移中的"up"和"down"
>
> 迁移总是按照时期运行,每个迁移文件的名字都类似这样:2014\_10\_12\_000000\_create\_users\_table.php，当一个新系统被迁移时,系统会抓取每个迁移,并运行它们的up方法-此时你正在”向上“执行迁移,迁移系统还允许你执行"回滚"最近的一组迁移,它将运行down方法,该方法将撤销up迁移所在的更改.
>
> 所以迁移的up应该叫执行迁移,down叫撤销迁移.

示例5-2显示了Laravel自带的"create users table"迁移.

{% code-tabs %}
{% code-tabs-item title="Example 5-2. Laravel’s default “create users table” migration" %}
```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateUsersTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('name');
            $table->string('email')->unique();
            $table->timestamp('email_verified_at')->nullable();
            $table->string('password');
            $table->rememberToken();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('users');
    }
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
邮箱验证

email\_verified\_at字段只出现在Laravel 5.7及之后的版本,它存储了用户验证邮箱地址的时间戳
{% endhint %}

现在我们有一个up方法告诉迁移帮我们创建一个名叫users的新表格,它带有一些字段,down方法告诉迁移丢弃users表格

