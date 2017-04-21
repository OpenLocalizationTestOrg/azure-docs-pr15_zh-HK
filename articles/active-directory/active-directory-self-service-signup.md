<properties
    pageTitle="自助服务注册 Azure 是什么？ |Microsoft Azure"
    description="Azure 概述的自助服务注册，如何来管理注册过程中，如何接管一个 DNS 域名。"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>


# <a name="what-is-self-service-signup-for-azure"></a>自助服务注册 Azure 是什么？

本主题说明在自助服务注册流程以及如何接管一个 DNS 域名。  

## <a name="why-use-self-service-signup"></a>为什么使用自助服务的注册？

- 获取客户对他们想要更快的服务。
- 创建基于电子邮件的服务的提供。
- 创建基于电子邮件的注册流程快速允许用户创建标识使用它们容易记住工作电子邮件别名。
- 非托管的 Azure 目录可以使到托管目录，可重复使用的其他服务。

## <a name="terms-and-definitions"></a>术语和定义

+ **自助服务注册**︰ 这是云服务的注册用户和他们的电子邮件域上基于已自动为其创建在 Azure 活动目录 (AD Azure) 标识的方法。
+ **非托管的 Azure 目录**︰ 这是在其中创建该身份信息的目录。 非托管的目录是没有全局管理员都有一个目录。
+ **电子邮件验证用户**︰ 这在 Azure AD 是一种类型的用户帐户。 具有自动创建后为自助服务的优惠注册被称为电子邮件验证用户身份的用户。 电子邮件验证用户已使用 creationmethod 标记的目录的常规成员 = EmailVerified。

## <a name="user-experience"></a>用户体验

例如，假设的用户的电子邮件是Dan@BellowsCollege.com收到通过电子邮件的敏感文件。 这些文件已被保护起来 Azure 权限管理 (Azure RMS)。 但陶建明的组织，风箱大学 Azure RMS 不注册了或已部署活动目录 RMS。 在这种情况下，Dan 可以注册 RMS 对于个人免费订阅才能读取受保护的文件。

如果陶建明是第一个从 BellowsCollege.com，那么在 Azure AD 将为 BellowsCollege.com 创建一个非托管的目录提供，该自助服务注册的电子邮件地址的用户。 如果 BellowsCollege.com 域中的其他用户注册此产品或类似的自助服务，他们还将在 Azure 中的同一个非托管目录中创建的电子邮件验证用户帐户。

## <a name="admin-experience"></a>管理体验

管理员负责非托管的 Azure 目录的 DNS 域名可以接管或合并后证明所有权的目录。 以下部分将介绍更多细节，管理经验，但以下是摘要︰

- 采用通过非托管的 Azure 目录时，只会变得非托管目录的全局管理员。 这有时被称为内部接管。
- 当合并非托管的 Azure 目录、 到 Azure 托管目录添加托管目录的 DNS 域名和创建一个映射的用户的资源因此用户可以继续访问而不会中断服务。 这有时称为外部接管。

## <a name="what-gets-created-in-azure-active-directory"></a>获取创建的内容在 Azure Active Directory 中？

#### <a name="directory"></a>目录

- Azure Active Directory 域创建目录，每个域一个目录。
- Azure 的广告目录有无全局管理员。

#### <a name="users"></a>用户

- 为每个注册用户，Azure 的广告目录中创建用户对象。
- 每个用户的对象标记为外部。
- 每个用户授予访问的服务，他们签约了。

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>如何声明一个自助服务的 Azure 广告我拥有域目录？

您可以声明一个自助服务的 Azure 广告通过执行域验证目录。 域验证证明您拥有创建 DNS 记录的域。

有两种方法实现了 Azure 的广告目录 DNS 接管︰

- 内部接管 （管理发现一个非托管的 Azure 目录，并想要变成一个托管目录）
- 外部接管 （管理员尝试将新域添加到其托管的 Azure 目录）

您可能会感兴趣验证您拥有某个域，因为采取通过非托管目录后用户执行自助服务注册，或者您可能将新域添加到现有托管目录。 例如，您有一个名为 contoso.com 域和您想要添加新的域命名为 contoso.co.uk 或 contoso.uk。

## <a name="what-is-domain-takeover"></a>什么是域接管？  

本部分介绍如何验证您拥有一个域

### <a name="what-is-domain-validation-and-why-is-it-used"></a>什么是域验证以及为什么使用？

要执行操作的目录上，Azure 广告要求您验证 DNS 域的所有权。  域的验证可报销目录，并将提升自助服务目录管理的目录，或合并到现有托管目录的自助服务目录。

## <a name="examples-of-domain-validation"></a>域有效性规则的示例

有两种方法实现 DNS 接管的目录︰

+ 内部的接管 （例如，管理员发现自助服务、 非托管目录，并想要转换为托管目录）
+ 外部接管 （例如，管理员尝试将新域添加到托管目录）

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>内部接管-升级自助服务、 非托管目录是一个托管的目录

执行内部接管时，该目录获取转换成从非托管目录管理的目录。 您需要完成您创建 DNS 区域中的 MX 记录或 TXT 记录的 DNS 域名称验证。 该操作︰

+ 验证您拥有域
+ 使目录管理
+ 使您的目录的全局管理

假设 IT 管理员从风箱大学发现自助服务，用户从学校注册了。 如已注册的 DNS 所有者名称 BellowsCollege.com，IT 管理员可以验证在 Azure 中的 DNS 名称的所有权，并再接管非托管目录。 目录将成为管理的目录，并且 IT 管理员分配全局管理员角色 BellowsCollege.com 目录。

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>外部接管的自助服务目录合并现有托管目录

