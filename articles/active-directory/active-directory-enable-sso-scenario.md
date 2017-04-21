<properties
    pageTitle="管理应用程序使用 Azure 的活动目录 |Microsoft Azure"
    description="此文章将 Azure Active Directory 集成与内部、 云和 SaaS 应用程序的好处。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="10/10/2016"
      ms.author="markvi"/>

# <a name="managing-applications-with-azure-active-directory"></a>管理应用程序使用 Azure 的活动目录

超越实际的工作流或内容中，公司都有为所有应用程序的两个基本要求︰

1. 为了提高工作效率，应用程序应该很容易发现和访问

2. 若要启用安全性和公司治理，组织需要控制以及谁可以和实际访问每个应用程序上的监督

在云应用程序的世界中这可以最好地实现使用控制"*谁可以做什么*"的身份。

在计算术语︰

- *谁*被称为*标识*的用户和组的管理

- *什么*被称为*访问管理*— 对受保护资源的访问权限的管理

这两个组件一起被称为*身份和访问管理 (IAM)*，由[Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam)的组定义"*安全训练科目，使适当的人能够访问适当的资源以适当时间出于正确的原因*"。

好，那么问题是什么？ 如果 IAM*未管理*在一个集成的解决方案︰

- 标识管理员必须单独创建和更新所有应用程序中的用户帐户分开，冗余和耗时的活动。

- 用户不必记住多个凭据以访问他们需要使用的应用程序。 结果是，用户往往写下自己的密码，或使用其他密码管理解决方案，这会带来其他数据安全风险。

- 冗余，耗时活动减少的时间用户和管理员在从事业务活动，提高企业的利润。

因此，什么通常可以防止采用集成的 IAM 解决方案的组织？

- 大多数技术解决方案根据需要进行部署和调整自己的应用程序的每个组织的软件平台。

- 云应用程序通常采用以较高的速率不是 IT 部门可以集成现有的 IAM 解决方案中。

- 安全和监视工具需要额外的自定义和集成，以实现全面 E2E 方案。

## <a name="azure-active-directory-integrated-with-applications"></a>Azure 的 Active Directory 集成与应用

Azure 的 Active Directory 是 Microsoft 的综合性的身份，作为服务 (IDaaS) 的解决方案中︰

- 可以用作云服务的 IAM 

- 提供单点登录 (SSO) 和报告的中心访问管理 

- 支持集成在应用程序库中，包括销售、 Google 应用程序，框，Concur，等[数千个应用程序](https://azure.microsoft.com/marketplace/active-directory/)的访问管理。 


使用 Azure Active Directory，对合作伙伴发布的所有应用程序和客户 （企业或消费者） 具有相同的标识和访问管理功能。<br> 这使您可以大大降低了运营成本。

如果您需要实现的应用程序未在应用程序库中列出？ 虽然这是有点更耗时比配置 SSO 应用程序从应用程序库，Azure 的广告提供了可以帮助您完成配置向导。

Azure 广告的价值超出"只是"云应用程序。 您还可以使用它与内部部署的应用程序通过提供安全的远程访问。 提供安全的远程访问，您可以消除对 Vpn 或其他传统的远程访问管理实现的需要。

通过为所有应用程序提供集中访问管理和单一登录 (SSO)，Azure AD 提供主数据安全性和工作效率问题的解决方案。

- 用户可以访问多个应用程序提供一个登录到收入完成的生成或业务运营活动给予更多的时间。

- 标识管理员可以管理应用程序在一个位置访问。

对用户和公司的好处是显而易见的。 让我们看一下好处标识管理员和组织。

## <a name="integrated-application-benefits"></a>集成的应用程序带来好处

SSO 过程包括两个步骤︰

- 身份验证，验证用户的身份的过程。

- 身份验证，启用或阻止访问资源访问策略的决策。

当使用 Azure 的广告来管理应用程序并启用 SSO:

- 对用户的部署 （如广告） 或 Azure AD 帐户进行身份验证。

- 对 Azure 的广告分配和保护策略确保一致的最终用户体验并使您能够添加任何应用程序，而不考虑其内部功能上的分配、 位置和 MFA 条件执行授权。

一定要了解授权的方式制订目标应用程序上它取决于如何使用 Azure AD 集成应用程序。

- **服务提供商预先集成的应用程序**如 Office 365 和 Azure，这些是直接基于 Azure AD 和依赖它为其全面的身份和访问管理功能的应用程序。 通过目录信息和令牌发放启用了对这些应用程序的访问。

- **由 Microsoft 和自定义应用程序预先集成的应用程序**这些都是独立的云应用程序依赖于内部应用程序目录并独立于 Azure 广告可以运行。 通过发出应用程序的特定凭据映射到应用程序帐户启用了对这些应用程序的访问。 根据应用程序功能，凭据可能联合身份验证令牌或用户名和以前的应用程序中设置的帐户的密码。

- **内部部署的应用程序**应用程序发布到 Azure 的广告应用程序代理，主要能够访问内部应用程序。 这些应用程序依赖于像 Windows 服务器的 Active Directory 部署目录上主办。 通过触发该代理启用访问这些应用同时考虑内部登录要求向最终用户交付应用程序内容。

例如，如果用户加入您的组织时，您需要在 Azure 广告主登录操作中创建的用户帐户。 如果此用户需要访问一个托管的应用程序，如队伍，也需要在销售队伍中创建此用户帐户并将其链接到 Azure 帐户以使 SSO 正常工作。 当用户离开您的组织时，最好删除 AD Azure 帐户和用户有权访问的应用程序存储 IAM 中的所有对应的帐户。

## <a name="access-detection"></a>自动检测

在现代企业中，IT 部门人员通常不了解所有的云应用程序正在使用。 结合云应用程序发现，Azure 的广告为您提供一个解决方案，可以检测到这些应用程序。

## <a name="account-management"></a>帐户管理

传统上，管理中的各种应用程序的帐户是由一个手动过程 IT 或支持组织中的个人。 Azure 的广告完全自动化跨所有服务提供商集成应用程序和由 Microsoft 支持自动化的用户供应或 SAML JIT 预先集成这些应用程序的帐户管理。

## <a name="automated-user-provisioning"></a>自动化的用户供应

某些应用程序的创建和删除或停用帐户提供自动化接口。 如果提供商提供此类接口，通过 Azure 广告利用它。 这因为管理任务自动进行，降低了运营成本，并提高环境的安全性，因为它会降低未经授权访问的可能性。

## <a name="access-management"></a>访问管理

使用 Azure 广告可以访问使用单独的应用程序或驱动分配的规则来管理。 您也可以委派访问管理组织确保最佳监督并减少帮助台的负担中适当的人。

## <a name="on-premises-applications"></a>内部部署的应用程序

内置在应用程序代理，您可以发布到您用户中产生内部应用程序一致都从 Azure 的广告监控、 报告和安全功能访问现代云应用程序和福利方面的经验。

## <a name="reporting-and-monitoring"></a>报告和监视

Azure 的广告提供了预先集成的报告和监视功能，您可以知道具有访问权限的应用程序和它们实际使用时。

## <a name="related-capabilities"></a>相关的功能

使用 Azure 广告可以保护您的应用程序的细粒度访问策略以及预先集成的 MFA。 若要了解有关 Azure MFA 的详细信息，请参阅[Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/)。

## <a name="getting-started"></a>入门教程

若要开始使用 Azure AD 集成应用程序，看一下[集成 Azure Active Directory 与应用程序获取启动指南](active-directory-integrating-applications-getting-started.md)。

## <a name="see-also"></a>请参见

[在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
