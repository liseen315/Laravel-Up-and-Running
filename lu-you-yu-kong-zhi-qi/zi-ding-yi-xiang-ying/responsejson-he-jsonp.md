# response\(\)-&gt;json\(\) 和 -&gt;jsonp\(\)

要手动创建JSON编码的HTTP响应，请将JSON可访问的内容（数组、集合或其他任何内容）传递给json\(\)方法：例如`return response()->json(user::all())`。它和make\(\)一样，只是它+json\_encode+s您的内容并设置适当的头文件。

