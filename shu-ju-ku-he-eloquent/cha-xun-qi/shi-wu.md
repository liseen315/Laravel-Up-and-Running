# 事物

如果您不熟悉数据库事务，那么它们是一个工具，它允许您将要执行的一系列数据库查询打包成一批，您可以选择回滚，撤消整个查询。事务通常用于确保执行一系列相关查询的全部而非部分，如果一个失败，ORM将回滚整个查询

Laravel查询事物特性是,如果在事物关闭前有异常,则事物中所有查询都将回滚,如果事物成功完成,则会提交所有查询并不回滚

让我们示例5-16事物的简单例子

```php
DB::transaction(function () use ($userId, $numVotes) { 
    // Possibly failing DB query
    DB::table('users')
        ->where('id', $userId)
        ->update(['votes' => $numVotes]);

    // Caching query that we don't want to run if the above query fails
    DB::table('votes')
        ->where('user_id', $userId)
        ->delete();
});
```

在这个例子中,我们假设之前进行了一些操作:汇总了用户的投票总数，我们希望缓存这个数字到用户表，然后从投票表清楚这些投票，但是，当然，在成功运行对用户表的更新之前，我们不希望清除投票。如果清除投票表失败，我们不想在用户表中保留更新的投票数。

如果两个查询中有任何问题，则不会应用另一个查询。这就是数据库事务的魔力。

请注意，您还可以手动开始和结束事务，这适用于查询器和Eloquent查询。以DB::beginTransaction\(\)开始，以DB::commit\(\)结束，并以DB::rollBack\(\)回滚

```php
DB::beginTransaction();
// Take database actions
if ($badThingsHappened) { 
    DB::rollBack();
}
// Take other database actions
DB::commit();
```

