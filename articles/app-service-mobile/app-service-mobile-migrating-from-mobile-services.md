<properties
    pageTitle="将移动服务迁移到移动应用程序的应用程序服务"
    description="了解如何轻松地迁移到移动应用程序的应用程序服务移动服务应用程序"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="adrianha"/>

# <a name="article-top"></a>将您现有的 Azure 移动服务迁移到 Azure 应用程序服务

与[常规的 Azure 应用程序服务的可用性]，Azure 移动服务站点可以方便地迁移就地利用 Azure 应用程序服务的所有功能。  本文档介绍如何在您的网站从 Azure 移动服务迁移到 Azure 应用程序服务时。

## <a name="what-does-migration-do"></a>迁移到您的网站做什么

Azure 移动服务的迁移将变成移动服务[Azure 应用程序服务]的应用程序而不会影响代码。  您通知集线器、 SQL 数据连接、 身份验证设置，已排定的作业和域的名称保持不变。  移动客户端使用 Azure 移动服务继续正常运作。  迁移后传输到 Azure 应用程序服务，将重新启动您的服务。

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>为什么应该迁移您的站点

Microsoft 正在推荐您迁移 Azure 移动服务利用 Azure 应用程序服务，包括的功能︰

  *  新主机功能，包括[WebJobs]和[自定义域名]。
  *  与您使用[VNet]除了[混合连接]的后端资源的连接。
  *  监控和故障诊断具有新的 Relic 或[应用程序的见解]。
  *  内置的 DevOps 工具，包括[临时插槽]，回退，并在生产测试。
  *  [自动缩放]、 负载平衡和[性能监控]。

有关好处 Azure 应用程序服务的详细信息，请参阅[移动服务与应用程序服务]主题。

## <a name="before-you-begin"></a>在开始之前

在开始之前您的站点的任何主要工作，您应该[备份您的移动服务]脚本和 SQL 数据库。

## <a name="migrating-site"></a>迁移网站

迁移过程将迁移单个 Azure 区域中的所有站点。

若要迁移您的网站︰

  1.  登录到[Azure 的传统门户网站]中。
  2.  选择想要迁移的区域中移动服务。
  3.  单击**迁移到服务应用程序**按钮。

    ![迁移按钮][0]

  4.  阅读迁移到应用程序服务对话框。
  5.  在提供的框中输入您的移动服务的名称。  例如，如果您的域名是 contoso.azure mobile.net，然后输入_contoso_在提供的框中。
  6.  单击刻度线按钮。

监视活动监视器中迁移的状态。 您的网站被列为 Azure 经典门户中*迁移*。

  ![迁移活动监视器][1]

每次迁移都将在 3 到 15 分钟，每个要迁移的移动服务。  在迁移期间，您的网站仍然可以使用。
迁移过程的末尾，您的站点将重新启动。  在重新启动过程中，可能会持续几秒钟，该站点将不可用。

## <a name="finalizing-migration"></a>完成迁移

若要测试您的网站从结束时在迁移过程中的移动客户端的计划。  请确保您可以执行所有常见的客户端操作，无需对移动客户端更改。  

### <a name="update-app-service-tier"></a>选择适当的应用程序服务定价层

必须更灵活地定价后将迁移到 Azure 应用程序服务。

  1.  登录到[Azure 的门户]。
  2.  选择**所有资源**或**应用程序服务**，然后单击您迁移的移动服务的名称。
  3.  默认情况下打开刀片式服务器的设置。
  4.  在设置菜单中，单击**应用程序服务计划**。
  5.  单击**定价层**平铺。
  6.  单击适用于您的要求，拼贴，然后单击**选择**。  您可能需要单击**所有视图**，以查看可用定价层。

作为第一步，我们建议以下层︰

| 移动服务定价层 | 应用程序的服务定价层 |
| :-------------------------- | :----------------------- |
| 免费                        | F1 自由                  |
| 基本                       | B1 基本                 |
| 标准                    | S1 标准              |

选择正确的价格为您的应用程序层处于相当大的灵活性。  请参阅[应用程序服务定价]详细介绍您的新应用程序服务的定价。

> [AZURE.TIP]应用程序服务标准层包含访问许多功能，您可以使用，包括[临时插槽]，自动备份，并自动调整。  了解新功能时有 ！

### <a name="review-migration-scheduler-jobs"></a>查看迁移调度程序作业

调度程序作业迁移后约 30 分钟才将不可见。  计划的作业，继续在后台运行。
之后他们仍然可以看到查看计划的作业︰

  1.  登录到[Azure 的门户]。
  2.  选择**浏览 >**，在_筛选器_框中，输入**计划**然后选择**计划程序集合**。

