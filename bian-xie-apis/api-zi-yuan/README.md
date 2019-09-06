# API 资源

如果你用的是Laravel 5.5之前版本，你可以跳过本节，然后移动到357页的"使用Laravel Passport 进行API 认证"。

在过去，我们在Laravel中开发API时遇到的首要挑战之一是如何转换数据。 最简单的API可以将Eloquent对象作为JSON返回，但很快大多数API的需求都会超出该结构。 我们应该如何将我们的Eloquent结果转换为正确的格式？ 如果我们想要嵌入其他资源，或者只是可选地，或者添加计算字段，或者隐藏API中的某些字段而不是其他JSON输出，该怎么办？ 特定于API的变换器是解决方案。

在Laravel 5.5及更高版本中，我们可以访问一个名为Eloquent API资源的功能，这些结构定义了如何将给定类的Eloquent对象（或Eloquent对象的集合）转换为API结果。 例如，您的Dog Eloquent模型现在有一个Dog资源，它负责将Dog的每个实例转换为适当的Dog形API响应对象

