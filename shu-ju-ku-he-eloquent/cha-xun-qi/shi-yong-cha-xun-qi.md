# 使用查询器

到目前为止,我们还没有真正的使用查询器,之前我们在DB facade上面调用了一些简单方法,让我们构建一些查询.

查询器将方法链接到一起从而构建查询，在链的末尾你可以使用一些方法\(可能是get\(\)\)来触发查询.

来让我们看一个简单的例子

```php
$usersOfType = DB::table('users')
        ->where('type', $type)
        ->get();
```

这里，我们创建了查询,查询users表格,查询条件是type,然后我们执行查询获取结果

让我们看下可用的链式方法有哪些,这些方法我称之为约束方法，修改方法，条件方法和结束/返回方法

**约束方法**

这些方法进行约束查询,并返回较小的数据子集

_select\(\)_

选择查询的列

```php
$emails = DB::table('contacts')
    ->select('email', 'email2 as second_email')
    ->get();
// Or
$emails = DB::table('contacts')
    ->select('email')
    ->addSelect('email2 as second_email')
    ->get();
```

_where\(\)_

你可以使用WHERE限制返回内容的范围，默认情况下,where\(\)接收3个参数，列,比较运算符,值

```php
$newContacts = DB::table('contact')
            ->where('created_at', '>', now()->subDay())
            ->get();
```

然而，如果第二个运算符是=，那么可以省略这个参数

`$vipContacts = DB::table('contacts')->where('vip',true)->get();`

如果要组合where\(\)语句,你可以再另外一个后面追加或者传递数组

```php
$newVips = DB::table('contacts')
            ->where('vip', true)
            ->where('created_at', '>', now()->subDay());
// Or
$newVips = DB::table('contacts')->where([['vip', true],
            ['created_at', '>', now()->subDay()],
        ]);
```

_orWhere\(\)_

创建一个简单的OR WHERE

```php
$priorityContacts = DB::table('contacts')
            ->where('vip', true)
            ->orWhere('created_at', '>', now()->subDay())
            ->get();
```

若要创建具有多个条件的更复杂OR WHERE语句,请给orWhere传递闭包

```php
$contacts = DB::table('contacts') 
    ->where('vip', true)
    ->orWhere(function ($query) {
        $query->where('created_at', '>', now()->subDay())
         ->where('trial', false);
    }) 
    ->get();
```

> 多个where\(\)和orWhere\(\)混合调用
>
> 如果将orWhere\(\)调用与多个where\(\)调用结合使用，则需要非常小心，以确保查询按您认为的方式进行。这并不是因为Laravel有任何错误，而是因为像下面这样的查询可能无法实现您期望的结果：

```php
$canEdit = DB::table('users')
            ->where('admin', true)
            ->orWhere('plan', 'premium')
            ->where('is_plan_owner', true)
            ->get();

SELECT * FROM users 
    WHERE admin = 1
    OR plan = 'premium' 
    AND is_plan_owner = 1;
```

> 如果要编写SQL，上面的示例中明确指出“if this or（this and this）”，则需要将闭包传递到orWhere\(\)中调用：

```php
$canEdit = DB::table('users')
            ->where('admin', true)
            ->orWhere(function ($query) {
                $query->where('plan', 'premium')
                    ->where('is_plan_owner', true);
            })
            ->get();
SELECT * FROM users 
    WHERE admin = 1
    OR (plan = 'premium' AND is_plan_owner = 1);
```

_whereBetween\(colName, \[low, high\]\)_

允许查询返回介于两个值之间的行\(包含两个值\)

```php
$mediumDrinks = DB::table('drinks')
            ->whereBetween('size', [6, 12])
            ->get();
```

同样适用于whereNotBetween\(\),但是结果会反转

_whereIn\(colName, \[1, 2, 3\]\)_

返回列表范围的行

```php
$closeBy = DB::table('contacts')
        ->whereIn('state', ['FL', 'GA', 'AL'])
        ->get();
```

同样适用于whereNotIn\(\)，结果会反转

