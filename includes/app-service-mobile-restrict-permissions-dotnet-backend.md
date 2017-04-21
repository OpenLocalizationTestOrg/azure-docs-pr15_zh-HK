
默认情况下，可以以匿名方式调用中移动应用程序端的 Api。 接下来，您需要限制对只有已通过身份验证的客户端的访问。  

+ **（通过门户） Node.js 后的端**︰  
    
    在您的移动应用程序的**设置**，单击**简单表**，然后选择表。 单击**更改权限**，选择**身份验证的访问**，对所有其他权限，然后单击**保存**。 

+ **.NET 的后端 (C#)**:  

    在服务器项目中，导航到**控制器** > **TodoItemController.cs**。 添加`[Authorize]` **TodoItemController**类中，特性，如下所示。 要限制只为特定方法的访问，还可以将此特性应用不仅仅与那些方法，而不是类。 重新发布服务器项目。


        [Authorize]
        public class TodoItemController : TableController<TodoItem>

+ **Node.js 后的端 （通过 Node.js 代码）** ︰  
    
    表访问需要身份验证，请向 Node.js 服务器脚本中添加以下行︰


        table.access = 'authenticated';

    有关详细信息，请参阅[如何︰ 对表进行访问要求身份验证](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth)。 若要了解如何从您的网站下载快速入门代码项目的信息，请参阅[如何︰ 下载使用 Git 的 Node.js 后端快速入门代码项目](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart)。

