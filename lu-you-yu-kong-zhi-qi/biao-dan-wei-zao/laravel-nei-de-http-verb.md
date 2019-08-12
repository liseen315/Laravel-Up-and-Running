# Laravel内的HTTP verb

正如你已经了解过的,你可以使用Route::get\(\).Route::post\(\),Route::any\(\)或者是Route::match\(\)来定义路由verbs,你也可以使用Route::patch\(\),Route::put\(\)以及Route::delete\(\)

但是如何使用Web浏览器发送除GET之外的请求？ 首先，HTML表单中的method属性确定其HTTP动词：如果您的表单有一个“GET”方法，它将通过查询参数和GET方法提交; 如果表单有“POST”方法，它将通过帖子正文和POST方法提交

JavaScript框架可以轻松发送其他请求，如DELETE和PATCH。 但是，如果您发现自己需要使用GET或POST之外的动词在Laravel中提交HTML表单，则需要使用表单方法伪造.