_whereNull\(colName\) 和 whereNotNull\(colName\)_

查询NULL或者Not NULL

_whereRaw\(\)_

可以给where传递原生非转义字符串

```php
$goofs = DB::table('contacts')->whereRaw('id = 12345')->get()
```

> 小心SQL注入！
>
> 传递给whereraw\(\)的任何SQL查询都不会被转义。小心使用此方法；这是应用程序中SQL注入攻击的主要机会。

_whereExists\(\)_

允许你传递子查询,并至少返回一条记录,假设你想获取至少有一条留言的用户

```php
$commenters = DB::table('users')
            ->whereExists(function ($query) {
                $query->select('id')
                ->from('comments')
                ->whereRaw('comments.user_id = users.id');
            })
            ->get();
```

_distinct\(\)_

返回与其他记录比较后的记录,通常与select\(\)搭配使用,因为如果你使用了主键,则不会有重复的记录

```php
$lastNames = DB::table('contacts')->select('city')->distinct()->get();
```

**修改方法**

这些方法更改了查询结果,不仅仅是限制了结果

_orderBy\(colName, direction\)_

查询结果排序,第二个参数可以是asc（默认值，升序）或desc（降序）

```php
 $contacts = DB::table('contacts')
            ->orderBy('last_name', 'asc')
            ->get();
```

_groupBy\(\) 和 having\(\) 或者 havingRaw\(\)_

将结果按列分组。或者，having（）和havingraw（）允许您根据组的属性筛选结果。例如，您可以只查找人口至少为30人的城市

```php
$populousCities = DB::table('contacts')
            ->groupBy('city')
            ->havingRaw('count(contact_id) > 30')
            ->get();
```

_skip\(\)和take\(\)_

最常用于分页，它们允许您定义返回多少行，以及在开始返回之前跳过多少行，就像分页系统中的页码和页面大小一样。

```php
// returns rows 31-40
$page4 = DB::table('contacts')->skip(30)->take(10)->get();
```

_latest\(colName\)和oldest\(colName\)_

通过传递的列进行升序\(oldest\)或者降序\(latest\)排列,如果没传值则按照created\_at排列

_inRandomOrder\(\)_

随机排序结果

**条件方法**

Laravel 5.2以及更高版本有两个方法,可以通过传递值的布尔状态对内容进行条件判断（需要传递个闭包）

_when\(\)_

第一个参数传"真"则会执行闭包内的查询,第一个参数传"假"那么什么都不做,注意第一个参数应该是布尔\(true,false\),一个可选值（$status，从用户输入中提取，并默认为null）或返回一个值的闭包,重要的是要返回true或者false,例如

```php
$status = request('status'); // Defaults to null if not set
$posts = DB::table('posts')
    ->when($status, function ($query) use ($status) {
        return $query->where('status', $status); })
    ->get();

$posts = DB::table('posts')
    ->when($ignoreDrafts, function ($query) { 
        return $query->where('draft', false);
    }) ->get();
```

您还可以传递第三个参数，即另一个闭包，只有在第一个参数_false_时才会应用该闭包。

_unless\(\)_

与when相反,如果第一个参数为false,则会运行第二个闭包

**结束/返回方法**

终止查询链并触发SQL查询,如果在查询链没有调用这些方法,那么你将得到一个查询器实例，如果在查询链上执行了这些方法,那么你将得到具体的记录.

_get\(\)_

获取查询器的所有结果

```php
$contacts = DB::table('contacts')->get();
$vipContacts = DB::table('contacts')->where('vip', true)->get();
```

_first\(\)和firstOrFail\(\)_

获取第一条结果,就像Like，但是它只返回一条

```php
$newestContact = DB::table('contacts')
            ->orderBy('created_at', 'desc')
            ->first();
```

如果没有结果，first\(\)将自动失败，而firstorfail\(\)将引发异常。

如果将列名数组传递给任一方法，它将只返回这些列的数据，而不是所有列的数据。

