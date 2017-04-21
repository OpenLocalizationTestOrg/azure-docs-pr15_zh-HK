<properties
    pageTitle="Azure Active Directory 混合标识设计考虑事项--定义的混合标识采用策略 |Microsoft Azure"
    description="有条件的访问控制，Azure Active Directory 检查挑选进行用户身份验证时，才允许访问该应用程序的特定条件。 一旦满足了这些条件，是经过身份验证的用户，并允许应用程序访问。"
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="define-a-hybrid-identity-adoption-strategy"></a>定义的混合标识采用策略

在此任务中，您将定义混合身份解决方案以满足业务要求，所述混合标识采用策略︰

- [确定业务需求](active-directory-hybrid-identity-design-considerations-business-needs.md)
- [确定目录同步的要求](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
- [确定多因素身份验证要求](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>定义业务需求战略
确定组织业务的第一个任务地址需求。  如果您不小心发生范围蔓延，这可以非常广泛。  在开始保持简单但始终牢记进行规划设计，将适应和将来便于更改。  无论是简单的设计还是极其复杂，Azure Active Directory 是 Microsoft 身份平台支持 Office 365、 Microsoft Online Services 和云识别的应用程序。

## <a name="define-an-integration-strategy"></a>定义一种集成策略
Microsoft 有云身份、 同步的身份和联合的身份的三个主要的集成方案。  您应该规划采用其中一种集成策略。  选择各异和决策中选择之一可能包括，哪种类型的用户体验，您想要提供您有一些现有的基础结构已经在原位，和什么是最具成本效益的战略。  
 
![](./media/hybrid-id-design-considerations/integration-scenarios.png)

在上图中定义的情况是︰

- **云的身份**︰ 这些都只是在云环境中存在的身份。  对于 Azure 的广告，它们将位于专门 Azure 的广告目录。
- **已同步**︰ 这些都是内部存在的身份和在云中。  使用 Azure AD 连接，这些用户创建或加入用 Azure AD 的现有帐户。  用户的密码哈希从内部环境同步到云环境中所谓的密码哈希值。  当使用同步需要注意的一点是，如果用户已禁用内部环境中，可能需要为该帐户状态显示在 Azure 的广告多达 3 个小时。  这是因为同步的时间间隔。
- **联合**︰ 这些标识存在内部部署和在云中。  使用 Azure AD 连接，这些用户创建或加入用 Azure AD 的现有帐户。  
 
>[AZURE.NOTE]
有关同步的详细信息选项读取[您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。

下表将帮助您确定的优点和缺点每个以下策略︰

| 战略         | 优势                                                                                                                                                                                                                                                  | 缺点                                                                                                                                                                                                                                                                                                                                                  |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **云的身份** | 易于管理的小公司。 <br> 无法安装所需的内部无其他硬件<br>如果用户离开公司很容易被禁用                                                                                                   | 用户需要登录访问云中的工作负载时 <br> 密码可能会也可能不是相同的云以及内部标识                                                                                                                                                                                                                     |
| **同步**     | 内部的密码将通过身份验证的内部部署和云目录 <br>对于小型、 中型或大型组织管理更容易 <br>用户可以对某些资源具有单一登录 (SSO) <br> 同步的 Microsoft 首选方法 <br> 易于管理 | 有些客户可能不愿意与特定公司的警察由于云同步它们的目录                                                                                                                                                                                                                                                  |
| **联合**        | 用户可以在单一登录 (SSO) <br>如果用户已终止或离开时，可以立即禁用该帐户和访问权限被吊销，<br> 支持高级方案无法完成同步                                           | 更多的步骤来设置和配置 <br> 较高的维护 <br> 可能需要额外的硬件的 STS 的基础架构 <br> 可能需要额外的硬件安装联合身份验证服务器。如果使用 AD FS，则需要额外的软件 <br> 对于 SSO 需要广泛的设置 <br> 如果联合身份验证服务器已停机故障的严重程度，用户将不能进行身份验证 |

### <a name="client-experience"></a>客户端体验
您使用的策略将决定用户的登录体验。  下表为您提供了有关哪些用户应期望他们的登录体验是。  注意并非所有联合的身份提供程序，在所有情况下支持 SSO。

**加入域的和私有的网络应用程序**︰
 

|                              | 同步的标识      | 联合的身份                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Web 浏览器                 | 基于表单的身份验证 | 有时，需要提供组织 ID 的单一登录 |
| Outlook                      | 提示输入凭据     | 提示输入凭据                                       |
| Skype 业务 (Lync)    | 提示输入凭据     | 单点登录 Lync，用于提示输入凭据交换   |
| Skydrive Pro                 | 提示输入凭据     | 单一登录                                               |
| 办公室专业人员加上订阅 | 提示输入凭据     | 单一登录                                               |

**外部不受信任的源或**:

|                              | 同步的标识      | 联合的身份                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Web 浏览器                 | 基于表单的身份验证 |  基于表单的身份验证 |
| Outlook 中，Skype 业务 (Lync) Skydrive Pro 机构订阅| 提示输入凭据     | 提示输入凭据                                       |
| Exchange ActiveSync    | 提示输入凭据     | 单点登录 Lync，用于提示输入凭据交换   |
| 移动应用程序                 | 提示输入凭据     | 提示输入凭据                                               |

如果您确定从任务 1 您有第三方或 IdP 打算使用一个提供与 Azure 的广告联盟，需要注意以下受支持的功能︰
- 这是符合 SP 精简配置文件的任何 SAML 2.0 提供可以支持对 Azure AD 身份验证及相关应用程序
- 支持备用身份验证，它有助于验证 OWA、 SPO 等。
- 可以支持 Exchange 联机客户端通过 SAML 2.0 增强的客户端配置文件 (ECP)

您还必须知道哪些功能将不可用︰

- WS 信任/联合身份验证支持，不会破坏其他所有活动的客户端
 - 这意味着没有 Lync 客户端、 OneDrive 客户机、 办公室预订、 办公移动办公室 2016年之前
- 过渡的办公室到被动身份验证将使他们能够支持纯 SAML 2.0 IdPs，但在客户端的客户端的基础上仍将支持


>[AZURE.NOTE]
最新的列表参阅文章 http://aka.ms/ssoproviders。

## <a name="define-synchronization-strategy"></a>定义同步策略
您将定义的工具，可用于同步此任务在组织的内部数据云以及应使用的拓扑。  因为大多数组织使用 Active Directory，信息使用 Azure AD 连接来解决上述问题提供一些详细信息。  对于没有 Active Directory 的环境，没有关于使用 FIM 2010 R2 或 MIM 2016 帮助规划这一策略的信息。  但是，Azure AD 连接的未来版本将支持 LDAP 目录，所以根据您的时间线，此信息可能无法帮助。

###<a name="synchronization-tools"></a>同步工具
多年来，几种同步工具已经存在，并用于各种方案。  目前 Azure AD 连接是转到工具选择所有受支持的方案。  AAD 同步和目录同步周围还仍是甚至可能立即出现在您的环境中。 

>[AZURE.NOTE]
有关受支持的功能每个工具的最新信息，请阅读[目录集成工具比较](active-directory-hybrid-identity-design-considerations-tools-comparison.md)文章。  

### <a name="supported-topologies"></a>受支持的拓扑
在定义同步策略时，必须确定使用的拓扑。 根据在步骤中已确定的信息 2 可以确定哪种拓扑是要使用的正确一个。 单个的目录林，单个 Azure AD 拓扑结构是最常见和组成一个单一的活动目录林和 Azure AD 的一个实例。  这准备使用在大部分情况下，在下图中所示，使用 Azure AD 连接快速安装时预期的拓扑结构。
 
![](./media/hybrid-id-design-considerations/single-forest.png)单目录林方案是非常常见的大型和小型甚至组织可以具有多个树林，如图 5 所示。

>[AZURE.NOTE]
有关更多有关不同的内部部署及使用 Azure AD 连接的 Azure AD 拓扑同步阅读文章[Azure AD 连接的拓扑](active-directory-aadconnect-topologies.md)。


![](./media/hybrid-id-design-considerations/multi-forest.png) 

多目录林方案

如果此案例然后 forest 多单 Azure AD 拓扑应考虑如果满足以下各项︰

- 用户可以跨所有目录林只有 1 身份 — 下面的用户部分唯一标识此更详细地介绍。
- 到他们的身份所在的林的用户进行身份验证
- 将来自该林的 UPN 和源定位点 （不可变标识）
- 所有目录林都可以访问 Azure AD 连接 – 这意味着不需要加入域中，可以放在 DMZ 中，如果这有助于这是。
- 用户有一个邮箱
- 林承载用户的邮箱具有属性可见 Exchange 全球通讯簿 (GAL) 中的最佳数据质量
- 如果用户没有邮箱，然后任何林可用于分配这些值
- 如果您有一个链接的邮箱，然后还有另一个帐户用于登录不同林中。

>[AZURE.NOTE]
在内部部署中存在并在云中"连接"通过一个唯一标识符的对象。 在目录同步的上下文中，此唯一标识符被称为 SourceAnchor。 在的单一登录环境，这被称为 ImmutableId。 关于使用的 SourceAnchor 的多个因素的[Azure AD 连接的设计概念](active-directory-aadconnect-design-concepts.md#sourceanchor)。

如果上述条件均不成立，并且您有多个活动帐户或多个邮箱，Azure AD 连接会挑一个，忽略其他。  如果有链接邮箱，但没有其他帐户，这些帐户将不会导出到 Azure 的广告，该用户将不是任何组的成员。  这是不同于它与目录同步过去并是有意以更好地支持这些多目录林方案。 在下图中显示了多林方案。
 
![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 
 
**多林多的 Azure 广告方案**

建议在 Azure AD 的组织有一个目录，但它支持其 1:1 的关系保持 Azure AD 连接同步服务器和 Azure 的广告目录之间。  对于 Azure AD 的每个实例中，您将需要 Azure AD 连接的安装。  此外，Azure 的广告，通过设计被隔绝开来，Azure AD 的一个实例中的用户将不能查看另一个实例中的用户。

它不可能而且支持来将 Active directory 的一个内部实例连接到多个 Azure 的广告目录，如下图所示︰

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 
 

**单林筛选方案**

要执行此操作以下必须满足︰

- Azure AD 连接同步服务器必须配置筛选以便它们各有一组互相排斥的对象。  这一点，例如，通过范围对特定域或组织单位的每台服务器。
- 一个 DNS 域只能在单个注册 Azure 的广告目录因此 Upn 广告必须在内部部署中的用户使用不同的命名空间
- 在一个实例中的 Azure AD 用户仅能查看其实例的用户。  他们将不能查看其他实例中的用户
- 只有 Azure 的广告目录之一可以与内部部署 Exchange 混合广告
- 双方的排他性也适用于回写。  这将使一些因为这些假定使用内部部署单个配置不支持此拓扑的回写功能。  这包括︰
 - 与默认配置组回写
 - 设备回写


请注意以下不支持，不应作为实现选择︰

- 不支持具有多个 Azure AD 连接同步服务器连接到同一个 Azure 的广告目录，即使它们被配置为同步互斥对象集
- 不支持同步到多个 Azure 的广告目录相同的用户。 
- 它还不支持进行配置更改，使在一个 Azure 的广告显示为另一个 Azure 的广告目录中的联系人的用户。 
- 这也是不支持修改 Azure AD 连接同步连接到多个 Azure 的广告目录。
- Azure 的广告目录是通过独立的设计。 它不支持更改 Azure AD 连接同步从另一个 Azure 的广告目录中读取数据，试图构建通用的和统一的全球通讯簿的目录之间的配置。 它还不支持导出用户作为联系人到另一个内部使用 Azure AD 连接同步的广告。


>[AZURE.NOTE]
如果您所在组织限制网络上的计算机连接到互联网，本文列出了端点 （Fqdn，IPv4 和 IPv6 地址范围），您也应包含在您的出站允许列表和 Internet Explorer 受信任网站区域的客户端计算机以确保您的计算机可以成功地使用 Office 365。 有关详细信息请阅读[Office 365 的 Url 和 IP 地址范围](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US)。

## <a name="define-multi-factor-authentication-strategy"></a>定义多因素身份验证策略
在此任务中，您将定义使用的多因素身份验证策略。  Azure 的多因素身份验证有两种不同的版本。  一个基于云的另一种是内部部署基于使用 Azure MFA 服务器。  基于评估未上面可以确定哪一种解决方案是正确的策略。  使用下表来确定哪些设计选项可以最好地满足您公司的安全要求︰

多因素设计选项︰

| 资产保护                                               | 云中的 MFA | MFA 前提 |
|---------------------------------------------------------------|------------------|----------------|
| Microsoft 的应用程序                                                | 是的              | 是的            |
| SaaS 应用程序的应用程序库中                                  | 是的              | 是的            |
| 通过 Azure 应用程序代理广告发布的 IIS 应用程序         | 是的              | 是的            |
| 不发布通过 Azure AD 的应用程序代理服务器的 IIS 应用程序 | 不               | 是的            |
| 作为 RDG VPN 的远程访问                                     | 不               | 是的            |

即使您可能已结算的解决方案为您的策略，仍需要使用从上面的计算在您的用户的位置上。  这可能会导致更改的解决方案。  使用下表来帮助您确定︰

| 用户位置                                                       | 首选的设计选项                 |
|---------------------------------------------------------------------|-----------------------------------------|
| Azure 的活动目录                                              | 在云环境中的多 FactorAuthentication |
| Azure 的 AD 和本地广告使用 AD FS 联合身份验证             | 这两                                    |
| Azure 的 AD 和本地广告使用 Azure AD 连接没有密码同步 | 这两                                    |
| Azure 的 AD 和本地 Azure AD 连接使用密码同步  | 这两                                    |
| 本地广告                                                      | 多因素身份验证服务器      |

>[AZURE.NOTE]
此外应确保您选择的多因素身份验证设计选项支持您的设计所需的功能。  有关详细信息请阅读[选择适合您的多元安全解决方案](../multi-factor-authentication-get-started.md#what-am-i-trying-to-secure)。

## <a name="multi-factor-auth-provider"></a>多因素身份验证提供程序
多因素身份验证是默认情况下，为全局管理员必须 Azure Active Directory 租户。 但是，如果您想要扩展到所有用户的多因素身份验证和/或为全局管理员希望能够获取优势功能，如管理门户、 自定义问候语和报表，然后您必须购买和配置多因素身份验证提供程序。

>[AZURE.NOTE]
此外应确保您选择的多因素身份验证设计选项支持您的设计所需的功能。 

##<a name="next-steps"></a>下一步行动
[确定数据保护需求](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>请参见
[设计考虑事项概述](active-directory-hybrid-identity-design-considerations-overview.md)
