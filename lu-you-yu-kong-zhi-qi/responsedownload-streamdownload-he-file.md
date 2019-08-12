# response\(\)-&gt;download\(\), -&gt;streamDownload\(\), 和 -&gt;file\(\)

要发送文件供最终用户下载，请将SplFileInfo实例或字符串文件名传递给download\(\)，并使用下载文件名作为第二个可选参数：例如，return response\(\) -&gt;download\('file501751.pdf ','myFile.pdf'\)，它将发送一个文件位于file501751.pdf并重命名.

要在浏览器中显示相同的文件（如果它是PDF或图像或浏览器可以处理的其他内容），请使用response\(\)--&gt;file\(\)，它与response-&gt;download\(\)参数相同

如果您想从外部服务中获取某些内容作为下载而无需将其直接写入服务器的磁盘，则可以使用response\(\)-&gt;streamDownload\(\)来传输下载。 它接收一个闭包，闭包输出一串字符串,文件名,和一个可选的头数组; 见例3-46

{% code-tabs %}
{% code-tabs-item title="Example 3-46. Streaming downloads from external servers" %}
```php
return response()->streamDownload(function () {
    echo DocumentService::file('myFile')->getContent();
}, 'myFile.pdf');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

