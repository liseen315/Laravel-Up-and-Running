# ForgotPasswordController

ForgotPasswordController只是简单地引入SendsPasswordResetEmails特征。 它使用showLinkRequestForm\(\)方法显示auth.passwords.email表单，并使用sendResetLinkEmail\(\)方法处理该表单的POST。 您可以使用broker\(\)方法自定义代理

