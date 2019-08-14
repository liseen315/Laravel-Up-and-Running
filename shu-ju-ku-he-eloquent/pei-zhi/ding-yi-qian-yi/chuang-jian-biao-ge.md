# 创建表格

我们已经看到默认的create\_users\_table迁移,它依赖于Schema facade以及它的方法,在这些迁移中我们所能做的都依赖于Schema的方法.

要创建一个表格,需要使用create\(\)方法，它的第一个参数定义了表格的名字,第个参数是个闭包函数,用于定义表格字段

```php
Schema::create('users', function (Blueprint $table) { 
    // Create columns here
});
```



