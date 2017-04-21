<properties 
    pageTitle="公共云服务管理任务 （经典） |Microsoft Azure" 
    description="了解如何管理 Azure 的经典门户中的云服务。" 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/10/2016"
    ms.author="adegeo"/>





# <a name="how-to-manage-cloud-services"></a>如何管理云服务

> [AZURE.SELECTOR]
- [Azure 门户](cloud-services-how-to-manage-portal.md)
- [Azure 的传统门户网站](cloud-services-how-to-manage.md)

在 Azure 的传统门户网站**云服务**区域中，可以更新服务角色或部署、 升级分阶段的部署到生产环境、 将资源链接到云服务，以便您可以查看资源的依赖项和调节资源组合在一起，并删除云服务或部署。


## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>如何︰ 更新云服务角色或部署

如果您需要更新您的云服务的应用程序代码，使用仪表板、**云服务**页或**实例**页面上的**更新**。 您可以更新单个角色或所有角色。 您需要将新的服务包和服务配置文件上载。

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com/)，在仪表板、**云服务**页或**实例**页面上，单击**更新**。

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. 在**部署标签**，输入一个名称来标识部署 (例如，mycloudservice4)。 您可以在仪表板上找到下**快速入门**的部署标签。

3. 在**包**中，使用**浏览**上, 传服务软件包文件 (.cspkg)。

4. 在**配置**使用**浏览**上, 传服务配置文件 (.cscfg)。

5. 在**角色**中，选择**全部**，如果您想要升级在云服务中的所有角色。 若要执行单个角色更新，选择您要更新的角色。 即使您选择要更新的特定角色，服务配置文件中的更新应用于所有角色。

6. 如果更新更改角色的数量或大小的任何角色，请选择**如果角色大小或数量的角色发生变化，允许更新**复选框，以启用更新以继续。 

    请注意，如果您更改 （即虚拟机承载角色实例的大小） 的角色或角色的数量的大小，必须重新映像，每个角色实例 （虚拟机），本地的任何数据都将丢失。

7. 如果任何服务的角色具有一个角色实例，选择**更新，即使一个或多个角色中包含一个单实例复选框**以启用才能继续升级。 

    Azure 只能保证 99.95%的服务可用性云服务更新的过程中每个角色是否至少两个角色实例 （虚拟机）。 这样一个虚拟机来处理客户端请求，而其他正在更新。

8. 单击**确定**（复选标记） 以开始更新服务。



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>如何︰ 换部署宣传分阶段的部署到生产环境

使用**交换**提升临时部署某种云服务的生产。 当您决定部署云服务的新版本时，可以转移和云服务暂存环境中测试新的发行版，而您的客户在生产中使用的当前版本。 准备升级到生产的新版本时，可以使用**交换**切换依据解决两个部署的 Url。 

您可以交换**云服务**页或控制板中的部署。

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com/)中，请单击**云服务**。

2. 在云服务列表中，单击以选择它的云服务。

