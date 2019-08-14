# 修改字段

要修改字段,只需要把修改的代码放到Schema::table的闭包内,然后调用change\(\)方法

> 修改字段前需要安装的依赖
>
> 在你修改或者丢弃某些字段前,你需要运行 composer require doctrine/dbal

所以,如果我们有一个字段名为name然后长度是255,我们想要修改长度为100，我们需要像下面这么写代码

```text
Schema::table('users', function (Blueprint $table) { 
    $table->string('name', 100)->change();
});
```

如果我们想要调整字段中的属性，也是一样的。为了使字段为空，我们执行以下操作

```text
Schema::table('contacts', function (Blueprint $table) { 
    $table->string('deleted_at')->nullable()->change();
});
```

如下是我们如何重命名字段

```text
Schema::table('contacts', function (Blueprint $table) {
    $table->renameColumn('promoted', 'is_promoted');
});
```

丢弃字段

```text
Schema::table('contacts', function (Blueprint $table) {
    $table->dropColumn('votes');
});
```

> 在SQLite中一次修改多个字段
>
> 如果你使用SQLite想通过一条迁移闭包来修改或者丢弃多个字段，那么你会收到报错
>
> 在第12章中，我建议您将SQLite用于测试数据库，即使您使用的是更传统的数据库，也可将此视为测试的限制
>
> 然而你不需要为每个更改创建迁移,你只需要在up内调用多次Schema::table\(\)

```php
public function up() {
    Schema::table('contacts', function (Blueprint $table) {
        $table->dropColumn('is_promoted');
    });
    Schema::table('contacts', function (Blueprint $table) {
        $table->dropColumn('alternate_email');
    });
}
```

