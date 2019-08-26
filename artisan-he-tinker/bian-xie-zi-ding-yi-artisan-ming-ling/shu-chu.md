# 输出

在执行命令期间，您可能希望向用户写入消息。最基本的方法是使用$this-&gt;info\(\)输出基本的绿色文本。

```php
$this->info('Your command has run successfully.');
```

您还可以使用comment\(\)（橙色）、question\(\)（突出显示的青色）、error\(\)（突出显示的红色）和line\(\)（未着色）方法来显示到命令行。

请注意，确切的颜色可能因机器而异，但它们会尝试符合本地机器与最终用户通信的标准

**表格输出**

table\(\)方法用于创建ASCII表格输出，如示例8-9

{% code-tabs %}
{% code-tabs-item title="Example 8-9. Outputting tables with Artisan commands" %}
```php
$headers = ['Name', 'Email'];
$data = [
    ['Dhriti', 'dhriti@amrit.com'],
    ['Moses', 'moses@gutierez.com'],
];
// Or, you could get similar data from the database:
$data = App\User::all(['name', 'email'])->toArray();
$this->table($headers, $data);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

示例8-9,有两组数据，header和data,他们都包含了”列“和"行"，第一列代表名，第二列代表email，这样，来自Eloquent调用的数据（仅限于提取名称和电子邮件）与headers匹配。

查看8-10输出表格格式:

{% code-tabs %}
{% code-tabs-item title="Example 8-10. Sample output of an Artisan table" %}
```text
+---------+--------------------+
| Name    | Email              |
+---------+--------------------+
| Dhriti  | dhriti@amrit.com   |
| Moses   | moses@gutierez.com |
+---------+--------------------+
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**进度条**

如果你运行了npm install,你会看到一个进度条,让我们在示例8-11构建它。

{% code-tabs %}
{% code-tabs-item title="Example 8-11. Sample Artisan progress bar" %}
```php
$totalUnits = 10;
$this->output->progressStart($totalUnits);
for ($i = 0; $i < $totalUnits; $i++) { 
    sleep(1);
    $this->output->progressAdvance();
}
$this->output->progressFinish();
```
{% endcode-tabs-item %}
{% endcode-tabs %}

我们在这里做了什么？首先，我们通知系统需要多少“单元”来工作。也许一个单元是一个用户，而你有350个用户。然后，该条将其在屏幕上可用的整个宽度除以350，并在每次运行progressadvance\(\)时将其递增1/350。完成后，运行progressFinish\(\)，以便它知道已完成显示进度条。

