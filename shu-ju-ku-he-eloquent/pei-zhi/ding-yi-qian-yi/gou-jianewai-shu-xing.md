# 构建额外属性

如前一节所示,字段的大多数属性例如长度都作为方法的第个参数设置,但是还有其他的一些属性,可以用链式的方式设置属性,例如电子邮件字段设置为nullable，并将其放到last\_name字段后

```php
Schema::table('users', function (Blueprint $table) { 
    $table->string('email')->nullable()->after('last_name');
});
// 译者注测试after报告mysql语法错误,暂时不知道原因
```

以下的方法用于给字段设置其他的属性.

**nullable\(\)**

允许给字段插入NULL值

**default\('default content'\)**

给字段设置默认值

**unsigned\(\)**

标记字段为无符号\(不是正或负,而是一个整数\)

**first\(\) \(MySQL only\)**

将字段放到首位

**after\(colName\) \(MySQL only\)**

将字段放于其他字段后

**unique\(\)**

给字段添加UNIQUE索引

**primary\(\)**

添加主键索引

**index\(\)**

添加基础索引

注意unique\(\), primary\(\), 和 index\(\)可以在构建上下文之外使用,稍微我们在介绍  
  
  
  


