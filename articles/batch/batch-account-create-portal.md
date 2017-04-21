<properties
    pageTitle="创建一个批处理 Azure 帐户 |Microsoft Azure"
    description="了解如何创建在 Azure 门户在云中运行大规模的并行工作负载中的 Azure 批帐户"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="marsma"/>

# <a name="create-an-azure-batch-account-using-the-azure-portal"></a>创建使用 Azure 门户 Azure 批帐户

> [AZURE.SELECTOR]
- [Azure 门户](batch-account-create-portal.md)
- [批次管理.NET](batch-management-dotnet.md)

了解如何创建在[Azure 门户]Azure 批帐户[azure_portal]，以及在何处查找重要帐户属性，如访问键和帐户的 Url。 我们还讨论了定价，并将 Azure 存储帐户链接到批帐户，以便您可以使用[应用程序的包](batch-application-packages.md)和[保持作业和任务输出](batch-task-output.md)的批处理。

## <a name="create-a-batch-account"></a>创建一个批处理帐户

1. 登录到[Azure 的门户网站][azure_portal]。

2. 单击**新建** > **计算** > **批次服务**。

    ![在市场上的批次][marketplace_portal]

3. 此时将显示**新批帐户**刀片式服务器。 请参阅项**到*e*下面有关刀片式服务器中的每个元素的说明。

    ![创建一个批处理帐户][account_portal]

    一。 **帐户名称**︰ 批帐户的唯一名称。 此名称必须是唯一 Azure 创建帐户 （请参见*位置*下面） 的区域内。 它可能只包含小写字符，数字，并且必须是 3-24 个字符长。

    b。 **订阅**︰ 要在其中创建批处理帐户订阅。 如果您只有一个订阅，它是默认选中的。

    c。 **资源组**︰ 现有资源组新批帐户，或选择创建一个新。

    d。 **位置**︰ 要在其中创建批处理帐户的 Azure 地区。 只受您的订购和资源组的区域显示为选项。

    电子。 **存储帐户**（可选）︰ 到新批帐户关联 （链接） 的**通用**存储帐户。 更多详细信息，请参阅下面的[链接 Azure 存储帐户](#linked-azure-storage-account)。

4. 单击**创建**来创建帐户。

  门户指示它是**部署**该帐户，并完成后，**已成功部署**的通知出现在*通知*。

## <a name="view-batch-account-properties"></a>查看批次帐户属性

一旦已经创建的帐户，您可以打开**批帐户刀片式服务器**以访问其属性和设置。 可以通过使用批处理帐户刀片式服务器左侧的菜单中访问所有帐户设置和属性。

![在 Azure 门户批帐户刀片式服务器][account_blade]

* **批次帐户 URL**︰ 使用[批次开发 Api](batch-technical-overview.md#batch-development-apis)创建的应用程序需要帐户 URL 来管理资源并在帐户下运行的作业。 批次帐户 URL 具有以下格式︰

    `https://<account_name>.<region>.batch.azure.com`

![在门户网站中的批处理帐户 URL][account_url]

* **访问键**︰ 您的应用程序还需要访问键时使用的批次科目中的资源。 若要查看或重新生成批次帐户的访问键，输入`keys`在批帐户刀片式服务器左侧的菜单**搜索**框中，然后选择**键**。

    ![在 Azure 门户批帐户密钥][account_keys]

## <a name="pricing"></a>定价

仅在"自由层中，"这意味着批帐户本身不收取提供了批处理帐户。 负责为批处理解决方案占用，底层 Azure 计算资源和所消耗的其他服务，您的工作负载运行时资源。 例如，进行计费的计算节点在您池和数据也存储在 Azure 存储输入或输出任务。 同样，如果您使用的批处理[应用程序数据包](batch-application-packages.md)功能，进行计费的用于存储应用程序软件包的 Azure 存储资源。 请参阅[批处理定价][batch_pricing]的详细信息。

## <a name="linked-azure-storage-account"></a>链接的 Azure 存储帐户

如前所述，您可以 （可选） 链接到批帐户的**通用**存储帐户。 批次的[应用程序包](batch-application-packages.md)功能使用 blob 存储链接的通用存储帐户，[批处理文件约定.NET](batch-task-output.md)库也一样。 这些可选的功能帮助您部署应用程序运行批处理任务，并保持它们产生的数据。

批当前支持*仅***通用**存储帐户类型[有关 Azure 存储帐户](../storage/storage-create-storage-account.md)[创建存储帐户](../storage/storage-create-storage-account.md#create-a-storage-account)，第 5 步中所述。 在 Azure 存储帐户链接到您的批处理帐户时，确保链接*只*是**通用**存储帐户。

![创建"一般用途"存储帐户][storage_account]

我们建议您创建批处理帐户供独占使用存储帐户。

>[AZURE.WARNING] 重新生成链接的存储帐户的访问键时要格外小心。 只有一个存储帐户密钥重新生成，然后单击链接的存储帐户刀片式服务器**同步密钥**。 等待 5 分钟，以便传播到池中，计算节点，然后重新生成并同步另一个密钥，如果有必要使用键。 如果您在同一时间再生两个键，计算节点将不能同步任何一项，和他们将失去对存储帐户的访问权限。

  ![重新生成存储帐户关键字][4]

## <a name="batch-service-quotas-and-limits"></a>批次服务配额和限制

请注意，如同 Azure 订购和其他 Azure 服务，某些[配额和限制](batch-quota-limit.md)适用于批帐户。 在帐户**属性**中的门户网站中显示批帐户的当前配额。

![在 Azure 门户批帐户配额][quotas]

设计和扩展您的批处理工作负载时，请记住这些配额。 例如，如果您的池不到达目标所指定的计算节点数，您可能已达到核心配额限制批帐户。

此外请注意，并不限制到单个批处理帐户为您 Azure 的订阅。 可以在单个批处理帐户，运行多个批处理工作负载或分发您批客户在相同的订阅中，但在 Azure 的不同地区之间的工作负载。

许多这些配额会增加只是使用 Azure 门户中提交一个免费的产品支持请求。 请求增加配额的详细信息，请参阅[配额和 Azure 批服务的限制](batch-quota-limit.md)。

## <a name="other-batch-account-management-options"></a>帐户的其他批次管理选项

除了使用 Azure 的门户网站，还可以创建和管理批用下面的帐户︰

* [批次 PowerShell cmdlet](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](../xplat-cli-install.md)
* [批次管理.NET](batch-management-dotnet.md)

## <a name="next-steps"></a>下一步行动

* [Azure 批处理功能概述](batch-api-basics.md)以了解有关批次服务概念和功能的详细信息，请参阅。 文章讨论主批资源如池、 计算节点、 作业和任务，并提供服务的功能，使大规模计算工作负载执行的概述。

* 学习开发使用[批处理.NET 客户端库](batch-dotnet-get-started.md)启用批次的应用程序的基础知识。 [介绍性文章](batch-dotnet-get-started.md)指导您运行应用程序，使用批处理服务多个计算节点，对执行工作负荷包括使用 Azure 存储为工作负荷文件转储和检索。

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "重新生成存储帐户关键字"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
