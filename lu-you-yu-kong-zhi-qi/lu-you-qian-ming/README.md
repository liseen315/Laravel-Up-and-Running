# 路由签名

许多应用程序经常会发送一次性的通知例如\(重置密码,接收邀请等\),并为这些操作提供短链接,假设发送一封通知收件人加入邮件列表的电子邮件

有三种办法来发送这条链接:

1.将该URL公开,不把URL暴露给其他人或者修改URL来让其他人能通过验证

2.先进行身份认证,要求用户在没登录时进行登录操作\(这种情况基本是不可能的,因为收件人可能都不是用户\)

3.“签名”链接，以证明用户从邮箱中收到的链接,而不需要用户登录,例如,http://myapp.com/invita‐ tions/5816/yes?signature=030ab0ef6a8237bd86a8b8

最后一种办法可以使用Laravel5.6.12版本称为签名路由的特性完成,这样一来构建这种认证链接系统变的简单起来,这些链接是由普通链接附加了"签名"组成的，这种URL不会被篡改,（因此没有人能修改URL用来访问他人信息）



