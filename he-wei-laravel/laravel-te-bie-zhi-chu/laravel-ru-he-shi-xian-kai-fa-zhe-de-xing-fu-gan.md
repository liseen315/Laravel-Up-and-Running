# Laravel如何让开发者感到幸福

说让开发变的快乐是一回事,做又是另外一回事,可能会问框架怎么能让开发人员感到快乐或者不快乐呢,Laravle尝试使用如下方法让开发变得简单:

首先,Laravel是一个快速的应用程序开发框架。这意味着它专注于表层（简单）的学习曲线，并最大限度地减少启动新应用程序和发布应用程序之间的步骤。构建Web应用程序的所有最常见任务，如从数据库交互到身份验证，到队列，再到电子邮件到缓存，都可以通过Laravel提供的组件简化,但是Laravel组件本身并不是很好,它们在整个框架中提供了一致的API和可预测的结构,这就意味着当你在Laravel内尝试新事物时,你可能会问:"怎么就生效了"

不以框架本身为终点,Laravel为构建和启动应用程序提供了一个完整的工具生态系统.你可以使用Homestead和Valet进行本地开发.Forge管理服务以及Envoyer高级部署.另外还有一套附加软件包,支付跟订阅的Cashier、websockets的echo、搜索的Scout、API认证的Passport、用于前端测试的Dusk、社交登录的Socialite、监控队列的Horizon,Nova用于创建管理面板,Spark用于启动你的Saas,Laravel正试图从开发人员的工作中去掉重复的工作,这样他们就可以专注做自己的事.

接下来，Laravel将重点放在“约定优于配置”—也就是说，如果您愿意使用Laravel的默认值，那么与其他要求您声明所有配置的框架相比，您所做的工作也要少得多。在Laravel上构建的项目比在大多数其他PHP框架上构建的项目花费更少的时间

Laravel也非常注重简洁性,如果你愿意你可以在Laravel内使用依赖注入,mocking以及数据映射器模式和存储库、命令查询责任分离以及其他各种更复杂的架构模式.尽管其他框架可能建议在每个项目上使用这些工具和结构,Laravel文档以及社区倾向于最简单的实现-类似这里有个全局函数,那里有个是个Facecade,那是个ActiveRecord,这允许开发人员创建最简单的应用程序来满足他们的需求,而不是限制其在复杂环境中.

Laravel与其他PHP框架有一个有趣的不同，它的创建者及其社区与Java和Ruby和Rails编程语言联系紧密，并受到启发。在现代PHP中，更倾向于冗长和复杂，并拥抱了更多的JAVA风格。于此Laravel又不失动态跟简单的编程语言特性

