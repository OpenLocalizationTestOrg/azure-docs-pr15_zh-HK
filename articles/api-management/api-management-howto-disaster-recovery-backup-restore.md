<properties 
    pageTitle="如何实现灾难恢复使用服务备份和还原在 Azure API 管理 |Microsoft Azure" 
    description="了解如何使用备份和恢复可以在 Azure API 管理中执行灾难恢复。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>如何实现灾难恢复使用服务备份和还原在 Azure API 管理

通过选择要发布和管理您的 Api 通过 Azure API 管理却利用许多容错能力和基础结构的能力，否则必须设计、 实施和管理。 Azure 平台可减轻大部分潜在的故障成本的一小部分。

从可用性恢复影响您的 API 管理服务是在其中承载您的地区的问题应该准备好在任何时候重建您的服务在其他地区。 根据您的可用性目标和恢复时间目标可能需要保留一个或多个区域中的备份服务，然后试着保持其配置和内容与活动服务同步。 服务备份和还原功能用于实现您的灾难恢复战略提供必要的构建基块。

本指南介绍了如何 Azure 资源管理器请求进行身份验证以及如何备份和还原您的 API 管理服务实例。

>[AZURE.NOTE] 备份和还原 API 管理服务实例用于灾难恢复的过程也可以用于复制的方案，例如临时 API 管理服务实例。
>
>请注意，每个备份时到期后 7 天。 如果您尝试将备份还原后 7 天到期期限已过，恢复将失败，并`Cannot restore: backup expired`消息。

## <a name="authenticating-azure-resource-manager-requests"></a>请求身份验证的 Azure 资源管理器

