# Eloquent事件

无论您是否在倾听，每次发生某些操作时，Eloquent模型都会将事件触发到应用程序，如果您熟悉pub/sub，那么它是相同的模型（您将在第16章中进一步了解Laravel的整个事件系统）。

下面是创建新联系人时绑定侦听器的快速摘要。我们将在AppServiceProvider的boot\(\)方法中绑定它，并假设我们每次创建新联系人时都通知第三方服务

{% code-tabs %}
{% code-tabs-item title="Example 5-62. Binding a listener to an Eloquent event" %}
```php
class AppServiceProvider extends ServiceProvider
{
    public function boot()
    {
        $thirdPartyService = new SomeThirdPartyService;
        Contact::creating(function ($contact) use ($thirdPartyService) {
            try {
                $thirdPartyService->addContact($contact);
            } catch (Exception $e) {
                Log::error('Failed adding contact to ThirdPartyService; canceled.');
                return false; // Cancels Eloquent create()
            }
        });
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

我们可以在示例5-26中看到,首先我们使用Modelname::eventName\(\)作为方法,然后给它传递了个闭包，闭包获取要操作的模型实例，接着，我们需要在某个服务提供者中定义这个监听器。最后，如果返回false，操作将取消，并取消save\(\)或update\(\)。

Eloquent模型触发如下事件：

* creating
* created
* updating
* updated
* saving
* saved
* deleting
* deleted
* restoring
* restored
* retrieved

除了restoring 和 restored外基本都很容易理解,当你恢复软删除会触发这俩事件。creating 和 updating会触发saving.created 和 updated.会触发saved

retrieved在Laravel 5.5及以后有效

