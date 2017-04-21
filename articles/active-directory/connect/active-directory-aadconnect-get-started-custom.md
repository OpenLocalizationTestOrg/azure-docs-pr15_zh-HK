<properties
    pageTitle="Azure AD 连接︰ 自定义安装 |Microsoft Azure"
    description="本文详细介绍了 Azure AD 连接的自定义安装选项。 按照以下说明来安装 Active Directory 通过 Azure AD 连接。"
    services="active-directory"
    keywords="什么是 Azure AD 连接，安装 Active Directory，Azure 广告所需的组件"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="custom-installation-of-azure-ad-connect"></a>Azure AD 连接的自定义安装
多个安装选项时，将使用 azure AD 连接**自定义设置**。 如果您有多个目录林，或者您想要配置没有涉及的快速安装的可选功能，则使用它。 在所有情况下，[**快速安装**](active-directory-aadconnect-get-started-express.md)选项不能满足您的部署或拓扑中使用它。

在开始安装 Azure AD 连接之前，请确保下载[Azure AD 连接](http://go.microsoft.com/fwlink/?LinkId=615771)和中必备步骤以完成[Azure AD 连接︰ 硬件和系统必备组件](../active-directory-aadconnect-prerequisites.md)。 此外请确保具有所需可用所述[Azure AD 连接的帐户和权限](active-directory-aadconnect-accounts-permissions.md)的帐户。

如果自定义设置不符合您的拓扑结构，例如若要升级目录同步，请参阅[相关文档](#related-documentation)的其他方案。

## <a name="custom-settings-installation-of-azure-ad-connect"></a>自定义设置安装的 Azure AD 连接

### <a name="express-settings"></a>快速设置
在此页上，单击**自定义**开始自定义的设置安装。

### <a name="install-required-components"></a>安装必需的组件
当您安装同步服务时，您可以取消选中可选配置节和 Azure AD 连接将自动设置所有数据。 它会设置 SQL Server 2012年直通 LocalDB 实例，创建相应的组并分配权限。 如果您想要更改默认设置，您可以使用下表来了解可用的可选配置选项。

![所需的组件](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

可选配置  | 说明
------------- | -------------
使用现有的 SQL Server | 允许您指定 SQL Server 名称和实例名称。 如果您已经有您想要使用数据库服务器，则选择此选项。 输入实例名称跟在**实例名称**逗号和端口号，如果您的 SQL Server 时，没有启用浏览。
使用现有服务帐户 | 默认情况下 Azure AD 连接创建本地服务帐户使用的同步服务。 该密码是自动生成的和未知的安装 Azure AD 连接的人。 如果您使用远程 SQL 服务器，或者使用代理服务器要求身份验证，您需要一个服务帐户在域中并知道密码。 在这些情况下，输入要使用的服务帐户。 请确保运行安装的用户是在 SQL SA，所以可创建服务帐户登录。 请参阅[Azure AD 连接的帐户和权限](active-directory-aadconnect-accounts-permissions.md#custom-settings-installation)
指定自定义同步组 | 默认情况下 Azure AD 连接时将创建四个组为服务器的本地安装了同步服务。 这些组是︰ 管理员组、 操作员组、 浏览组和密码重置组。 您可以指定自己的组下面。 组必须是本地服务器上，并且不能位于域中。

### <a name="user-sign-in"></a>用户登录
在安装之后所需的组件，您需要选择用户单一登录方法。 下表提供可用选项的简短说明。 有关登录的方法的完整说明，请参阅[用户登录](../active-directory-aadconnect-user-signin.md)。

![用户登录](./media/active-directory-aadconnect-get-started-custom/usersignin.png)

单一登录选项 | 说明
------------- | -------------
密码同步 | 用户将能够登录到 Microsoft 云服务，例如 Office 365，使用他们在他们的内部网络中使用相同的密码。 用户密码同步到 Azure 作为密码哈希的广告，在云中进行身份验证。 详细信息，请参阅[密码同步](../active-directory-aadconnectsync-implement-password-synchronization.md)。
与 AD FS 联合身份验证 | 用户将能够登录到 Microsoft 云服务，例如 Office 365，使用他们在他们的内部网络中使用相同的密码。  用户将被重定向到其本地 AD FS 实例登录，并会发生内部身份验证。
未配置 | 既不能安装和配置功能。 如果您已经有第三方联合服务器或另一个现有的解决方案中的位置，请选择此选项。

### <a name="connect-to-azure-ad"></a>连接到 Azure 的广告
在连接到 Azure 广告屏幕上，输入的全局管理员帐户和密码。 如果您在上一页上选择**与 AD FS 联合身份验证**，不登录域中的帐户，您打算启用联盟。 推荐方法是使用带有 Azure 的广告目录的默认**onmicrosoft.com**域中的帐户。

此帐户仅用于在 Azure AD 中创建服务帐户，则不使用该向导完成后。  
![用户登录](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

如果全局管理员帐户具有 MFA 启用了，则需要在提供的密码再次登录弹出并完成 MFA 挑战。 面临的挑战可能会提供一个验证码或电话联络。  
![MFA 的用户登录](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

全局管理帐户也可以启用[特权身份管理](../active-directory-privileged-identity-management-getting-started.md)。

如果您收到错误，并且有连接问题，请参阅[连接问题的疑难解答](../active-directory-aadconnect-troubleshoot-connectivity.md)。

## <a name="pages-under-the-section-sync"></a>下面的部分同步

### <a name="connect-your-directories"></a>连接您的目录
若要连接到活动目录域服务，Azure AD 连接需要具有足够的权限的帐户的凭据。 您可以输入域部分 NetBios 或 FQDN 格式，即 FABRIKAM\syncuser 或 fabrikam.com\syncuser。 此帐户可以是常规用户帐户，因为它只需要默认读取的权限。 但是，这取决于您的方案，可能需要更多的权限。 有关详细信息，请参阅[Azure AD 连接的帐户和权限](../active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account)

![连接目录](./media/active-directory-aadconnect-get-started-custom/connectdir.png)

### <a name="azure-ad-sign-in-configuration"></a>Azure 广告登录配置
此页允许您查看 UPN 域存在于内部 AD DS 和验证在 Azure 的广告。 此页还允许您配置要使用范围内的属性。

![未验证的域](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
查看标记**不会添加**并**不验证**每个域。 请确保已在 Azure AD 验证使用这些域。 当您验证您的域，请单击刷新符号。 有关详细信息，请参阅[添加和验证域](../active-directory-add-domain.md)

**范围内**的属性范围内是特性的用户使用登录到 Azure AD 时，Office 365。 在 Azure AD 用户进行同步之前，应验证域使用，也称为 UPN 后缀。 Microsoft 建议保留默认属性范围。 如果此属性是不可路由，并且无法验证，则可以选择另一个属性。 例如可以选择电子邮件作为属性保存的登录 id。 使用比范围内的另一个特性就是**备选 ID**。 备用的 ID 属性值必须符合 RFC822 标准。 可以与密码同步和联盟使用备选 ID。

>[AZURE.WARNING]
使用其他 ID 与不兼容所有 Office 365 提供工作负荷。 有关详细信息，请参阅[配置备用的登录 ID](https://technet.microsoft.com/library/dn659436.aspx)。

### <a name="domain-and-ou-filtering"></a>域和 OU 筛选
默认情况下将同步所有域和 Ou。 如果有某些域或 Ou，您不想同步到 Azure 的广告，可以取消选取这些域和 Ou。  
![DomainOU 过滤](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png)在向导的此页面配置基于域的筛选。 有关详细信息，请参阅[基于域的筛选](../active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering)。

还有可能某些域而无法到达由于防火墙限制。 这些域默认情况下未选中和一条警告。  
![无法访问域](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
如果看到此警告，请确保这些域将确实不可访问，并且警告在预料之中。

### <a name="uniquely-identifying-your-users"></a>唯一地标识您的用户
跨目录林功能匹配，可以定义用户从 AD DS 林 Azure AD 中的表示方式。 用户可能也会表示仅一次跨所有目录林或有启用和禁用帐户的组合。 用户也可以表示为一些目录林中的联系人为。

![唯一](./media/active-directory-aadconnect-get-started-custom/unique.png)

设置 | 说明
------------- | -------------
[用户将只出现一次跨所有目录林](../active-directory-aadconnect-topologies.md#multiple-forests-separate-topologies) | 在 Azure AD 中作为单个对象创建所有的用户。 对象未加入元节中。
[邮件属性](../active-directory-aadconnect-topologies.md#multiple-forests-full-mesh-with-optional-galsync) | 如果不同目录林中的邮件属性具有相同的值，此选项联接用户和联系人。 已使用 GALSync 创建联系人时，请使用此选项。
[ObjectSID 和 msExchangeMasterAccountSID / msRTCSIP OriginatorSid](../active-directory-aadconnect-topologies.md#multiple-forests-account-resource-forest) | 此选项将连接与资源目录林中的被禁用用户启用的用户帐户的树林中。 在 Exchange 中，这种配置也称为链接邮箱。 如果您只使用 Lync 和交换，不会在该资源林中，还可以使用此选项。
sAMAccountName 和 MailNickName | 此选项加入它的地方找不到该用户的登录 ID 的属性。
特定的属性 | 此选项允许您选择您自己的特性。 **的限制︰**请确保选择已可以在元节中找到的属性。 如果选择自定义属性 （不在中元节），无法完成该向导。

**源标记**的属性 sourceAnchor 是用户对象的生存期内是不可变的特性。 它是在 Azure 广告链接内部用户与该用户的主键值。 因为无法更改该属性，必须准备好要使用的属性。 很好的例子是 objectGUID。 此属性不会更改，除非将用户帐户移林/域之间。 在林之间移动帐户的位置多目录林环境中，另一个属性必须使用，例如，包含雇员 Id 的属性。 避免将一个人结婚时更改或更改工作分配的属性。 您不能使用特性与@-sign,因此不能使用电子邮件和范围。 该属性也是区分大小写因此目录林之间移动一个对象时，请务必保留大写/小写。 二进制属性是 base64 编码的但是其他属性类型保留在其未编码的状态。 在联合方案和一些 Azure AD 接口，此属性也称为是 immutableID。 [设计概念](../active-directory-aadconnect-design-concepts.md#sourceAnchor)中找不到源定位有关的详细信息。

### <a name="sync-filtering-based-on-groups"></a>基于组的同步筛选
筛选组功能允许您为试验同步只有一小部分的对象。 若要使用此功能，请为此目的在内部活动目录中创建一个组。 然后到 Azure AD 作为直接的成员中添加用户和组应进行同步。 稍后，您可以添加和删除用户到此组，以维护的应该是 Azure AD 中的对象的列表。 所有您想要同步的对象必须是组的直接成员。 用户、 组、 联系人和计算机/设备都必须是直接成员。 嵌套的组成员身份不会解决。 当您添加组添加成员，该组本身并不是其成员。

![同步筛选](./media/active-directory-aadconnect-get-started-custom/filter2.png)

>[AZURE.WARNING]
此功能仅用于支持试验性部署。 不要在全面的生产部署中使用它。

在全面的生产部署中，它将很难维护单个组与要同步的所有对象。 您应改用方法之一[配置筛选](../active-directory-aadconnectsync-configure-filtering.md)中。

### <a name="optional-features"></a>可选功能
该屏幕允许您选择用于特定方案的可选功能。

![可选功能](./media/active-directory-aadconnect-get-started-custom/optional.png)

>[AZURE.WARNING]
如果您当前有目录同步或 Azure AD 同步活动，无法激活任何 Azure AD 连接的回写功能。

可选功能 | 说明
------------------- | -------------
混合的 Exchange 部署 | 内部部署的 Exchange 邮箱的共存允许 Exchange 混合部署功能和 Office 365 中。 Azure AD 连接正在返回到您的内部目录同步一组特定的[属性](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback)从 Azure 的广告。
Azure AD 应用程序和属性过滤 | 通过启用 AD Azure 应用程序和属性筛选，可以定制的同步特性集。 此选项将另外两个配置页面添加到向导。 有关详细信息，请参阅[AD Azure 应用程序和属性筛选](#azure-ad-app-and-attribute-filtering)。
密码同步 | 如果您选择联盟作为签入解决方案，您可以启用此选项。 密码同步然后可以用作备份选项。 有关其他信息，请参阅[密码同步](../active-directory-aadconnectsync-implement-password-synchronization.md)。
密码写回 | 通过启用密码写回，源于在 Azure AD 中的密码更改写回您的内部目录。 有关详细信息，请参阅[密码管理入门](../active-directory-passwords-getting-started.md)。
组的写回 | 如果您使用**Office 365 组**功能，您可以有您内部部署 Active Directory 中表示这些组。 此选项才可用如果必须存在您的内部部署 Active Directory 中的 Exchange。 有关详细信息，请参阅[组写回](../active-directory-aadconnect-feature-preview.md#group-writeback)。
设备写回 | 允许您将写回设备对象在 Azure 广告为您的内部部署 Active Directory 的条件访问方案。 有关详细信息，请参阅[在 Azure AD 连接中的启用设备写回](../active-directory-aadconnect-feature-device-writeback.md)。
目录扩展特性同步 | 通过启用目录扩展特性同步，到 Azure AD 同步指定的特性。 有关详细信息，请参阅[目录扩展](../active-directory-aadconnectsync-feature-directory-extensions.md)。

### <a name="azure-ad-app-and-attribute-filtering"></a>Azure AD 应用程序和属性过滤
如果您想要限制要同步到 Azure 广告的特性，然后开始选择您使用的哪些服务。 如果您在此页进行配置更改，新的服务必须显式地选择所通过重新运行安装向导。

![可选功能的应用程序](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

根据在上一步中选择的服务，则此页将显示同步的所有属性。 此列表是正在同步的所有对象类型的组合。 如果有某些特定的属性，您需要不同步，您可以取消选择这些属性。

![可选功能特性](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

>[AZURE.WARNING]
移除属性可能会影响功能。 有关最佳做法和建议，请参阅[同步特性](../active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize)。

### <a name="directory-extension-attribute-sync"></a>目录扩展特性同步
由您的组织或活动目录中的其他属性添加自定义属性，可以在 Azure AD 扩展架构。 要使用此功能，请选择**可选功能**页面上**目录扩展特性同步**。 您可以选择多个属性在此页上的同步。

![目录扩展](./media/active-directory-aadconnect-get-started-custom/extension2.png)

有关详细信息，请参阅[目录扩展](../active-directory-aadconnectsync-feature-directory-extensions.md)。

## <a name="configuring-federation-with-ad-fs"></a>使用 AD FS 配置联合身份验证
使用 Azure AD 连接配置 AD FS 很简单只需几次单击。 以下是前需进行配置。

- 启用远程管理与联合身份验证服务器的 Windows Server 2012 R2 服务器
- 用于启用远程管理的 Web 应用程序代理服务器的 Windows Server 2012 R2 服务器
- 联合身份验证服务名称的 SSL 证书，您打算使用 (例如 sts.contoso.com)

### <a name="ad-fs-configuration-pre-requisites"></a>AD FS 配置必备
要配置您的 AD FS 服务器场使用 Azure AD 连接，请确保在远程服务器上启用 WinRM。 此外，要通过端口要求列在[表 3-Azure AD 连接和联合身份验证服务器/WAP](../active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-federation-serverswap)。

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>创建新的 AD FS 服务器场或使用现有的 AD FS 服务器场
您可以使用现有的 AD FS 服务器场或您可以选择创建一个新的 AD FS 场。 如果您选择创建一个新，您需要提供的 SSL 证书。 如果 SSL 证书受密码保护，您被提示输入密码。

![AD FS 服务器场](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

如果您选择使用现有的 AD FS 服务器场，则将直接转至配置 AD FS 和 Azure 广告屏幕之间的信任关系。

### <a name="specify-the-ad-fs-servers"></a>指定 AD FS 服务器
输入您要在安装 AD FS 服务器。 您可以添加一个或多个服务器根据您的容量规划的需要。 Active Directory 之前执行此配置连接的所有服务器。 Microsoft 建议安装测试和试生产部署的单个 AD FS 服务器。 然后添加并部署更多的服务器以满足比例需要通过初始配置后，再次运行 Azure AD 连接。

>[AZURE.NOTE]
确保，所有服务器都加入到 AD 域之前进行此配置。

![AD FS 服务器](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-the-web-application-proxy-servers"></a>指定的 Web 应用程序代理服务器
输入您想为您的 Web 应用程序的代理服务器的服务器。 Web 应用程序代理服务器部署在 DMZ （面向外部网），并支持从外部网络的身份验证请求。 您可以添加一个或多个服务器根据您的容量规划的需要。 Microsoft 建议安装测试和试生产部署单个 Web 应用程序代理服务器。 然后添加并部署更多的服务器以满足比例需要通过初始配置后，再次运行 Azure AD 连接。 建议您拥有同等数量的满足来自 intranet 身份验证的代理服务器。

>[AZURE.NOTE]
<li> 如果您使用的帐户不是 AD FS 服务器上的本地管理员，您被提示输入管理员凭据。</li>
<li> 确保在运行此步骤之前没有 Azure AD 连接服务器和 Web 应用程序代理服务器之间的 HTTP/HTTPS 连接。</li>
<li> 请确保 Web 应用程序服务器与 AD FS 服务器允许身份验证请求中的排列方式之间的 HTTP/HTTPS 连接。</li>

![Web 应用程序](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

提示您输入凭据，以便可以在 web 应用程序服务器建立安全连接到 AD FS 服务器。 这些凭据必须是 AD FS 服务器上的本地管理员。

![代理服务器](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-the-service-account-for-the-ad-fs-service"></a>指定 AD FS 服务的服务帐户
AD FS 服务需要域服务帐户进行身份验证的用户和查找 Active Directory 中的用户信息。 它可以支持两种类型的服务帐户︰

- **管理服务帐户的组**-在 Active Directory 域服务的 Windows Server 2012 中引入的。 这种类型的客户提供服务，如 AD FS 中，而无需定期更新帐户密码的单个帐户。 如果您已经有 Windows Server 2012 域控制器时，AD FS 服务器所属的域中，请使用此选项。
- **域用户帐户**-这种类型的帐户，必须提供一个密码，并定期更新的密码，该密码更改或过期时。 仅当您不具有 AD FS 服务器所属的域中 Windows Server 2012 域控制器时，请使用此选项。

如果您选择托管服务帐户的组，并且永远不会在 Active Directory 中使用此功能，则提示您提供企业管理员凭据。 这些凭据用于启动密钥存储区和启用 Active Directory 中的功能。

![AD FS 服务帐户](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-the-azure-ad-domain-that-you-wish-to-federate"></a>选择您想要联盟的 Azure AD 域
使用此配置来设置 AD FS 和 Azure 的广告之间的联盟关系。 它配置 AD FS 对 Azure AD 问题安全令牌并配置 Azure AD 信任来自此特定的 AD FS 实例的标记。 此页仅允许您在初始安装中配置单个域。 您可以通过再次运行 Azure AD 连接以后配置多个域。

![Azure AD 域](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-the-azure-ad-domain-selected-for-federation"></a>验证选定为联盟的 Azure AD 域
选择要被联合的域时，Azure AD 连接提供了必要的信息来验证未验证的域。 请参阅[添加验证域和](../active-directory-add-domain.md)有关如何使用此信息。

![Azure AD 域](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

>[AZURE.NOTE]
AD 连接尝试验证的配置阶段中的域。 如果您继续配置而不添加所需的 DNS 记录，不能够完成配置向导。

## <a name="configure-and-verify-pages"></a>配置和验证页
在此页上发生配置。

>[AZURE.NOTE]
在继续安装之前和配置联合身份验证，请确保您已配置[联合身份验证服务器的名称解析](../active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers)。

![准备好进行配置](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>调试模式下
也可以设置新的同步服务器在调试模式下进行。 它仅支持让一台同步服务器导出到一个目录中的云。 但如果希望能够从另一台服务器，例如一个运行目录同步，然后可以启用在调试模式下的 Azure AD 连接。 当启用时，同步引擎导入并同步数据正常工作，但它不会将任何内容导出到 Azure 广告或广告。 在调试模式下禁用功能密码同步和密码写回。

![调试模式下](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

在临时模式下，很可能会对同步引擎进行必要的更改，并查看什么将会被导出。 当配置看上去不错时，请再次运行安装向导并禁用临时模式。 现在导出数据到 Azure AD 从此服务器。 一定要积极地导出这样只有一台服务器的同时禁用另一台服务器。

有关详细信息，请参阅[调试模式](../active-directory-aadconnectsync-operations.md#staging-mode)。

### <a name="verify-your-federation-configuration"></a>请验证您的联合身份验证配置
Azure AD 连接验证您的 DNS 设置，当您单击验证按钮。

![完成](./media/active-directory-aadconnect-get-started-custom/completed.png)

![验证](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

此外，执行下面的验证步骤︰

- 验证您可以登录从域联接计算机在 intranet 上浏览器︰ 连接到 https://myapps.microsoft.com 并验证登录与您的登录帐户。 AD DS 内置管理员帐户不同步，并且不能用于验证。
- 验证您可以登录从外部网的设备。 在家用计算机或移动设备上，连接到 https://myapps.microsoft.com，并提供您的凭据。
- 丰富的客户端登录验证。 连接到 https://testconnectivity.microsoft.com，选择**Office 365**选项卡并选择**Office 365 单一登录测试**。

## <a name="next-steps"></a>下一步行动
安装完成后，注销并重新登录到 Windows 之前使用同步服务管理器或同步规则编辑器。

现在，您已经安装的 Azure AD 连接您可以[验证安装和分配许可证](../active-directory-aadconnect-whats-next.md)。

了解有关安装启用了这些功能︰[防止意外删除](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)和[Azure AD 连接运行状况](../active-directory-aadconnect-health-sync.md)。

了解更多有关这些常见的主题︰[调度程序和如何触发同步](../active-directory-aadconnectsync-feature-scheduler.md)。

了解有关[将您的内部标识使用 Azure Active Directory 集成](../active-directory-aadconnect.md)。

## <a name="related-documentation"></a>相关的文档

主题 |  
--------- | ---------
Azure AD 连接概述 | [与 Azure Active Directory 集成您的内部标识](../active-directory-aadconnect.md)
安装使用快速设置 | [Azure AD 连接的快速安装](active-directory-aadconnect-get-started-express.md)
从目录同步升级 | [从 Azure AD 同步工具 （目录同步） 升级](active-directory-aadconnect-dirsync-upgrade-get-started.md)
用于安装的帐户 | [有关 Azure AD 连接的帐户和权限的详细信息](active-directory-aadconnect-accounts-permissions.md)