有数量有限的免费计划程序作业可用后迁移。  查看使用情况和[Azure 计划程序计划]。

### <a name="configure-cors"></a>如果需要，配置 CORS

跨原始资源共享是一种技术，允许网站访问另一域上 Web API。  如果与一个关联网站使用 Azure 移动服务，则需要配置 CORS 作为迁移过程的一部分。  如果要从移动设备中以独占方式访问 Azure 移动服务，然后不需要进行配置，除在极少数情况下 CORS。

作为**MS_CrossDomainWhitelist**的应用程序设置迁移的 CORS 设置不可用。  若要将网站迁移到应用程序服务 CORS 设施︰

  1.  登录到[Azure 的门户]。
  2.  选择**所有资源**或**应用程序服务**，然后单击您迁移的移动服务的名称。
  3.  默认情况下打开刀片式服务器的设置。
  4.  单击**CORS** API 菜单中。
  5.  在提供的框中，每个后按 enter 键输入任何允许的起源。
  6.  一旦允许来源列表正确，请单击保存按钮。

> [AZURE.TIP]使用 Azure 应用程序服务的优点之一是您可以在同一站点上运行您的网站和手机信息服务。  有关详细信息，请参阅[后续步骤](#next-steps)部分。

### <a name="download-publish-profile"></a>下载新的发布配置文件

更改您的站点的发布配置文件迁移到 Azure 应用程序服务。  如果要发布您的网站从 Visual Studio 中，您将需要一个新的发布配置文件。  若要下载新的发布配置文件︰

  1.  登录到[Azure 的门户]。
  2.  选择**所有资源**或**应用程序服务**，然后单击您迁移的移动服务的名称。
  3.  单击**获取发布配置文件**。

PublishSettings 文件将下载到您的计算机。  它通常被称为_站点名_。PublishSettings。  发布设置导入现有项目︰

  1.  打开 Visual Studio 和 Azure 手机服务项目。
  2.  右击**解决方案资源管理器**中的项目并选择**发布...**
  3.  单击**导入**
  4.  单击**浏览**并选择您下载发布设置文件。  单击**确定**
  5.  单击**验证连接**以确保发布设置工作。
  6.  单击**发布**来发布网站。


## <a name="working-with-your-site"></a>使用您的网站后迁移

开始使用您的新应用程序服务在[Azure 门户]后迁移。  以下是您用来在[Azure 传统门户网站]，以及对应的应用程序服务执行的特定操作中的一些注意事项。

### <a name="publishing-your-site"></a>下载和发布您迁移的站点

您的网站可通过 git 或 ftp，可以用各种不同的机制，包括 WebDeploy、 TFS，Mercurial，GitHub 和 FTP 重新发布。  部署凭据迁移与站点的其余部分。  如果您未设置部署凭据或不记住它们，您可以重置它们︰

  1. 登录到[Azure 的门户]。
  2. 选择**所有资源**或**应用程序服务**，然后单击您迁移的移动服务的名称。
  3. 默认情况下打开刀片式服务器的设置。
  4. 单击发布**部署凭据**菜单。
  5. 在提供的框中输入新的部署凭据，然后单击保存按钮。

这些凭据用于克隆使用 git 的站点或从 GitHub、 TFS 或 Mercurial 设置自动部署。  有关详细信息，请参阅[Azure 应用程序服务部署文档]。

### <a name="appsettings"></a>应用程序设置

可通过应用程序设置的大多数设置迁移的移动服务。  您可以将应用程序设置的从[Azure 的门户]。
若要查看或更改您的应用程序的设置︰

  1. 登录到[Azure 的门户]。
  2. 选择**所有资源**或**应用程序服务**，然后单击您迁移的移动服务的名称。
  3. 默认情况下打开刀片式服务器的设置。
  4. 在常规菜单中单击**应用程序设置**。
  5. 滚动到应用程序设置部分中，找到您的应用程序设置。
  6. 单击要编辑的值的应用程序设置的值。  单击**保存**以保存的值。

您可以同时更新多个应用程序设置。

> [AZURE.TIP]有两个具有相同值的应用程序设置。  例如，您可能会看到_ApplicationKey_和_MS\_ApplicationKey_。  在同一时间更新这两个应用程序设置。

### <a name="authentication"></a>身份验证

所有身份验证设置为应用程序设置迁移站点中不可用。  要更新您的身份验证设置，必须修改相应的应用程序设置。  下表显示了您的身份验证提供程序的相应应用程序设置︰

| 提供程序          | 客户机 ID                 | 客户端密码                | 其他设置             |
| :---------------- | :------------------------ | :--------------------------- | :------------------------- |
| Microsoft 帐户 | **MS\_MicrosoftClientID**  | **MS\_MicrosoftClientSecret** | **MS\_MicrosoftPackageSID** |
| Facebook          | **MS\_FacebookAppID**      | **MS\_FacebookAppSecret**     |                            |
| Twitter           | **MS\_TwitterConsumerKey** | **MS\_TwitterConsumerSecret** |                            |
| Google            | **MS\_GoogleClientID**     | **MS\_GoogleClientSecret**    |                            |
| Azure 的广告          | **MS\_AadClientID**        |                              | **MS\_AadTenants**          |

注︰ **MS\_AadTenants**存储为租户域 （移动服务门户中的"允许承租人"字段） 的列表，以逗号分隔。

> [AZURE.WARNING] **请在设置菜单中使用的身份验证机制**
>
> Azure 应用程序服务提供了在下的"无代码"身份验证和授权系统单独_身份验证 / 授权_设置菜单，并在设置菜单下的 （已弃用）_移动身份验证_选项。  这些选项是与迁移的 Azure 移动服务不兼容。  您可以[升级您的站点](app-service-mobile-net-upgrading-from-mobile-services.md)以充分利用 Azure 应用程序服务的身份验证。

### <a name="easytables"></a>数据

移动服务中的_数据_选项卡已更换_简单表_通过内 Azure 的门户。  若要访问简单表︰

  1. 登录到[Azure 的门户]。
  2. 选择**所有资源**或**应用程序服务**，然后单击您迁移的移动服务的名称。
  3. 默认情况下打开刀片式服务器的设置。
  4. 单击移动菜单中的**简单表**。

您可以通过单击**添加**按钮添加一个表或通过单击表名来访问您已有的表。  从该刀片式服务器，您可以执行的各种操作包括︰

* 更改表的权限
* 编辑操作脚本
* 管理表架构
* 删除表
* 清除表格内容
* 删除表中的特定行

### <a name="easyapis"></a>API

移动服务的_API_选项卡已更换由_简单的 Api_在 Azure 的门户。  若要访问简单的 Api:

  1. 登录到[Azure 的门户]。
  2. 选择**所有资源**或**应用程序服务**，然后单击您迁移的移动服务的名称。
  3. 默认情况下打开刀片式服务器的设置。
  4. 在移动菜单上单击**方便的 Api** 。

刀片式服务器中已列出您迁移的 Api。  此外可以从该刀片式服务器 API。  要管理特定的 API，请单击该 API。
从新的刀片，可以调整权限和编辑的脚本 api。

### <a name="on-demand-jobs"></a>调度程序作业

所有调度程序作业可通过调度程序作业集合中的节。  若要访问您的计划程序作业︰

  1. 登录到[Azure 的门户]。
  2. 选择**浏览 >**，在_筛选器_框中，输入**计划**然后选择**计划程序集合**。
  3. 选择为您的站点的作业集合。  它被命名为_站点名_的作业。
  4. 单击**设置**。
  5. 在管理下单击**调度程序作业**。

已排定的作业列出了在迁移之前先指定的频率。  禁用按需作业。  若要运行按需作业︰

  1. 选择您想要运行的作业。
  2. 如有必要，请单击**启用**以启用该作业。
  3. 单击**设置**，然后**计划**。
  4. 选择再次出现的**一次**，然后单击**保存**

按需作业位于`App_Data/config/scripts/scheduler post-migration`。  我们建议您将所有按需作业转换为[WebJobs]或[函数]。  编写新的调度程序作业，为[WebJobs]或[功能]。

### <a name="notification-hubs"></a>通知集线器

移动服务使用推式通知的通知集线器。  下面的应用程序设置用于在迁移后将通知中心链接到您的移动服务︰

| 应用程序设置                    | 说明                              |
| :------------------------------------- | :--------------------------------------- |
| **MS\_PushEntityNamespace**             | 通知中心 Namespace           |
| **MS\_NotificationHubName**             | 通知中心名称                |
| **MS\_NotificationHubConnectionString** | 通知中心连接字符串   |
| **MS\_命名空间名称**                   | MS_PushEntityNamespace 一个别名      |

通知中心通过[Azure 门户]管理。  请注意 （可以查找此使用的应用程序设置） 的通知中心名称︰

  1. 登录到[Azure 的门户]。
  2. 选择**浏览**>，然后选择**通知集线器**
  3. 单击通知中心名称与手机信息服务相关联。

> [AZURE.NOTE]如果通知中心是一种"混合"类型，则不可见。  "混合"键入通知集线器利用通知集线器和旧式的服务总线功能。  在继续之前的[转换混合的命名空间]。  转换完成后，通知中心位于[Azure 的门户]。

有关更多信息，请查看[通知集线器]文档。

> [AZURE.TIP]在[Azure 门户]通知集线器管理功能仍处于预览。  [Azure 传统门户网站]仍然可用于管理您的所有通知集线器。

### <a name="legacy-push"></a>传统的推设置

如果您之前通知集线器上推出移动服务上配置推送，您正在使用_传统推_。  如果您看不到您的配置中列出的通知中心正在使用推，那么很可能您使用的_传统的推_。  此功能与其他功能迁移。  但是，我们建议您在升级到通知集线器在迁移完成后不久。

在此期间，所有旧式强制设置 （APNS 证书的明显的例外） 都可用的应用程序设置中。  通过替换文件系统上的相应文件来更新 APNS 证书。

### <a name="app-settings"></a>其他的应用程序设置

以下附加的应用程序设置都迁移从您的移动服务，并可在*设置*下 > *的应用程序设置*︰

| 应用程序设置              | 说明                             |
| :------------------------------- | :-------------------------------------- |
| **MS\_MobileServiceName**         | 您的应用程序的名称                    |
| **MS\_MobileServiceDomainSuffix** | 域前缀。 例如 azure mobile.net |
| **MS\_ApplicationKey**            | 您的应用程序键                    |
| **MS\_主密钥**                 | 您的应用程序主要密钥                     |

应用程序键和主密钥是从原来的移动服务与应用程序的键相同。  特别是，移动客户端发送应用程序注册表项来验证他们的移动 API 的使用。

### <a name="cliequivalents"></a>命令行等效项

_Azure 移动_命令不再可用于管理 Azure 移动服务站点。  相反，许多功能已被替换为_azure 站点_命令中。  使用下表查找常用命令等效项︰

| _Azure 的移动_命令                     | 等效的_Azure 站点_命令            |
| :----------------------------------------- | :----------------------------------------- |
| 移动位置                           | 站点位置列表                         |
| 移动列表                                | 站点列表                                  |
| 移动的显示_名称_                         | 网站显示_名称_                           |
| 移动重启_名称_                      | 站点重启_名称_                        |
| 移动重新部署_名称_                     | 站点部署重新部署_commitId_ _名称_ |
| 设置移动的项的_名称__类型__值_       | 网站设置删除_键__名称_ <br/> 网站设置添加_键_=_值_的_名称_ |
| 移动配置列表_名称_                  | 网站设置列表_名称_                |
| 移动配置获取_名称__密钥_             | 网站设置显示_项__名称_          |
| 移动配置集_名称__密钥_             | 网站设置删除_键__名称_ <br/> 网站设置添加_键_=_值_的_名称_ |
| 移动域列表_名称_                  | 网站的域名列表_名称_                    |
| 移动域添加_名称__域_          | 网站域添加_域_的_名称_            |
| 移动域删除_名称_                | 网站域名删除_域__名称_         |
| 移动缩放显示_名称_                   | 网站显示_名称_                           |
| 移动缩放更改_名称_                 | 网站缩放模式_模式__名称_ <br /> 网站扩展实例的_实例__名称_ |
| 移动设置列表_名称_              | 网站设置列表_名称_                |
| 移动设置添加_名称__密钥__值_ | 网站设置添加_键_=_值_的_名称_   |
| 移动设置删除_名称__密钥_      | 网站设置删除_键__名称_        |
| 移动设置显示_名称__密钥_        | 网站设置删除_键__名称_        |

通过更新相应的应用程序设置更新身份验证或推式通知设置。
编辑文件，并将通过 ftp 或 git 网站发布。

### <a name="diagnostics"></a>诊断程序和日志记录

诊断日志记录通常是 Azure 应用程序服务中被禁止的。  要启用诊断日志记录︰

  1. 登录到[Azure 的门户]。
  2. 选择**所有资源**或**应用程序服务**，然后单击您迁移的移动服务的名称。
  3. 默认情况下打开刀片式服务器的设置。
  4. 在功能菜单下选择**诊断日志**。
  5. **单击以下日志**︰**应用程序记录 （文件系统）**、**详细的错误消息**和**失败请求跟踪**
  6. 单击**文件系统**用于 Web 服务器日志记录
  7. 单击**保存**

若要查看的日志︰

  1. 登录到[Azure 的门户]。
  2. 选择**所有资源**或**应用程序服务**，然后单击您迁移的移动服务的名称。
  3. 单击**工具**按钮
  4. 在遵守菜单下，选择**日志流**。

在生成日志显示在窗口中。  您还可以下载供以后分析使用部署证书的日志。 有关详细信息，请参阅[记录]文档。

## <a name="known-issues"></a>已知的问题

### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>删除迁移移动应用程序克隆会导致站点停机

如果克隆使用 Azure PowerShell 您迁移的移动服务，然后删除克隆，为生产服务的 DNS 条目被删除。  您的网站是不能通过 Internet 访问的。  

解决方案︰ 如果您想要克隆站点，这样通过门户。

### <a name="changing-webconfig-does-not-work"></a>更改 Web.config 不起作用

如果您的 ASP.NET 网站，变为`Web.config`文件执行不会得到应用。  Azure 应用程序服务生成一个合适`Web.config`文件以支持移动服务运行时的启动过程。  通过使用 XML 转换文件，您可以覆盖某些设置 （如自定义标头）。  创建一个文件在名为`applicationHost.xdt`-此文件必须以结束`D:\home\site`Azure 服务目录。  将上载`applicationHost.xdt`通过自定义部署脚本文件中，或者直接使用 Kudu。  以下代码显示了一个示例文档︰

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

有关详细信息，请参阅在 GitHub 上的[XDT 转换示例]文档。

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>迁移的移动服务不能添加到通讯管理器中

创建一个流量管理器的配置文件时，不能直接选择对该配置文件迁移的移动服务。  使用"外部端点"。  只能通过 PowerShell 添加外部端点。  有关详细信息，请参阅[通信量管理器教程](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/)。

## <a name="next-steps"></a>下一步行动

现在，您的应用程序迁移到应用程序服务，则有更多的功能，您可以使用︰

  * 部署[暂存槽]，可以转移到您的站点的更改并执行 A / B 测试。
  * [WebJobs]提供按需调度作业的替代。
  * 可以[连续地部署]您的网站通过将您的网站链接到 GitHub、 TFS 或 Mercurial。
  * 可以使用[应用程序的见解]来监视您的站点。
  * 提供网站和移动的 API 相同的代码。

### <a name="upgrading-your-site"></a>到 Azure 移动应用程序 SDK 升级您的移动服务站点

  * 对于 Node.js 基于服务器的项目，新的[移动应用程序 Node.js SDK]提供了几种新功能。 例如，可以现在做本地开发和调试、 使用上面 0.10，任何 Node.js 版本和任何 Express.js 中间件与自定义。

  * 有关。基于网络的服务器项目中，新的[移动应用程序 SDK NuGet 程序包](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/)NuGet 相关性有更大的灵活性。  这些软件包支持新的应用程序服务的身份验证，并在编写时使用任何 ASP.NET 项目。 要了解有关升级的更多信息，请参阅[升级您现有的.NET 移动服务对应用程序服务](app-service-mobile-net-upgrading-from-mobile-services.md)。

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[应用程序服务的定价]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[应用程序的见解]: ../application-insights/app-insights-overview.md
[自动缩放]: ../app-service-web/web-sites-scale.md
[Azure 应用程序服务]: ../app-service/app-service-value-prop-what-is.md
[Azure 应用程序服务部署文档]: ../app-service-web/web-sites-deploy.md
[Azure 的传统门户网站]: https://manage.windowsazure.com
[Azure 门户]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Azure 计划程序计划]: ../scheduler/scheduler-plans-billing.md
[连续部署]: ../app-service-web/app-service-continuous-deployment.md
[将混合命名空间]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[自定义域名]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[Azure 应用程序服务的一般可用性]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[混合连接]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[日志记录]: ../app-service-web/web-sites-enable-diagnostic-log.md
[移动应用程序 Node.js SDK]: https://github.com/azure/azure-mobile-apps-node
[移动服务与应用程序服务]: app-service-mobile-value-prop-migration-from-mobile-services.md
[通知集线器]: ../notification-hubs/notification-hubs-push-notification-overview.md
[性能监视]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[备份您的移动服务]: ../mobile-services/mobile-services-disaster-recovery.md
[临时的插槽]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[WebJobs]: ../app-service-web/websites-webjobs-resources.md
[XDT 转换示例]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[函数]: ../azure-functions/functions-overview.md
