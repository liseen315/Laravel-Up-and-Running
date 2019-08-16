# 原生SQL

如示例5-15，可以使用DB facade和statement\(\)方法来对数据库进行原生调用,DB::statement\('SQL statement here'\)

但是也有几个特殊的方法用于日常操作,select\(\),insert\(\),update\(\)和delete\(\)将返回对应的行,而statement\(\)就不会返回，其次使用这些方法，未来的开发人员将更清楚你正在生成哪种类型语句

**原生select**

最简单的DB方法是select\(\),你可以不用附加任何参数就可以运行它

`$users = DB::select('select * from users');`

它将返回stdClass对象集合。

> Illuminate 集合
>
> 在Laravel 5.3之前，DB facade为方法返回了一个stdClass对象 只返回一行\(如first\(\)\)和一个返回多行的数组\(如all\(\)\),Laravel 5.3之后,DB facade和Eloquent一样返回多行数据集合,DB facade 返回Illuminate\Support\Collection实例,Eloquent返回Illuminate \Database\Eloquent\Collection实例,该实例继承Illuminate\Support\Collection然后拥有一些Eloquentt特殊方法
>
> PHP集合就像一个拥有超级能力的数组,允许你在数据上运行map\(\), filter\(\), reduce\(\), each\(\),等等。你可以在17章了解更多关于集合信息

**参数绑定跟命名绑定**

Laravel的数据库体系结构允许使用PDO参数绑定，这可以保护您的查询免受潜在的SQL攻击。向语句传递参数非常简单，只需将语句中的值替换为？，然后将值添加到调用的第二个参数中

```php
$usersOfType = DB::select(
        'select * from users where type = ?',
        [$type]
);
```

为了清晰起见，还可以命名这些参数

```php
$usersOfType = DB::select(
        'select * from users where type = :type',
        ['type' => $userType]
);
```

**原生插入**

原生命令看起来像这样:

```php
DB::insert(
        'insert into contacts (name, email) values (?, ?)',
        ['sally', 'sally@me.com']
);
```

**原生更新**

原生更新如下:

```php
$countUpdated = DB::update(
        'update contacts set status = ? where id = ?',
        ['donor', $id]
);
```

**原生删除**

```php
$countDeleted = DB::delete(
    'delete from contacts where archived = ?', [true]
);
```