>[AZURE.IMPORTANT] REST API，用于备份和恢复使用 Azure 资源管理器，并且具有比 REST Api 来管理您的 API 管理实体不同的身份验证机制。 本节中的步骤介绍如何 Azure 资源管理器请求进行身份验证。 有关详细信息，请参阅[身份验证 Azure 资源管理器的请求](http://msdn.microsoft.com/library/azure/dn790557.aspx)。

所有您使用 Azure 资源管理器的资源执行的任务必须使用 Azure Active Directory 使用以下步骤进行验证。

-   添加到 Azure Active Directory 租户应用程序。
-   为您添加应用程序设置权限。
-   身份验证请求到 Azure 资源管理器中获取令牌。

第一步是创建一个 Active Directory 的 Azure 应用程序。 登录到使用包含您的 API 管理服务实例订阅[Azure 经典门户](http://manage.windowsazure.com/)并导航到您的默认 Azure Active Directory**应用程序**选项卡。

>[AZURE.NOTE] 如果 Azure Active Directory 默认目录看不到您的帐户，则请联系 Azure 订阅的管理员授予您的帐户所需的权限。 查找您的默认目录的信息，请参阅[查找您的默认目录](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-portal)。

![创建 Azure 活动目录的应用程序][api-management-add-aad-application]

单击**添加****添加我的公司正在开发的应用程序**，然后选择**本机客户端应用程序**。 输入一个描述性名称，然后单击下一步的箭头。 如输入占位符 URL `http://resources` **重定向 URI**，因为它是必填的字段，但不是以后使用的值。 单击复选框，以保存应用程序。

保存应用程序后，单击**配置**，向下滚动到**到其他应用程序的权限**部分中，并单击**添加应用程序**。

![添加权限][api-management-aad-permissions-add]

选择**Windows Azure** **服务管理 API** ，然后单击添加应用程序复选框。

![添加权限][api-management-aad-permissions]

单击**委派权限**旁边新添加的**Windows Azure** **服务管理 API**应用程序、**访问 Azure 服务管理 （预览）**，请选中复选框，单击**保存**。

![添加权限][api-management-aad-delegated-permissions]

在调用生成备份和还原它的 Api 之前, 有必要以获取令牌。 下面的示例使用[Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget 程序包来检索令牌。

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace GetTokenResourceManagerRequests
    {
        class Program
        {
            static void Main(string[] args)
            {
                var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenant id}");
                var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

                if (result == null) {
                    throw new InvalidOperationException("Failed to obtain the JWT token");
                }

                Console.WriteLine(result.AccessToken);

                Console.ReadLine();
            }
        }
    }

更换`{tentand id}`， `{application id}`，和`{redirect uri}`使用以下说明。

更换`{tenant id}`租户 id 为您刚刚创建的 Azure Active Directory 应用程序。 您可以通过单击**查看终结点**访问 id。

![终结点][api-management-aad-default-directory]

![终结点][api-management-endpoint]

更换`{application id}`和`{redirect uri}`**重定向 Uri**部分从 Azure Active Directory 应用程序的**配置**选项卡中使用的**客户端 Id**和 URL。 

![资源][api-management-aad-resources]

一旦指定的值，该代码示例将返回一个标记类似于下面的示例。

![标记][api-management-arm-token]

在调用之前以下各节介绍的备份和恢复操作，设置针对 REST 调用授权请求标头。

    request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);

## <a name="step1"></a>备份 API 管理服务
若要备份 API 管理服务发出以下 HTTP 请求︰

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

其中︰

* `subscriptionId`-包含您正在尝试的 API 管理服务的订阅 id 到备份
* `resourceGroupName`-Api-默认-{服务地区} 窗体中的字符串在`service-region`标识 Azure 区域位置 API 管理服务，您试着承载备份，例如`North-Central-US`
* `serviceName`-API 管理服务的名称做的备份在其创建时指定
* `api-version`-替换`2014-02-14`

在请求的正文中，指定目标 Azure 存储帐户名称、 访问键，blob 容器名称和备份的名称︰

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

设置的值`Content-Type`请求标头为`application/json`。

备份是一项长时间运行操作，可能需要多分钟才能完成。  如果请求成功，并且已启动备份过程会收到`202 Accepted`响应状态代码与`Location`头。  使 GET 请求中的 URL 到`Location`标头以确定操作的状态。 在进行备份时您将继续收到 202 接受状态代码。 响应代码`200 OK`将指示备份操作成功完成。

**注意**︰

- 在请求正文**必须存在**指定**容器**。
* 在备份过程中您如 SKU**不应试图任何服务管理操作**升级或降级，更改域的名称，等等。 
* 自其创建的时刻还原的**备份可以保证仅为 7 天**。 
* **使用率数据**用于创建分析报告备份中**不包含**。 [Azure API 管理 REST API，][]用于定期检索安全保管的分析报告。
* 执行服务备份所采用的频率会影响您的恢复点目标。 若要最小化我们建议实施定期备份，以及对您的 API 管理服务进行重要更改后执行按需备份。
* 服务配置 （例如 Api、 策略、 开发人员门户的外观） 备份操作时所做的**更改**是在流程**可能不包括在备份中，因此将会丢失**。

## <a name="step2"></a>恢复 API 管理服务
若要恢复 API 管理服务从以前创建的备份进行以下 HTTP 请求︰

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

其中︰

* `subscriptionId`-包含您要还原到备份 API 管理服务的订阅 id
* `resourceGroupName`-Api-默认-{服务地区} 窗体中的字符串在`service-region`如标识要还原到备份 API 管理服务承载，Azure 区域`North-Central-US`
* `serviceName`-API 管理在其创建时指定要还原到的服务的名称
* `api-version`-替换`2014-02-14`

在请求的正文中，指定备份文件位置，即 Azure 存储帐户名、 访问键、 blob 容器名称和备份名称︰

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

设置的值`Content-Type`请求标头为`application/json`。

恢复是一项长时间运行操作，可能需要 30 个或更多分钟才能完成。  如果请求成功，并且已启动还原过程会收到`202 Accepted`响应状态代码与`Location`头。  使 GET 请求中的 URL 到`Location`标头以确定操作的状态。 在还原过程中您将继续收到 202 接受状态代码。 响应代码`200 OK`将指示还原操作成功完成。

>[AZURE.IMPORTANT] 服务的**SKU** **必须匹配**到还原正在还原的备份服务的 SKU。
>
>服务配置 （例如 Api、 策略、 开发人员门户的外观） 还原操作时所做的**更改**是进度**可能会覆盖**。

## <a name="next-steps"></a>下一步行动
请查看下面的 Microsoft 博客的备份/恢复过程的两个不同的演练。

-   [复制 Azure API 管理帐户](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/) 
    -   感谢您对 Gisela 她所占的这篇文章。
-   [Azure API 管理︰ 备份和还原配置](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
    -   由石详细的方法并不匹配的正式指导，但它是非常有趣。

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API 管理 REST API，]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
 
