# 来自请求

注入请求对象（request facade和request\(\)助手）有几种方法可用于表示当前页面的URL状态，但现在让我们重点关注获取有关URL段的信息。

如果你对着不熟悉，那么URL域名之后的每一组字符都称为段。因此，[http://www.myapp.com/users/15/](http://www.myapp.com/users/15/有两个部分：用户和15) 有两个部分：用户，15

正如您可能猜测的那样，我们有两个可用的方法：$request-&gt;segments\(\)返回所有段的数组，$request-&gt;segment\($segmentid\)允许我们获取单个段的值。请注意，段是基于1的索引返回的，因此在前面的示例中，$request-&gt;segment\(1\)将返回用户。

请求对象、请求facade和request\(\)全局助手提供了更多的方法来帮助我们从URL中获取数据。要了解更多信息，请参阅第10章