_find\(id\)和findOrFail\(id\)_

与first\(\)类似,传递一个与主键对应的ID进行查找,如果ID对应的不存在,find会失败,而findOrFail会异常

```php
$contactFive = DB::table('contacts')->find(5);
```

_value\(\)_

从第一行提取单个字段,像first\(\)但是你只想获取单个列

```php
$newestContactEmail = DB::table('contacts')
            ->orderBy('created_at', 'desc')
            ->value('email');
```

_count\(\)_

返回所有匹配结果的计数

```php
$countVips = DB::table('contacts')
    ->where('vip', true)
    ->count();
```

_min\(\)和max\(\)_

返回特定列的最小或最大值

```php
$highestCost = DB::table('orders')->max('amount');
```

_sum\(\)和avg\(\)_

返回特定列中所有值的总和或平均值

```php
$averageCost = DB::table('orders')
            ->where('status', 'completed')
            ->avg('amount');
```

**使用DB::raw在查询器中写原生查询**

之前看到过一些方法用于原生语法-例如select有一个对应的selectRaw方法可以给其传递字符串用于查询,然后放到WHERE语法后

当然你也可以调用DB::raw\(\)来实现相同的结果

```php
$contacts = DB::table('contacts')
        ->select(DB::raw('*, (score * 100) AS integer_score'))
        ->get();
```

**连接**

定义联合查询有时有点困难,但是Laravel的查询器做的比较好,如例子

```php
$users = DB::table('users')
        ->join('contacts', 'users.id', '=', 'contacts.user_id')
        ->select('users.*', 'contacts.name', 'contacts.status')
        ->get();
```

join\(\)创建了一个连接查询,你可以用链式写法组合多个join，或者使用leftJoin来创建左连接

最后，你也可以给join传递闭包创建更复杂的连接查询

```php
DB::table('users')
            ->join('contacts', function ($join) {
                $join
                    ->on('users.id', '=', 'contacts.user_id')
                    ->orOn('users.id', '=', 'contacts.proxy_user_id');
            })->get();
```

**组合**

通过首先创建两个查询，然后使用union\(\)或unionall\(\)方法，可以将它们联合起来（将它们的结果组合成一个结果）

```php
$first = DB::table('contacts')
        ->whereNull('first_name');
        
$contacts = DB::table('contacts')
    ->whereNull('last_name')
    ->union($first)
    ->get();
```

**插入**

插入非常简单,传递一个数组插入单条记录,传递数组的数组,插入多条记录,使用insertGetId\(\)替代insert\(\)可以自动生成主键

```php
$id = DB::table('contacts')->insertGetId([
        'name' => 'Abe Thomas',
        'email' => 'athomas1987@gmail.com',
]);
DB::table('contacts')->insert([
        ['name' => 'Tamika Johnson', 'email' => 'tamikaj@gmail.com'],
        ['name' => 'Jim Patterson', 'email' => 'james.patterson@hotmail.com'],
]);
```

**更新**

更新也非常简单,创建更新查询只需要将数组传递给它

```php
DB::table('contacts')
        ->where('points', '>', 100)
        ->update(['status' => 'vip']);
```

还可以使用increment\(\)和decrement\(\)方法快速增加和减少列。第一个参数是列名，第二个（可选）是要递增/递减的数字

```php
DB::table('contacts')->increment('tokens', 5);
DB::table('contacts')->decrement('tokens');
```

**删除**

删除更简单,在查询结尾调用delete\(\)即可

```php
DB::table('users')
        ->where('last_login', '<', now()->subYear())
        ->delete();
```

您还可以截断表，该表将删除每一行并重置自增ID。

**JSON操作**

如果你有一个JSON列,你可以使用箭头语法来更新或选择JSON结构

```php
// Select all records where the "isAdmin" property of the "options"
// JSON column is set to true
DB::table('users')->where('options->isAdmin', true)->get();
        
// Update all records, setting the "verified" property
// of the "options" JSON column to true
DB::table('users')->update(['options->isVerified', true]);
```

