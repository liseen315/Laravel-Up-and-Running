# ResetPasswordController

ResetPasswordController简单地引入ResetsPasswords特征。 此特征提供对基本密码重置视图的验证和访问，然后使用Laravel的PasswordBroker类的实例（或任何其他实现PasswordBroker接口的实例，如果您选择自己编写）来处理发送密码重置电子邮件并实际重置 密码。

就像我们已经介绍的其他特性一样，它处理显示重置密码视图（showResetForm\(\)显示auth.passwords.reset视图）和从该视图发送的POST请求（reset\(\)验证并发送相应的响应 ）。 resetPassword\(\)方法实际上重置了密码，您可以使用broker\(\)和带有guard\(\)的auth guard自定义代理

如果您对自定义任何此类行为感兴趣，只需重写要在控制器中自定义的特定方法。

