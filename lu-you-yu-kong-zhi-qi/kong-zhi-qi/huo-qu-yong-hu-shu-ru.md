# 获取用户输入

在控制器方法中执行的第二个最常见的操作是从用户那里获取输入并对其进行操作。这引入了一些新概念，所以让我们来看一点示例代码，并浏览一下新的部分

{% code-tabs %}
{% code-tabs-item title="Example 3-25. Binding basic form actions" %}
```php
// routes/web.php
<?php

Route::get('tasks/create','TaskController@create');
Route::post('tasks','TaskController@store');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

我们绑定了tasks/create的GET动作\(用于显示新建任务表单\)和tasks的POST动作\(我们创建新任务会发送POST进行存储\),我们假设控制器内的create仅用于显示表单,那么我们看一下示例3-26的store方法：

{% code-tabs %}
{% code-tabs-item title="Example 3-26. Common form input controller method" %}
```php
// TasksController.php
...
public function store() {
    Task::create(request()->only(['title', 'description'])); 
    return redirect('tasks');
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这个例子使用了Eloquent模型以及redirect\(\)功能,我们将稍后讨论他们,现在让我们看是如何获取的数据

我们使用request\(\)辅助函数表示HTTP请求\(稍后讨论\),并使用only\(\)方法获取用户提交的"title","description"字段.

然后我们传递数据到Task模型的create方法内，同时创建了Task实例,最后我们重定向到了tasks页面.

这里有几个抽象层在工作，我们将在一秒钟内介绍这些抽象层，但是要知道only\(\)方法的数据来自同一个数据池,Request对象常用的方法包括all\(\),get\(\),这些方法的每个数据都是用户提供的,包括查询的或者是POST的.因此，我们的用户在“添加任务”页面上填写了两个字段：“标题”和“说明”。

稍微分解一下抽象的话是,request接收数组并返回数据

```php
request()->only(['title', 'description']); // returns:
[
    'title' => 'Whatever title the user typed on the previous page',
    'description' => 'Whatever description the user typed on the previous page',
]
```

然后Task::create\(\)用返回的数组数据创建一条新的任务

```php
 Task::create([
     'title' => 'Buy milk',
     'description' => 'Remember to check the expiration date this time, Norbert!',
]);
```

只需要用户提供“标题”和“描述”字段就可以创建一个任务



