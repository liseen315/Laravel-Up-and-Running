# 创建字段

要在表格内创建新字段,不论是在创建表格调用,还是在修改表格调用,都需要使用Blueprint的实例,它是闭包函数的参数.

```php
Schema::create('users', function (Blueprint $table) { 
    $table->string('name');
});
```

让我们看看Blueprint实例可用的方法,我将介绍他们在MySQL的工作方式,但是如果你用的是其他数据库,Laravel将使用等价的方式处理.

以下是Blueprint的方法:

**integer\(colName\), tinyInteger\(colName\), smallInteger\(colName\), mediumInteger\(colName\), bigInteger\(colName\)**

添加INTEGER字段,或者是其他形式的整型字段.

**string\(colName, length\)**

添加一条带可选长度的VARCHAR类型字段

**binary\(colName\)**

添加一个BLOB类型字段

**boolean\(colName\)**

添加一条BOOLEAN类型字段

**char\(colName, length\)**

添加一条带可选长度的CHAR字段

**datetime\(colName\)**

添加一个DATETIME字段

**decimal\(colName, precision, scale\)**

添加具有精度和小数位数的DECIMAL字段，例如，decimal\(“amount”,5,2\)指定精度为5，小数位数为2

**double\(colName, total digits, digits after decimal\)**

添加DOUBLE字段,例如double\('tolerance', 12, 8\)，指12位数字长度,8位在小数位右边.例如7204.05691739

**enum\(colName, \[choiceOne, choiceTwo\]\)**

添加一个带选项的ENUM字段

**float\(colName, precision, scale\)**

添加一个FLOAT字段

**json\(colName\) and jsonb\(colName\)**

添加一个JSON或者JSONB字段\(Laravel 5.1中只有TEXT字段\)

**text\(colName\), mediumText\(colName\), longText\(colName\)**

添加TEXT字段

**time\(colName\)**

添加TIME字段

**timestamp\(colName\)**

添加TIMESTAMP字段

**uuid\(colName\)**

添加UUID字段\(MySQL内是CHAR\(36\)\)

如下是特殊的Blueprint方法: 

**increments\(colName\) 和 bigIncrements\(colName\)**

添加无符号自增INTEGER 或者是BIG INTEGER主键

**timestamps\(\) and nullableTimestamps\(\)**

添加 created\_at 和 updated\_at 时间戳字段

**rememberToken\(\)**

添加一个remember\_token字段用于用户"remember me" 令牌

**softDeletes\(\)**

添加一个deleted\_at时间戳字段用于软删除

**morphs\(colName\)**

提供一个colName，添加一个整型 colName\_id以及一个字符串colName\_type，例如\(morphs\(tag\),添加整型tag\_id和字符串tag\_type\)，用于表之前的关系

  


  
  
  




