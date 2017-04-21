<properties
   pageTitle="Azure 自动化安全 |Microsoft Azure"
   description="本文概述了 Azure 自动化中的自动化帐户自动安全和可用的不同身份验证方法。"
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="自动化安全，安全自动化" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/29/2016"
   ms.author="magoedte" />

# <a name="azure-automation-security"></a>Azure 自动化安全
Azure 的自动化允许您自动完成任务的资源在 Azure，内部，并与其他云提供商 Amazon Web 服务 (AWS) 等。  Runbook 执行其所需的操作，它必须具有与内订阅所需的最小权限来安全地访问资源的权限。  
这篇文章将介绍由 Azure 自动化支持的各种身份验证方案，并将向您展示如何开始基于环境或环境管理所需。  

## <a name="automation-account-overview"></a>自动化客户概述
当您首次启动了 Azure 自动化时，您必须创建至少一个自动化帐户。 自动化帐户允许您隔离您的自动化资源 （运行手册、 资产配置） 从其他自动化帐户中包含的资源。 您可以使用自动化帐户分隔成单独的逻辑环境的资源。 例如，您可能的开发、 生产，另一个，另一个用于内部环境使用一个帐户。  Azure 自动化客户是与您的 Microsoft 客户或在 Azure 订阅创建帐户不同。

每个自动化客户的自动化资源与单个 Azure 区域，但自动化客户可以管理任何区域中的资源。 如果您有需要的数据和资源，以便能独立于特定区域的策略是在不同的区域中创建自动化客户的主要原因。

>[AZURE.NOTE]自动化客户和它们所包含的资源在 Azure 的门户网站中创建，Azure 的传统门户网站无法访问。 如果您想要管理这些帐户或它们与 Windows PowerShell 的资源，则必须使用 Azure 资源管理器模块。

根据资源在 Azure 自动化中使用 Azure 资源管理器和 Azure 的 cmdlet，您执行的任务的所有必须到 Azure 使用 Azure Active Directory 组织的基于凭据的身份验证。  基于证书的身份验证原始身份验证方法使用 Azure 服务管理模式，但它是复杂到安装程序。  使用 Azure AD 用户验证到 Azure 被引入的后在 2014年不仅简化了过程配置身份验证帐户，但也支持以非交互方式使用 Azure 资源管理器和经典资源与合作，以一个用户帐户验证到 Azure 的能力。   

当前在 Azure 门户中创建一个新的自动化帐户，它将自动创建︰

-  运行方式帐户在 Azure Active Directory 证书，创建新的服务主体和分配的参与者基于角色的访问控制 (RBAC)，将用于管理资源管理器资源使用运行手册。
-  传统运行方式帐户通过上传管理证书可用于管理 Azure 服务管理或使用运行手册的传统资源。  

基于角色的访问控制是使用 Azure 资源管理器可用来授予到 Azure AD 用户帐户和运行方式帐户允许的操作并进行身份验证的服务主体。  请阅读[Azure 自动化文章中基于角色的访问控制](../automation/automation-role-based-access-control.md)的详细信息以帮助开发用于管理自动化的权限模型。  

运行手册上混合 Runbook 工作人员在您的数据中心或针对计算中 AWS 服务运行不能使用相同的方法，通常用于运行手册对 Azure 的资源进行身份验证。  这是因为这些资源在 Azure 之外运行，因此，将需要对将本地访问的资源进行身份验证的自动化中定义他们自己的安全凭据。  

## <a name="authentication-methods"></a>身份验证方法

下表总结了 Azure 自动化和文章描述如何设置身份验证运行手册所支持的每个环境的不同的身份验证方法。

方法  |  环境  | 文章
----------|----------|----------
Azure AD 用户帐户 | Azure 的资源管理器和 Azure 服务管理 | [使用 Azure AD 用户帐户身份运行手册](../automation/automation-sec-configure-aduser-account.md)
Azure 运行方式帐户 | Azure 的资源管理器 | [使用 Azure 运行方式帐户身份运行手册](../automation/automation-sec-configure-azure-runas-account.md)
作为帐户的 azure 经典运行 | Azure 服务管理 | [使用 Azure 运行方式帐户身份运行手册](../automation/automation-sec-configure-azure-runas-account.md)
Windows 身份验证 | 内部数据中心 | [混合的 Runbook 工作人员的身份验证运行手册](../automation/automation-hybrid-runbook-worker.md)
AWS 凭据 | Amazon 的 Web 服务 | [身份验证与 Amazon 的 Web 服务 (AWS) 运行手册](../automation/automation-sec-configure-aws-account.md)



