<properties
    pageTitle="Azure Active Directory 混合标识设计考虑事项--定义数据保护策略 |Microsoft Azure"
    description="您将定义为混合身份解决方案以满足您定义的业务要求的数据保护战略。"
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


# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>定义为混合标识解决方案的数据保护战略

在此任务中，您将定义混合身份解决方案以满足业务要求中定义的数据保护战略︰

- [确定数据保护需求](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
- [确定内容管理需求](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
- [确定访问控制要求](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
- [确定事件响应要求](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>定义数据保护选项
它已[确定目录同步要求](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)中所述，Microsoft Azure 广告可以同步与您 Active Directory 域服务 (AD DS) 位于内部。 这种集成使组织能够利用 Azure 的广告来验证用户的凭据时他们正在访问企业资源。 这可以完成这两种情况︰ 在其余部署并在云中的数据。  在 Azure AD 中的数据访问要求用户通过安全令牌服务 (STS) 的身份验证。

一旦通过身份验证，身份验证令牌和复制的分区从读取用户主体名称 (UPN)，确定与用户的域相对应的容器。 用户存在、 已启用的状态和角色的信息通过授权系统用于确定是否在此会话中授权此用户目标租户对请求的访问。 某些授权的操作 （具体而言，创建用户，密码重置） 创建审核跟踪，组织管理员可以用于管理法规遵从性或调查。

从内部数据中心迁移到 Azure 存储通过 Internet 连接的移动数据可能不总是可取由于数据量、 可用带宽或其他事项。 [Azure 存储导入/导出服务](../storage/storage-import-export-service.md)提供了放置或检索大量数据 blob 存储在一个基于硬件的选择。 它允许您[BitLocker 加密](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2)硬盘驱动器将直接发送到 Azure 数据中心，云运算符会将内容上载到您的存储帐户，也可以到您要返回到您的驱动器下载 Azure 数据。 只有加密的磁盘接受此进程 （使用 BitLocker 密钥生成的服务本身期间作业设置）。 BitLocker 密钥提供给 Azure 分开，从而提供了不带密钥共享。

由于传输中的数据可以在不同情况下发生，也是相关了解 Microsoft Azure 使用[虚拟网络](https://azure.microsoft.com/documentation/services/virtual-network/)隔离来自另一个租户的通讯采用措施例如主机和来宾级防火墙、 IP 数据包筛选器、 阻塞端口，和 HTTPS 的终结点。 但是，大部分 Azure 的内部通信，包括基础设施与基础设施和基础结构与客户 （内部部署），也将进行加密。 另一个重要方案是在 Azure 数据中心; 通讯Microsoft 管理网络，以确保任何 VM 可以模拟或窃听的另一个 IP 地址。 当访问 Azure 存储或 SQL 数据库或连接到云服务使用 TLS/SSL。 在这种情况下，客户管理员负责获取 TLS/SSL 证书并将其部署到其租户基础结构。 通过 Microsoft Azure 虚拟网络的单个部署中承租人之间或同一部署中的虚拟机之间通信移动数据时，可以通过加密的通信协议，例如 HTTPS、 SSL/TLS，或其他人进行保护。

这取决于如何回答中[确定的数据保护需求](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)的问题，应该可以确定希望如何保护您的数据以及混合身份解决方案将如何帮助您上的。 下表显示所支持的 Azure 的选项可用于每个数据保护方案。


| 数据保护选项         | 在云中存放 | 在其余部署 | 在传输过程中 |
|---------------------------------|----------------------|---------------------|------------|
| BitLocker 驱动器加密      | X                    | X                   |            |
| SQL Server 可以对数据库进行加密 | X                    | X                   |            |
| 虚拟机到虚拟机加密             |                      |                     | X          |
| SSL/TLS                         |                      |                     | X          |
| VPN                             |                      |                     | X          |


>[AZURE.NOTE]
阅读在[微软 Azure 信任中心](https://azure.microsoft.com/support/trust-center/)，详细了解每个 Azure 服务是符合认证[法规遵从性功能](https://azure.microsoft.com/support/trust-center/services/)。
由于数据保护选项使用多层方法，比较这些选项之间没有适用于此任务。 请确保您将充分利用所有可用的数据为每种状态选项。

## <a name="define-content-management-options"></a>定义内容管理选项
使用 Azure AD 管理混合身份基础结构的一个优点是过程是从最终用户的角度来看完全透明。 用户将重试访问共享的资源，资源要求身份验证，用户必须将身份验证请求发送到 Azure 广告才能获取令牌，并访问该资源。 整个过程发生的背景，而无需用户交互。 还有可能要向一[组](active-directory-manage-groups.md#getting-started-with-access-management)用户授予权限，以允许它们执行某些常见操作。

通常是数据隐私担忧的单位所需的解决方案数据分类。 如果他们当前的内部部署基础结构已经在使用数据分类，就可以利用 Azure 广告作为主存储库的用户的标识。 它是使用内部部署，数据分类 Windows Server 2012 R2 为调用[数据分类 Toolkit](https://msdn.microsoft.com/library/Hh204743.aspx)的通用工具。 此工具可以帮助识别、 分类和保护您的私有云，文件服务器上的数据。 还有可能利用[自动文件分类](https://technet.microsoft.com/library/hh831672.aspx)中 Windows Server 2012 来实现此目的。

如果您的组织没有数据分类中的位置，但需要保护敏感文件，而无需添加新的服务器上部署，则可以使用 Microsoft [Azure 权限管理服务](https://technet.microsoft.com/library/JJ585026.aspx)。  Azure RMS 使用加密、 身份，以及授权策略来帮助保护您的文件和电子邮件，并且它跨多个设备 — — 手机、 平板电脑和电脑。 因为 Azure RMS 是云服务，则不需要显式配置与其他组织信任之前可以与他们共享受保护的内容。 如果他们已具有 Office 365 或 Azure 的广告目录，则自动支持跨组织协作。 您还可以同步只是 Azure RMS 需要使用 Azure 活动目录同步服务 （AAD 同步） 或 Azure AD 连接为内部部署 Active Directory 帐户，支持公共标识的目录属性。

内容管理的一个重要的部分是了解谁正在访问哪些资源，因此格式的日志记录功能是非常重要的身份管理解决方案。 Azure AD 提供包括超过 30 天的日志︰

- 角色成员身份中的更改 (例如︰ 添加到全局管理员角色的用户)
- 更新凭据 (例如︰ 密码更改)
- 域管理 (ex︰ 验证自定义的域，删除域)
- 添加或删除应用程序
- 用户管理 (例如︰ 添加、 删除、 更新用户)
- 添加或删除许可证

>[AZURE.NOTE]
请阅读[Microsoft Azure 的安全和审核日志管理的](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf)详细了解在 Azure 中的日志记录功能。
这取决于如何回答中[确定的内容管理需求](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)的问题，您应能够确定混合标识解决方案中管理内容的方式。 能够将与 Azure AD 集成所有选项在表 6 中公开时，务必要定义它是更适合于您的业务需求。

| 内容管理选项                                                               | 优势                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | 缺点                                                                                                                                                                                                                               |
|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 集中式的内部 （Active Directory 权限管理服务器）                      | 负责对数据进行分类的服务器基础架构的完全控制 <br> 在 Windows 服务器上，不需要额外的许可证或订阅中的内置功能 <br> 可以使用 Azure AD 中混合方案集成 <br> 在 Microsoft 联机服务，如在线交换和 SharePoint Online 以及 Office 365 提供支持信息权限管理 (IRM) 功能 <br> 支持内部 Microsoft 服务器产品，例如 Exchange Server、 SharePoint 服务器和文件服务器的运行 Windows 服务器和文件分类基础结构 (FCI)。 | 因为更高，维护 （跟上更新、 配置以及可能升级），IT 拥有的服务器 <br> 需要服务器基础结构部署<br> Doesn'tleverage Azure 功能本身                                     |
| 集中于云 (Azure RMS)                                                     | 易于管理与内部部署解决方案 <br> 可以与混合方案中的 AD DS 集成 <br>  与 Azure 的广告完全集成 <br> 不需要为部署服务服务器内部部署 <br> 支持部署 Microsoft 服务器产品如 Exchange Server、 SharePoint、 服务器和文件服务器的运行 Windows 服务器和文件分类基础结构 (FCI) <br> IT 可以具有对其租户键具有 BYOK 功能的完全控制。                                                                                    | 您的组织必须支持 RMS 的云订阅 <br> 您的组织必须有 Azure 的广告目录对 RMS 支持用户身份验证                                                                                  |
| 混合 （Azure RMS 集成在一起，内部活动目录权限管理服务器） | 这种情况下累积的优势的同时，集中内部和在云中。                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | 您的组织必须支持 RMS 的云订阅 <br> 您的组织必须有 Azure 的广告目录对 RMS，支持用户身份验证 <br> 需要一个 Azure 的云服务之间的连接和内部部署基础结构 |

## <a name="define-access-control-options"></a>定义访问控制选项
通过利用身份验证，授权和访问控制功能能够使您的公司，同时允许用户和合作伙伴使用一个中心的身份存储库使用单一登录 (SSO)，如下图所示的 Azure AD 中可用︰

![](./media/hybrid-id-design-considerations/centralized-management.png)

集中化的管理，完全与其他目录的集成

Azure 的 Active Directory 提供了单一登录到数千种 SaaS 应用程序和内部部署 web 应用程序。 请阅读[Azure Active Directory 联合身份验证兼容性列表︰ 可用来实现单一登录的第三方身份提供程序](https://msdn.microsoft.com/library/azure/jj679342.aspx)通过 Microsoft 测试有关 SSO 第三方的更多细节的文章。 此功能使公司能够在保持控制的身份和访问管理实现各种 B2B 方案。 然而，在 B2B 设计过程务必了解将由合作伙伴和 Azure 支持此方法进行验证的身份验证方法。 目前，这些是由 Azure 的广告支持的方法︰

- 安全断言标记语言 (SAML)
- OAuth
- Kerberos
- 标记
- 证书


>[AZURE.NOTE] 阅读以了解有关每个协议和 Azure 中的其功能的更多详细信息的[Azure 活动目录的身份验证协议](https://msdn.microsoft.com/library/azure/dn151124.aspx)。

使用 Azure 的广告支持，移动业务应用程序可以使用相同的简单手机服务身份验证体验使员工能够登录到他们公司的 Active Directory 凭据其移动应用程序。 使用此功能，Azure 广告标识提供程序在旁边移动服务与其他标识提供商已经在我们支持 （其中包括 Microsoft 帐户、 Facebook ID、 Google ID 和使用 Twitter，ID） 作为支持。 如果内部部署的应用程序使用用户的凭据位于公司的 AD DS，从合作伙伴和来自云中的用户的访问权限应该是透明的。 可以管理 （基于云的） web 应用程序、 web API、 Microsoft 云服务、 第三方 SaaS 应用程序和本机 （移动） 的客户端应用程序中，用户的条件访问控制和审核，在一个位置报告具有安全性的优点。 但是，它建议在非生产环境中或有限数量的用户验证此。


>[AZURE.TIP] 说到 Azure 的广告没有组策略与 AD DS 至关重要。 为了强制执行设备的策略需要[Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx)之类的移动设备管理解决方案。

用户验证使用 Azure 的广告，很重要评估用户将具有的访问级别。 各异的用户将拥有对资源的访问级别，Azure 的广告可以添加附加的安全层，通过控制访问某些资源，而您必须牢记于心，资源本身也可以其自己的访问控制列表单独设置，如文件服务器中的文件的访问控制。 下图总结了级别的访问控制，您可以在混合方案中︰

![](./media/hybrid-id-design-considerations/accesscontrol.png)


在图中显示图 X 中每个交互组件表示一个访问控制方案，该方案可以覆盖的 Azure 的广告。 下面有每个方案的说明︰

1.条件访问应用程序承载内部︰ 您可以使用已注册的设备访问策略的应用程序被配置为使用 Windows Server 2012 R2 的 AD FS。 有关内部的条件访问设置的详细信息，请参阅[设置内部条件访问使用 Azure 活动目录的设备注册](active-directory-conditional-access-on-premises-setup.md)。
2.访问到 Azure 管理门户控制︰ Azure 也有能力使用 RBAC （角色基于访问控制） 控制对管理门户的访问。 此方法使公司限制的个人可以做后他有权 Azure 管理门户的操作量。 通过使用 RBAC 控制对门户网站的访问，IT 管理员 ca 使用以下访问权限管理方法委派访问权限︰

 - 基于组的角色分配︰ 您分配访问权限可以同步的 Azure AD 组从您本地的活动目录。 这使您可以利用您的组织已在工具和流程的管理组中的现有投资。 您还可以使用 Azure AD 特优的委派的组管理功能。
 - 利用内置的 Azure 中的角色︰ 您可以使用三个角色 — — 所有者、 参与者和读卡器，以确保用户和组有权执行完成其工作所需的任务。
 - 细粒度访问资源︰ 您可以将角色分配给用户和组的特定订阅、 资源组或单个 Azure 网站或数据库等资源。 这种方式，您可以确保用户具有访问他们所需的所有资源并不能访问它们不需要管理的资源。

>[AZURE.NOTE] 阅读[在 Azure 中的基于角色的访问控制](https://azure.microsoft.com/updates/role-based-access-control-in-azure-preview-portal/)来了解有关此功能的更多详细信息。 对于开发人员，正在生成应用程序，并且想要为他们自定义的访问控制，还有可能用于 Azure AD 应用程序角色进行授权。 查看此[WebApp-RoleClaims-最低示例](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet)如何构建您的应用程序以使用此功能。

3.条件访问 Office 365 提供应用程序与 Microsoft Intune: IT 管理员可以设置条件访问设备策略，以保护公司的资源，而同时允许信息工作者集中上兼容的设备来访问这些服务。 有关详细信息，请参阅[Office 365 提供服务的条件访问设备策略](active-directory-conditional-access-device-policies.md)。

4.条件对于 Saas 应用程序的访问︰[此功能](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx)使您可以配置每个应用程序的多因素身份验证的访问规则，并能够阻止不受信任网络上的用户的访问权限。 可以将多因素身份验证规则应用于指派给应用程序，或仅对用户指定的安全组内的所有用户。 如果他们从一个 IP 地址，在组织的内部网络访问应用程序，用户可能不从多因素身份验证的要求。

访问控制选项使用多层方法，因为这些选项之间的比较不是适用于此任务。 请确保您将充分利用可用的需要控制对您的资源访问权限的每个方案的所有选项。

## <a name="define-incident-response-options"></a>定义事件响应选项
Azure 的广告可以帮助 IT 与标识潜在通过监视用户的活动，IT 部门可以对环境中的安全风险利用 Azure AD 的访问和使用情况报告能够深入了解的完整性和组织的目录的安全性。 使用此信息，IT 管理员可以更好地确定可能的安全风险，以便他们可以充分地规划以减轻这些风险可能所在。  [Azure 广告高级订阅](active-directory-get-started-premium.md)有一套安全报告，可让 IT 部门能够获取此信息。 [Azure AD 报告](active-directory-view-access-usage-reports.md)进行分类，如下所示︰

- **异常报告**︰ 包含在事件中，我们发现是反常的符号。 我们的目标是让您注意到此类活动，并使您能够确定事件是否可疑。
- **集成的应用程序报告**︰ 深入了解如何在您的组织正在使用云应用程序。 Azure 的 Active Directory 提供了有上千个云应用程序的集成。
- **错误报告**︰ 指明资源调配到外部应用程序的帐户时可能出现的错误。
- **特定于用户的报告**︰ 在某一特定用户的活动数据中显示设备/签名。
- **活动日志**︰ 过去 24 小时内，过去 7 天内，或最近 30 天内，以及组活动更改和密码重置和注册活动内包含的所有已审核的事件的记录。

>[AZURE.TIP]
此外可以帮助处理案例的事件响应小组的另一份是[与泄漏的凭据的用户](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx)报告。  此报告面，这些泄漏的凭据列表和您租户之间的所有匹配项。

其他重要事件响应调查期间可使用 Azure AD 中内置的报告，并且︰

- **密码重置活动**︰ 管理提供密码重置在组织中使用的方式积极的见解。
- **密码重置注册的活动**︰ 提供了用户在其中注册密码重置为它们的方法的见解和他们选择的方法。
- **组活动**︰ 为该组提供的更改历史记录 (ex︰ 用户添加或删除)，启动访问权限面板中。

除了核心报告功能，可以还在事件响应调查过程中，IT 部门可以利用的 Azure AD 津贴中利用审计报告如获得信息︰

- 角色成员身份中的更改 (例如︰ 添加到全局管理员角色的用户)
- 更新凭据 (例如︰ 密码更改)
- 域管理 (ex︰ 验证自定义的域，删除域)
- 添加或删除应用程序
- 用户管理 (例如︰ 添加、 删除、 更新用户)
- 添加或删除许可证

由于事件响应选项使用多层方法，比较这些选项之间没有适用于此任务。 请确保您将充分利用所有要求作为公司的事故响应过程的一部分使用 Azure AD 报告功能的每个方案的可用选项。

## <a name="next-steps"></a>下一步行动
[确定混合身份管理任务](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)


## <a name="see-also"></a>请参见
[设计考虑事项概述](active-directory-hybrid-identity-design-considerations-overview.md)
