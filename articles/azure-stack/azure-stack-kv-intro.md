<properties
    pageTitle="Azure 堆栈密钥存储库简介 |Microsoft Azure"
    description="了解如何 Azure 堆栈密钥存储库管理密钥和密码"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="introduction-to-key-vault-in-azure-stack"></a>Azure 堆栈中的密钥存储库简介 #

## <a name="before-you-start"></a>在开始之前

本文的假设如下︰

- 读取器可以访问已 Azure 密钥存储库已启用的订阅。
- Azure PowerShell SDK 是配置并可用。
- TP2 版本中，可以只从 PowerShell 执行所有租户面向操作。

## <a name="key-vault-basics"></a>密钥存储库基础知识

Azure 堆栈中的密钥存储库帮助保护加密密钥和云应用程序和服务的秘密使用。 通过使用密钥存储库，您可以加密密钥和机密信息 （例如身份验证密钥、 存储帐户密钥、 数据加密密钥，.pfx 文件和密码）。

密钥存储库简化了密钥管理进程，并使您可以控制访问和加密数据的密钥。 开发人员可以为开发和测试在几分钟内，创建注册表项并再无缝地迁移到生产键。 安全管理员可以授予 （和撤消） 键，如所需的权限。

订阅了 Azure 堆栈的任何人都可以创建并使用密钥存储库。 虽然好处密钥存储库的开发人员和安全管理员，它可以实现和管理的管理员来管理其他 Azure 堆栈服务组织。 例如，此管理员可以登录到订阅了 Azure 堆栈，创建用来存储密钥，并将负责这些操作任务的组织的电子仓库︰

- 创建或导入的密钥或密码
- 取消或删除项或秘密
- 授权用户或应用程序来访问密钥存储库，以便他们可以管理或使用它的键和机密信息
- 配置密钥用法 （例如，签名或加密）

该管理员可以向开发人员提供 Uri 可以从他们的应用程序，调用并为安全管理员提供密钥使用率日志记录信息。

开发人员还可以管理密钥直接使用 Api。 有关详细信息，请参阅密钥存储库开发人员指南 》。

## <a name="scenarios"></a>方案

下表描述了在某些情形下，密钥存储库可以帮助满足开发人员和安全管理员的需要。


### <a name="developer-for-an-azure-stack-application"></a>Azure 堆栈应用程序的开发人员

**问题**︰ 我想要编写一个应用程序的密钥用于签名和加密，Azure 堆栈但我想它们是外部应用程序中，以便解决方案适合于地理位置分散的应用程序。

**语句**︰ 在保险库中存储和调用 URI 时所需的密钥。


### <a name="developer-for-software-as-a-service-saas"></a>软件即服务 (SaaS) 的开发人员

**问题︰**我不想我客户的租户键和机密的责任或可能负有的责任。

**语句︰**客户可以将其自己的密钥导入到 Azure 堆栈，并对其进行管理。 我希望客户能够拥有并管理其密钥，以便我可以专注于做我需要做什么最好，它将提供核心软件功能。


### <a name="chief-security-officer-cso"></a>首席安全官员 (CSO)

**问题︰**我想确保我的组织控制密钥的生命周期，并可以监视密钥用法。

**语句**密钥存储库被设计，Microsoft 不会看到或提取您的密钥。  当应用程序需要使用客户的密钥执行的加密操作时，密钥存储库执行此操作为您的应用程序。 应用程序看不到客户的键。  尽管我们使用多个 Azure 堆栈中服务和资源，我想从 Azure 堆栈中的一个位置管理密钥。 该存储库提供一个单一的界面，而不管多少电子仓库了 Azure 堆栈，哪些区域中他们支持，以及使用它们的应用程序。

## <a name="next-steps"></a>下一步行动

[开始使用密钥存储库](azure-stack-kv-getting-started.md)
