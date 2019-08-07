# HTTP verbs

最常见的HTTP verbs是GET和POST,随后有PUT和DELETE,还有HEAD,OPTIONS以及PATCH,有两个在平常web开发不常用的,TRACE和CONNECT

如下是简短的总结:

GET

请求资源\(或者资源列表\)

HEAD

请求GET响应的头版本信息

POST

创建资源

PUT

覆盖资源

PATCH

修改资源

DELETE

删除资源

OPTIONS

询问服务器当前URL哪种verbs允许,每个动作期望你使用特定的verb调用特定的url,你可以通过它了解每个verb的用途

表格3-1显示了一个资源控制器可用的动作

| Verb | URL | Controller method | Name | Description |
| :--- | :--- | :--- | :--- | :--- |
| GET | tasks | index\(\) | tasks.index | 显示所有任务 |
| GET | tasks/create | create\(\) | tasks.create | 显示创建任务表单 |
| POST | tasks | store\(\) | tasks.store | 从表单提交任务 |
| GET | tasks/{task} | show\(\) | tasks.show | 显示任务 |
| GET | tasks/{task}/edit | edit\(\) | tasks.edit | 编辑任务 |
| PUT/PATCH | tasks/{task} | update\(\) | tasks.update | 从编辑表单编辑任务 |
| DELETE | tasks/{task} | destroy\(\) | tasks.destroy | 删除一个任务 |