在外部接管，您已经有一个托管的目录和希望所有的用户和组从非托管目录加入该托管的目录，而不是自己的两个单独的目录。

作为管理的目录管理，添加到域中，并且该域碰巧有一个与其关联的非托管的目录。

例如，假设您是 IT 管理员，您已经具有 Contoso.com，为您的组织注册一个域名管理的目录。 您发现，用户从您的组织有执行自助服务号向上提供通过电子邮件域名user@contoso.co.uk,这是您的组织拥有的另一个域名。 这些用户当前 contoso.co.uk 托管目录中具有帐户。

您不想要管理两个单独的目录，因此 contoso.co.uk 的非托管的目录合并 contoso.com 您现有的 IT 管理目录。

外部接管遵循相同的 DNS 验证进程内部的接管。  差别是︰ 用户和服务被重新映射到 IT 管理目录。

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>执行外部接管的影响是什么？

有外部接管，以便用户能够继续访问而不会中断服务创建的用户对资源的映射。 许多应用程序，对于个人、 包括 RMS 处理用户对资源的映射，并且用户可以继续访问这些服务，而不会更改。 如果应用程序不能有效地处理用户对资源的映射，外部接管可能显式阻止以防止用户不好的体验。

#### <a name="directory-takeover-support-by-service"></a>目录服务的接管支持

目前下列服务支持接管︰

- RMS


很快将支持以下服务接管︰

- PowerBI

以下不这样做，需要更多的管理操作在外部接管后迁移用户数据。

- SharePoint/OneDrive


## <a name="how-to-perform-a-dns-domain-name-takeover"></a>如何执行 DNS 域的名称接管

您有关于如何执行域验证 （和进行接管，如果您愿意的话） 的几个选项︰

1.  Azure 的管理门户

    接管时触发执行域添加。  如果目录已存在的域，您必须执行外部接管的选项。

    登录到 Azure 门户使用您的凭据。  导航到您现有的目录，然后再**添加域**。

2.  Office 365

    可以使用在 Office 365[管理域](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/)页面上的选项若要使用您的域和 DNS 记录。 请参阅[验证您在 Office 365 中的域](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/)。

3.  Windows PowerShell

    以下步骤需要执行使用 Windows PowerShell 的验证。

    步骤    |   若要使用的 Cmdlet
    ------- | -------------
    创建凭据 | 获取凭据
    连接到 Azure 的广告 | 连接 MsolService
    获取域的列表   | 获得 MsolDomain
    创建一个难题  | 获得 MsolDomainVerificationDns
    创建 DNS 记录   | 您的 DNS 服务器上执行此操作
    验证所面临的挑战    | 确认 MsolEmailVerifiedDomain

例如︰

1. 连接到使用自助服务响应时所用的凭据的 Azure AD︰ 导入模块 MSOnline $msolcred = 获取凭据连接 msolservice-凭据 $msolcred

2. 获取列表的域︰

    获得 MsolDomain

3. 然后运行 Get MsolDomainVerificationDns 用于创建一项挑战︰

    获得 MsolDomainVerificationDns – 域名*your_domain_name* – 模式 DnsTxtRecord

    例如︰

    获得 MsolDomainVerificationDns – 域名 contoso.com – 模式 DnsTxtRecord

4. 复制此命令返回的值 （挑战）。

    例如︰

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9

5. 在公用 DNS 命名空间，创建一个 DNS txt 记录包含您在上一步中复制的值。

    此记录的名称是父域的名称，因此如果您通过从 Windows 服务器的 DNS 角色创建该资源记录，保留记录名称为空，这只粘贴值在文本框中

6. 运行确认 MsolDomain cmdlet 以验证所面临的挑战︰

    确认 MsolEmailVerifiedDomain 的域名*your_domain_name*

    例如︰

    确认 MsolEmailVerifiedDomain-域名 contoso.com

一项成功的挑战将返回到没有错误提示。

## <a name="how-do-i-control-self-service-settings"></a>如何控制自助服务设置？

管理员目前拥有两个自助服务的控件。 他们可以控制︰

- 无论用户可以加入通过电子邮件的目录。
- 无论用户可以许可应用程序和服务本身。


### <a name="how-can-i-control-these-capabilities"></a>如何控制这些功能？

管理员可以配置这些功能使用这些 Azure AD cmdlet 集 MsolCompanySettings 参数︰

+ **AllowEmailVerifiedUsers**控制用户是否可以创建或加入一个非托管的目录。 如果将该参数设置为 $false，没有电子邮件验证用户可以加入目录。
+ **AllowAdHocSubscriptions**控制向上执行自助服务登录的用户的能力。 如果将该参数设置为 $false，任何用户都可以不执行自助服务注册。


### <a name="how-do-the-controls-work-together"></a>控件的工作方式在一起？

可以配合使用这两个参数来定义更精确地控制自助服务号组成。 例如，以下命令将允许用户执行自助服务号，但只，如果这些用户已经有一个帐户在 Azure AD （换句话说，用户需要创建一个电子邮件验证帐户无法执行自助服务号向上）︰

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

下面的流程图解释了这些参数的不同组合和目录和自助服务号产生的条件。

![][1]

更多的信息和如何使用这些参数的示例，请参阅[设置 MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)。

## <a name="see-also"></a>请参见

-  [如何安装和配置 Azure PowerShell](../powershell-install-configure.md)

-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)

-  [Azure Cmdlet 引用](https://msdn.microsoft.com/library/azure/jj554330.aspx)

-  [一组 MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
