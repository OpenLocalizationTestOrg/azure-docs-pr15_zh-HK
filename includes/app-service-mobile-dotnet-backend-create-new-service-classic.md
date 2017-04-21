1. 在[Azure 门户]登录。

2. 单击**新建 +** > **Web + 移动** > **移动应用程序**，然后提供移动应用程序端的名称。

3. **资源组**中，选择一个现有的资源组，或创建一个新 （为您的应用程序使用相同的名称）。 
 
    您可以选择另一个应用程序服务计划，或新建一个。 应用程序服务的详细信息计划，以及如何在不同的定价中创建一个新的计划层并在所需的位置，请参阅[Azure 应用程序服务计划深度探讨](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)。

4. **应用程序服务计划**中，默认的计划 （在[标准层](https://azure.microsoft.com/pricing/details/app-service/)） 处于选中状态。 您还可以选择不同的计划，或[新建一个](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan)。 应用程序服务计划的设置确定的[位置的功能，成本和计算资源](https://azure.microsoft.com/pricing/details/app-service/)与应用程序相关联。 

    该计划确定后，请单击**创建**。 这将创建移动应用程序的后端。 
    
6. 在为新的移动应用程序后端**设置**刀片式服务器，请单击**快速入门**> 客户端应用程序平台 >**连接数据库**。 

    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)

7. 在**添加数据连接**刀片式服务器，请单击**SQL 数据库** > **创建一个新数据库**，键入数据库**名称**，选择定价层，然后单击**服务器**。  您可以重新使用此新数据库。 如果您已在同一位置有一个数据库，您可以选择**使用现有的数据库**。 由于带宽成本和更高的延迟不被建议中不同位置的数据库使用。
 
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)

8. 在**新的服务器**刀片式服务器，在**服务器名称**字段中键入一个唯一的服务器名称，提供登录名和密码，请检查**允许 azure 服务来访问服务器**，并单击**确定**。 这将创建新的数据库。

9. 回到中**添加数据连接**刀片式服务器，单击**连接字符串**，键入您的数据库中，登录名和密码值，单击**确定**。 等待几分钟之前成功部署的数据库。

<!-- URLs. -->
[Azure 门户]: https://portal.azure.com/
