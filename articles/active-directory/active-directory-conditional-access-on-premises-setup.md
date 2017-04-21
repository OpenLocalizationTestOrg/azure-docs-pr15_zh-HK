<properties
    pageTitle="设置内部使用 Azure 活动目录设备注册的条件访问 |Microsoft Azure"
    description="若要启用对内部应用程序在 Windows Server 2012 R2 中使用活动目录联合身份验证服务 (AD FS）) 的条件访问的分步指南。"
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>


# <a name="setting-up-on-premises-conditional-access-using-azure-active-directory-device-registration"></a>设置内部条件访问使用 Azure 活动目录设备注册

个人拥有的设备的用户可以知道您的组织通过要求用户在工作的地方加入他们到 Azure 活动目录设备注册服务的设备进行标记。 下面是一个分步指南，以启用对内部应用程序在 Windows Server 2012 R2 中使用活动目录联合身份验证服务 (AD FS）) 的条件访问。

> [AZURE.NOTE]
> 在 Azure 活动目录设备注册服务条件访问策略中使用设备注册时，office 365 的许可或 Azure AD 特优许可证就是必需的。 这包括由 Active Directory 联合身份验证服务 (AD FS) 实施对内部资源的策略。

对于内部部署的条件访问方案的详细信息，请参阅[加入到从 SSO 和无缝第二个因子身份验证跨公司应用程序的任何设备的场所](https://technet.microsoft.com/library/dn280945.aspx)。

这些功能可供购买 Azure 活动目录特优许可证的客户。

<a name="supported-devices"></a>支持的设备
-------------------------------------------------------------------------
* Windows 7 加入域的设备。
* Windows 8.1 个人和域联接的设备。
* iOS 6 和更高版本的 Safari 浏览器
* 4.0 版或更高版本，三星 GS3 android 手机，三星注 2 以上或平板电脑上面。


<a name="scenario-prerequisites"></a>方案的前提条件
------------------------------------------------------------------------
* 对 Office 365 订阅或 Azure Active Directory 津贴
* Azure Active Directory 租户
* Windows 服务器的 Active Directory (Windows Server 2008 或更高)
* 更新的 Windows Server 2012 R2 中的架构
* Azure 的 Active Directory 特优的许可证
* Windows Server 2012 R2 联合身份验证服务、 配置 SSO 到 Azure 的广告
* Azure 的 Active Directory 连接的 Windows Server 2012 R2 Web 应用程序代理 Microsoft （Azure AD 连接）。 [在此处下载 Azure AD 连接](http://www.microsoft.com/en-us/download/details.aspx?id=47594)。
* 已验证的域。

<a name="known-issues-in-this-release"></a>在此版本中的已知的问题
-------------------------------------------------------------------------------
* 基于设备条件访问策略要求从 Azure Active Directory Active Directory 回写的设备对象。 它可以长达 3 小时的设备对象写入后到 Active Directory
* iOS 7 设备总是提示用户选择一个证书，客户端证书身份验证过程。
* 某些版本的 iOS8 之前 iOS 8.3 不起作用。

## <a name="scenario-assumptions"></a>方案的假设
此方案假定您拥有 Azure AD 租户和内部部署 Active Directory 组成的混合环境。 这些租户应使用 Azure AD 连接进行连接并验证的域和 SSO 的 AD FS。 下面的清单可帮助您配置您的环境与上面描述的阶段。

<a name="checklist-prerequisites-for-conditional-access-scenario"></a>清单︰ 条件访问方案的先决条件
--------------------------------------------------------------
与您的内部部署 Active Directory Azure AD 租户。

## <a name="configure-azure-active-directory-device-registration-service"></a>配置 Azure Active Directory 设备注册服务
使用本指南可以部署和组织配置 Azure 活动目录设备注册服务。

本指南假定您已配置 Windows 服务器的 Active Directory 并已订阅 Microsoft Azure 活动目录。 请参阅上面的系统必备组件。

若要使用 Azure Active Directory 租户部署 Azure 活动目录设备注册服务，完成下面的项目清单，按顺序中的任务。 当引用链接可将您带到概念的主题时后, 返回到此核对清单，以便您可以继续执行此检查表中的其余任务审查概念主题。 某些任务将包括方案验证步骤，可帮助您确认该步骤已成功完成。

## <a name="part-1-enable-azure-active-directory-device-registration"></a>第 1 部分︰ 启用 Azure Active Directory 设备注册

请按照下面来启用和配置 Azure 的活动目录设备注册服务的清单。

| 任务                                                                                                                                                                                                                                                                                                                                                                                             | 引用                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| 在您 Azure Active Directory 租户加入工作区的设备，实现了设备注册。 默认情况下，服务不是允许多因素身份验证。 但是，多因素身份验证被建议注册设备时。 在启用之前 ADRS 在多因素身份验证，请确保该多因素身份验证提供程序配置 AD FS。 | [启用设备注册 Azure 的活动目录](active-directory-conditional-access-device-registration-overview.md)               |
| 通过查找已知的 DNS 记录，设备将会发现 Azure 活动目录设备注册服务。 您必须配置您的公司 DNS，以使设备可以发现 Azure 活动目录设备注册服务。                                                                                                                                                   | [配置 Azure 活动目录设备注册发现。](active-directory-conditional-access-device-registration-overview.md) |

##<a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>第 2 部分︰ 部署和配置 Windows Server 2012 R2 Active Directory 联合身份验证服务和建立与 Azure 广告联盟关系

| 任务                                                                                                                                                                                                                                                                                                                                                                                             | 引用                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| 部署 Windows Server 2012 R2 架构扩展与 Active Directory 域服务域。 您不需要任何域控制器升级到 Windows Server 2012 R2。 模式升级是唯一的要求。 | [升级您的 Active Directory 域服务架构](#upgrade-your-active-directory-domain-services-schema)               |
| 通过查找已知的 DNS 记录，设备将会发现 Azure 活动目录设备注册服务。 您必须配置您的公司 DNS，以使设备可以发现 Azure 活动目录设备注册服务。                                                                                                                                                   | [准备活动目录支持的设备](#prepare-your-active-directory-to-support-devices) |


##<a name="part-3-enable-device-writeback-in-azure-ad"></a>在 Azure 广告中的第 3 部分︰ 启用设备写回

| 任务                                                                                                                                                                                                                                                                                                                                                                                             | 引用                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| 完成 Azure AD 连接中启用设备写回第 2 的部分。 完成后，返回这本指南。 | [在 Azure AD 连接中启用设备写回](#upgrade-your-active-directory-domain-services-schema)               |


##<a name="optional-part-4-enable-multi-factor-authentication"></a>[可选]第 4 部分︰ 启用多因素身份验证

强烈建议您配置多因素身份验证的多个选项之一。 如果您想要要求 MFA，请参阅[选择适合您的多元安全解决方案](../multi-factor-authentication/multi-factor-authentication-get-started.md)。 它包括每个解决方案和链接以帮助您配置您选择的解决方案的说明。

## <a name="part-5-verification"></a>第 5 部分︰ 验证

已完成部署。 现在，您可以尝试一些方案。 请按照下面的链接以尝试服务，成为熟悉的功能


| 任务                                                                                                                                                                                                                         | 引用                                                                       |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 某些设备加入您的工作区使用 Azure 活动目录的设备注册。 您可以加入 iOS、 窗口和 Android 设备                                                                                         | [将设备连接到使用 Azure 活动目录设备注册工作场所](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| 可以查看和启用/禁用注册的设备使用管理门户。 在此任务中将查看一些已注册的设备使用管理门户。                                                        | [Azure 的 Active Directory 设备注册概述](active-directory-conditional-access-device-registration-overview.md)                             |
| 请验证该设备对象都被写回从 Azure Active Directory Windows 服务器的 Active Directory。                                                                                                                  | [确认注册的设备到 Active Directory 都写回](#verify-registered-devices-are-written-back-to-active-directory)                  |
| 现在，用户可以注册他们的设备，您可以创建应用程序允许注册的设备的 AD FS 中的访问策略。 在此任务中，您将创建应用程序的访问规则和自定义访问被拒绝的消息 | [创建一个应用程序访问策略和自定义的访问被拒绝消息](#create-an-application-access-policy-and-custom-access-denied-message)            |



## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>与内部部署 Active Directory 集成 Azure 的活动目录
这将帮助您与您的内部部署 active directory，使用 Azure AD 连接集成 Azure AD 租户。 虽然步骤还可在 Azure 的传统门户网站，请记下本节中列出的任何特定说明。

1.  登录到 Azure 经典门户使用是 Azure AD 中的全局管理员帐户。
2.  在左窗格中，选中**Active Directory**。
3.  在**目录**选项卡上，选择您的目录。
4.  选择**目录集成**选项卡。
5.  在**部署和管理**的部分，按照 1 到 3 以将 Azure Active Directory 集成与您的内部目录的步骤。
  1.    添加域。
  2.    安装并运行 Azure AD 连接︰ 安装 Azure AD 连接时使用下面的说明， [Azure AD 连接的自定义安装](./connect/active-directory-aadconnect-get-started-custom.md)。
  3. 验证和管理目录同步。 在本步骤中，单点登录指令不可用。

  > [AZURE.NOTE]
  > 上面的链接的文档中所述配置 AD FS 联合身份验证。 您不需要配置任何预览功能。


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Active Directory 域服务架构升级

> [AZURE.NOTE]
> 将 Active Directory 架构升级不可逆转。 建议您先执行此测试环境中。

1. 登录到您具有架构管理员和企业管理员权限的帐户的域控制器。
2. 将**[媒体] \support\adprep**目录和子目录复制到 Active Directory 域控制器之一。
3. 其中 [媒体] 是 Windows Server 2012 R2 安装媒体的路径。
4. 从命令提示符处，导航到 adprep 目录并执行︰ **adprep.exe /forestprep 成功**。 按照屏幕上的说明完成架构升级。

## <a name="prepare-your-active-directory-to-support-devices"></a>准备活动目录支持设备

>[AZURE.NOTE] 这是一项一次性操作，必须运行，以便准备活动目录林以支持设备。 您必须具有企业管理员权限登录和活动目录林必须具有 Windows Server 2012 R2 的架构来完成此过程。


##<a name="prepare-your-active-directory-forest-to-support-devices"></a>准备活动目录林以支持设备

> [AZURE.NOTE]
>这是一项一次性操作，必须运行，以便准备活动目录林以支持设备。 您必须具有企业管理员权限登录和活动目录林必须具有 Windows Server 2012 R2 的架构来完成此过程。

### <a name="prepare-your-active-directory-forest"></a>准备活动目录林

1.  在联合身份验证服务器上，打开 Windows PowerShell 命令窗口，然后键入︰ 初始化 ADDeviceRegistration
2.  当系统提示您输入**ServiceAccountName**，输入 AD fs 选中该服务帐户作为服务帐户的名称。 如果是 gMSA 帐户， **domain\accountname$**格式输入的帐户。 对于域帐户，请使用格式**domain\accountname**。



### <a name="enable-device-authentication-in-ad-fs"></a>启用 AD FS 中的设备验证

1. 在联合身份验证服务器上，打开 AD FS 管理控制台，然后定位到**AD FS** > **身份验证策略**。
2. 选择**编辑全局主身份验证...** 从**操作**窗格中。
3. 检查**启用的身份验证设备**，然后选择**确定**。
4. 默认情况下，AD FS 将定期删除未使用的设备活动目录。 这样可以在 Azure 中受控设备使用 Azure 活动目录设备注册时，必须禁用此任务。


### <a name="disable-unused-device-cleanup"></a>禁用未使用的设备清理
1. 在联合身份验证服务器上，打开 Windows PowerShell 命令窗口，然后键入︰ 集 AdfsDeviceRegistration MaximumInactiveDays 0

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>准备设备写回的 Azure AD 连接

1.  完成第 1 部分︰ 准备 Azure AD 连接。


## <a name="join-devices-to-your-workplace-using-azure-active-directory-device-registration"></a>将设备连接到使用 Azure 活动目录设备注册工作场所

### <a name="join-an-ios-device-using-azure-active-directory-device-registration"></a>加入一个 iOS 设备使用 Azure 活动目录设备注册

活动目录设备注册 azure iOS 设备使用无线电配置文件注册过程。 这一过程始于用户连接到使用 Safari 网络浏览器的配置文件注册 URL。 URL 格式如下所示︰

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

其中`yourdomainname`是 Azure Active Directory 配置的域名。 例如，如果您的域名是 contoso.com，URL 将是︰

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

有许多不同的方式来传达给您的用户此 URL。 推荐的方法之一是发布此 URL 中的自定义应用程序可以访问被拒绝在 AD FS 中的消息。 在即将到来的部分介绍︰[创建应用程序访问策略和自定义的访问被拒绝消息](#create-an-application-access-policy-and-custom-access-denied-message)。

###<a name="join-a-windows-81-device-using-azure-active-directory-device-registration"></a>加入 Windows 8.1 设备使用 Azure 活动目录设备注册

1. 在您的 Windows 8.1 设备，导航到**PC 设置** > **网络** > **的工作场所**。
2. UPN 格式输入您的用户名。 例如， dan@contoso.com.。
3. 选择**加入**。
4. 出现提示时，登录与您的凭据。 现在加入该设备。

###<a name="join-a-windows-7-device-using-azure-active-directory-device-registration"></a>加入 Windows 7 设备使用 Azure 活动目录设备注册
若要注册 Windows 7 域加入部署设备注册软件程序包所需的设备。 软件包称为 Windows 7 的工作场所加入，而且是可用于在[Microsoft 连接网站](https://connect.microsoft.com/site1164)下载。 说明如何使用该软件包都在[为 Windows 7 的域配置设备自动注册加入设备](active-directory-conditional-access-automatic-device-registration-windows7.md)。

### <a name="join-an-android-device-using-azure-active-directory-device-registration"></a>加入 Android 设备使用 Azure 活动目录设备注册

[Azure 鉴别码 Android 的主题](active-directory-conditional-access-azure-authenticator-app.md)如何在 Android 设备上安装 Azure 身份验证器应用程序并添加工作帐户都有说明。 当工作帐户在 Android 设备上创建成功时，该设备是加入该组织的工作场所。

## <a name="verify-registered-devices-are-written-back-to-active-directory"></a>确认注册的设备到 Active Directory 都写回
您可以查看并验证该设备对象已写入重新将 Active Directory 使用 LDP.exe 或 ADSI 编辑。 二者都有使用活动目录管理员工具。

默认情况下，将作为 AD FS 场位于同一域中放一些写入后从 Azure Active Directory 的设备对象。

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>创建一个应用程序访问策略和自定义的访问被拒绝消息
请考虑以下情形︰ 创建 AD FS 中的信赖方信任的应用程序和配置颁发授权规则，允许注册的设备。 现在只有已注册的设备被允许访问该应用程序。 为了更容易为您的用户才能访问该应用程序，您可以配置自定义拒绝访问的消息，其中包括说明如何加入他们的设备。 现在，您的用户具有无缝地访问应用程序注册其设备。

以下步骤显示如何实现此方案。

>[AZURE.NOTE]
本节假设您，您已经配置信赖方信任 AD FS 中的应用程序。

1. 打开 AD FS MMC 工具，然后定位到 AD FS > 信任关系 > 信赖方信任。
2. 找到应用程序，将应用此新的访问规则。 用鼠标右键单击该应用程序，然后选择编辑声明规则...
3. 选择**颁发授权规则**选项卡，然后选择**添加规则...**
4. 从**声明规则**模板下拉列表中，选择**允许或拒绝用户基于传入声明**。 选择**下一步**。
5. 声明规则名称︰ 字段类型︰**允许从已注册的设备访问**
6. 从传入声明类型︰ 下拉列表中，选择**是已注册的用户**。
7. 在传入声明值︰ 字段中，键入︰**真**
8. 选择**允许用户与此传入声明访问**单选按钮。
9. 选择**完成**，然后选择**应用**。
10. 删除是个刚刚创建的规则的任何规则。 例如，删除默认**允许所有用户的访问**规则。

您的应用程序现在被配置为仅在用户来自它们注册并加入到工作场所的设备时，才允许访问。 对于更高级的访问策略，请参阅[与多元访问控制管理风险](https://technet.microsoft.com/library/dn280949.aspx)。

接下来，您将为应用程序配置自定义错误消息。 错误消息会让用户知道，它们必须加入其设备工作场所才允许他们访问该应用程序。 您可以创建自定义应用程序拒绝访问的消息使用自定义的 HTML 和 Windows PowerShell。

在联合身份验证服务器上，打开 Windows PowerShell 命令窗口并键入以下命令。 该命令的部分替换为特定于您的系统的项目︰

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
您可以访问此应用程序之前，您必须注册您的设备。

**如果您使用的 iOS 设备上，则选择此链接来连接您的设备**︰

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

这个 iOS 设备加入您的工作区。


**如果您使用的 Windows 8.1 设备**，您可以加入您的设备转到**PC 设置** > **网络** > **的工作场所**。


其中"**信赖方信任名称**"是在 AD FS 配置成信赖方信任的应用程序对象的名称。
其中**yourdomain.com**是 Azure Active Directory 配置的域名称。 如 contoso.com。
请务必将传递给**一组 AdfsRelyingPartyWebContent** cmdlet 的 html 内容中删除任何换行符 （如果有的话）。


现在，当用户访问您的应用程序从没有用 Azure 的活动目录设备注册服务注册的设备，他们将收到页看起来类似于下面的截屏。

![用户没有使用 Azure AD 注册其设备时出现错误 Screeshot](./media/active-directory-conditional-access/error-azureDRS-device-not-registered.gif)

##<a name="related-articles"></a>相关的文章

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
