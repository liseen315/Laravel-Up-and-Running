# 修改路由可以访问签名链接

既然生成了签名链接,那么就需要防止未签名的访问,最简单的办法是使用签名中间件,如果它不在app/http/kernel.php中的$routemiddleware数组中，则应该在light\routing\middleware\validatesignature中

```php
Route::get('invitations/{invitation}/{answer}', 'InvitationController')
    ->name('invitations')
    ->middleware('signed');
```

如果你愿意,你可以调用请求对象的hasValidSignature\(\)方法,代替使用signed中间件

```php
class InvitationController
{
    public function __invoke(Invitation $invitation, $answer, Request $request)
    {
        if (!$request->hasValidSignature()) {
            abort(403);
        }
//
    }
}
```

