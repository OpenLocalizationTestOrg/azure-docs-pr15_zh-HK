<properties
    pageTitle="服务配额和限制批处理 |Microsoft Azure"
    description="了解默认 Azure 批配额、 限制和约束，以及如何申请配额增加"
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
    ms.topic="article"
    ms.date="09/10/2016"
    ms.author="marsma"/>

# <a name="quotas-and-limits-for-the-azure-batch-service"></a>配额和限制的 Azure 批服务

作为与其他 Azure 服务有限制对某些与批服务相关联的资源。 许多的这些限制是订阅或帐户级别通过 Azure 应用默认配额。 本文讨论了这些默认值，并可以请求配额的方式增加。

如果您计划要在批处理中运行生产工作负载，您可能需要增加一个或多个默认值高于配额。 如果您想要提升配额，您可以打开在线[客户支持请求](#increase-a-quota)，不收取任何费用。

>[AZURE.NOTE] 配额是信用限额，不能保证容量。 如果您有大规模的产能需求，请联系 Azure 的支持。

## <a name="subscription-quotas"></a>订阅配额
**资源**|**默认限制**|**最大限制**
---|---|---
批次每个地区每个订阅的帐户 | 1 | 50

## <a name="batch-account-quotas"></a>批次帐户配额
[AZURE.INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="other-limits"></a>其他限制
**资源**|**最大限制**
---|---
每个计算节点的[并发任务](batch-parallel-node-tasks.md) | 4 x 个节点核心
每个批处理帐户的[应用程序](batch-application-packages.md)        | 20
每个应用程序的应用程序包  | 40
应用程序包的大小 （每个）       | 约 195 GB<sup>1</sup>

<sup>1</sup>大块斑点大小的 azure 存储限制

## <a name="view-batch-quotas"></a>查看批次配额

在[Azure 门户]中查看批处理帐户配额[portal]。

1. 在门户中，选择**批次帐户**，然后选择您感兴趣的批次帐户。

2. 选择批帐户的菜单刀片式服务器上的**属性**

3. 属性刀片式服务器显示当前应用于批处理帐户**配额**

    ![批次帐户配额][account_quotas]

## <a name="increase-a-quota"></a>增加配额

请按照下面的步骤来申请配额增加使用[Azure 的门户网站][portal]。

1. 在门户网站的右上角选择门户的仪表板或问号 （**？**）**帮助 + 支持**平铺。

2. 选择**新的支持请求** > **基础知识**。

3. 在**基本知识**刀片式服务器︰

    一。 **问题类型** > **配额**

    b。 选择您的订购。

    c。 **配额类型** > **批次**

    d。 **支持计划** > **配额支持-包括**

    单击**下一步**。

4. 在**问题**刀片式服务器︰

    一。 选择根据您[对业务的影响]的**严重性**[support_sev]。

    b。 **详细信息**，指定每个您想要更改的配额、 批帐户名和新的限制。

    单击**下一步**。

5. 在**联系人信息**刀片式服务器︰

    一。 选择**首选联系方法**。

    b。 请验证并输入所需的联系人详细信息。

    单击**创建**以提交支持请求。

一旦您提交您的支持请求，Azure 支持将与您联系。 注意完成此请求可能需要最多 2 个工作日。

## <a name="related-topics"></a>相关的主题

* [创建使用 Azure 门户 Azure 批帐户](batch-account-create-portal.md)

* [Azure 批处理功能概述](batch-api-basics.md)

* [Azure 订阅和服务限制、 配额和约束](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
