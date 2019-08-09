# 添加签名

为了给一个URL添加签名,需要给路由添加名字

```php
Route::get('invitations/{invitation}/{answer}', 'InvitationController')
    ->name('invitations');
```

要生成指向此路由的链接，您可以使用route\(\)辅助函数，也可以使用url facade执行相同的操作：`URL::route('invitations', ['invitation' => 12345, 'answer' => 'yes'])`。要生成指向此路由的签名链接，只需使用signedRoute\(\)方法即可。如果您想生成一个有时效的签名路由，请使用temporarySignedRoute\(\):

```php
// Generate a normal link
URL::route('invitations', ['invitation' => 12345, 'answer' => 'yes']); // Generate a signed link
URL::signedRoute('invitations', ['invitation' => 12345, 'answer' => 'yes']);
// Generate an expiring (temporary) signed link
URL::temporarySignedRoute(
    'invitations',
    now()->addHours(4),
    ['invitation' => 12345, 'answer' => 'yes']
);
```

> 使用now辅助函数
>
> Laravel5.5版提供了now辅助函数，它是相当于Carbon::now\(\)；它返回一个代表今天的carbon对象。如果你使用的是5.5之前的版本，则可以将本书中now\(\)替换为Carbon::now\(\)。

> Carbon它是一个包含在Laravel中的日期时间库。