3. 单击**交换**。

    打开下面的确认提示。

    ![云服务交换](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. 在验证部署信息后，单击**是**可供换用的部署。

    部署交换速度很快因为仅更改了虚拟 IP 地址 (Vip) 的部署。

    为了节省计算成本，可以删除在暂存环境中的部署时确定新的生产部署按预期方式运行。

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>如何︰ 将资源链接到云服务

若要在其他资源中显示您的云服务的依存关系，可以将 SQL Azure 数据库实例或存储帐户链接到云服务。 可以链接和取消链接**链接的资源**页上的资源，然后监视它们云服务操控板上的用法。 如果监视打开链接的存储帐户，您可以云服务操控板上的监视请求的总数。

使用**链接**来链接到云服务的一个新的或现有 SQL 数据库实例或存储帐户。 然后可以扩展数据库和使用其**比例**页面的云服务角色。 （存储帐户缩放会自动随着使用量的增加。）有关详细信息，请参阅[如何扩展云服务和链接的资源](cloud-services-how-to-scale.md)。 

此外可以监视、 管理和扩展数据库在 Azure 的传统门户网站的**数据库**节点中。 

"链接"在这一点上的资源不会连接到资源的应用程序。 如果您创建的新数据库**链接**，您将需要添加到应用程序代码中的连接字符串，然后升级云服务。 您还需要添加的连接字符串，如果您的应用程序中链接的存储帐户使用的资源。

下面的过程描述如何将链接到云服务的新的 SQL 数据库服务器上部署的新 SQL 数据库实例。

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>若要链接到云服务的 SQL 数据库实例

1. 在[Azure 的传统门户网站](http://manage.windowsazure.com/)中，请单击**云服务**。 然后单击要打开的仪表板的云服务的名称。

2. 单击**链接的资源**。

    **链接的资源**页面将打开。

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. 单击**链接的资源**或**链接**。

    **链接资源**向导将启动。

    ![链接 Page1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. 单击**创建新的资源**或**链接现有的资源**。

5. 选择要链接资源的类型。 在[Azure 的传统门户网站](http://manage.windowsazure.com/)中，请单击**SQL 数据库**。 （预览 Azure 传统门户网站不支持将存储帐户链接到云服务。）

6. 要完成数据库配置，按照说明进行操作的**SQL 数据库**区域的帮助中 Azure 的传统门户网站。

    您可以按照在消息区域链接操作的进度。

    ![链接进度](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkProgress.png)

    完成链接后，您可以监视云服务操控板上的链接的资源的状态。 有关扩展链接的 SQL 数据库的信息，请参阅[如何扩展云服务和链接的资源](cloud-services-how-to-scale.md)。

### <a name="to-unlink-a-linked-resource"></a>要取消链接的资源

1. 在[Azure 的传统门户网站](http://manage.windowsazure.com/)中，请单击**云服务**。 然后单击要打开的仪表板的云服务的名称。

2. 单击**链接的资源**，然后选择该资源。

3. 单击**取消链接**。 然后单击**是**确认提示。

    取消链接 SQL 数据库具有对数据库或数据库应用程序的连接没有影响。 您仍可以管理 Azure 的传统门户网站的**SQL 数据库**区域中的数据库。



## <a name="how-to-delete-deployments-and-a-cloud-service"></a>如何︰ 删除部署和云服务

您可以删除云服务之前，您必须删除每个现有的部署。

为了节省计算成本，您可以验证生产部署工作正常后删除临时部署。 即使云服务未在运行，您可以将角色实例的记帐的计算成本。

使用以下过程删除部署或云服务。 

1. 在[Azure 的传统门户网站](http://manage.windowsazure.com/)中，请单击**云服务**。

2. 选择云服务，然后单击**删除**。 （要选择云服务而无需打开仪表板，请单击任意位置云服务条目中的名称除外。）

    如果必须在临时或生产的部署，您将看到类似于以下窗口底部的选项菜单。 您可以删除云服务之前，您必须删除任何现有的部署。

    ![删除菜单](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)


3. 若要删除部署，请单击**删除生产部署**或**删除临时部署**。 然后，看到确认提示时，单击**是**。 

4. 如果您打算删除云服务，请重复步骤 3，如果需要可以删除其他部署。

5. 若要删除云服务，请单击**删除云服务**。 然后，看到确认提示时，单击**是**。

> [AZURE.NOTE]
> 如果详细监视配置为您的云服务，Azure 不删除监视数据从您的存储帐户时删除云服务。 您将需要手动删除这些数据。 有关在何处查找度量表的信息，请参见"如何︰ 访问详细监视 Azure 经典门户外部数据"如何[监视云服务](cloud-services-how-to-monitor.md)。

## <a name="next-steps"></a>下一步行动

 * [云服务的常规配置](cloud-services-how-to-configure.md)。
* 了解如何[部署云服务](cloud-services-how-to-create-deploy.md)。
* 配置[自定义的域的名称](cloud-services-custom-domain-name.md)。
* 配置[ssl 证书](cloud-services-configure-ssl-certificate.md)。
