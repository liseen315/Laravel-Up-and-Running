# 总结

在默认用户模型，create\_users\_table迁移，认证控制器和认证脚手架之间，Laravel提供了现成的完整用户身份验证系统。RegisterController处理用户注册，LoginController处理用户身份验证，ResetPasswordController和ForgotPasswordController处理密码重置，所有的这一切都有默认的属性和方法用于重写某些默认行为。

Auth facade 和auth\(\)全局函数用于方法当前用户\(auth\(\)-&gt;user\(\)\)并且使得检查用户是否登录变得简单\(auth\(\)-&gt;check\(\)和auth\(\)-&gt;guest\(\)\)。

Laravel还内置了一个授权系统，允许您定义特定的功能（创建联系人，访问加密后的页面）或定义用户与整个模型交互的策略。

您可以使用Gate facade，User类上的can\(\)和cannot\(\)方法，Blade中的@can和@cannot指令，控制器上的authorize\(\)方法或can中间件来检查授权。

