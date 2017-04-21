<properties
    pageTitle="对 Azure Active Directory 访问问题进行故障诊断 |Microsoft Azure"
    description="了解您可以采取以解决您的组织的在线资源的访问问题的步骤。"
    services="active-directory"
    keywords="基于设备的条件访问，设备注册启用设备注册、 设备注册和 MDM"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="markvi"/>


# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>对 Azure Active Directory 访问问题进行故障诊断

您尝试访问您的组织的 SharePoint Online intranet 和收到"拒绝访问"错误消息。 你是做什么工作的？

本文介绍了可以帮助您解决您的组织的在线资源的访问问题的补救措施。

以帮助解决 Azure 活动目录 (AD Azure) 访问的问题，请转到部分中的文章，介绍了设备平台︰

-   Windows 设备
-   iOS 设备 (检查后立即寻求与 Iphone 和 iPads。)
-   Android 设备 (检查后立即与 Android 的求助电话和平板电脑。)

## <a name="access-from-a-windows-device"></a>从 Windows 设备访问

如果您的设备运行下面的平台之一，看起来在下面的部分显示了当您尝试访问应用程序或服务时的错误消息︰

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2

### <a name="device-is-not-registered"></a>未注册设备

如果您的设备未在注册 Azure 的广告并且用基于设备的策略保护应用程序，您可能会看到页，其中显示以下错误消息之一︰

!["您不能获得那里从这里"消息未注册设备](./media/active-directory-conditional-access-device-remediation/01.png "方案")

如果您的设备已加入域到 Active Directory 在您的组织中，试试这个︰

1.  请确保，在您登录到 Windows 使用工作帐户 （Active Directory 帐户）。
2.  连接到企业网络的虚拟专用网络 (VPN) 或 DirectAccess 通过。
3.  建立连接后，按下 Windows 徽标键 + L 键锁定您的 Windows 会话。
4.  请输入您的工作帐户凭据解锁您的 Windows 会话。
5.  等待一分钟，然后再试访问应用程序或服务。
6.  如果您看到同一页上，单击**详细信息**链接，然后与您的管理员的详细信息。

如果您的设备未加入域的且运行 Windows 10，您有两个选项︰

- 运行 AD 的 Azure 联接
- 将您的工作或学校的帐户添加到 Windows

有关这些选项有什么不同的信息，请参阅[使用 Windows 10 设备在您的工作区中](active-directory-azureadjoin-windows10-devices.md)。

要运行 Azure AD 连接，请执行以下步骤为您的设备运行的平台。 （azure AD 连接上没有 Windows 手机。）

**Windows 10 周年更新**

1.  打开**设置**应用程序。
2.  单击**帐户** > **访问工作或学校**。
3.  单击**连接**。
4.  单击**连接此设备到 Azure 的广告**。
5.  向您的组织的身份验证，如果出现提示，然后按照显示的步骤提供多因素身份验证。
6.  出，签名，然后使用您的帐户登录。
7.  尝试再次访问该应用程序。


**10 11 月 2015 Windows 更新**

1.  打开**设置**应用程序。
2.  单击**系统** > **有关**。
3.  单击**加入 Azure 的广告**。
4.  向您的组织的身份验证，如果出现提示，然后按照显示的步骤提供多因素身份验证。
5.  注销，，然后以工作帐户 （Azure 的广告帐户） 登录。
6.  尝试再次访问该应用程序。

若要添加您工作或学校的帐户，请执行以下步骤︰

**Windows 10 周年更新**

1.  打开**设置**应用程序。
2.  单击**帐户** > **访问工作或学校**。
3.  单击**连接**。
4.  向您的组织的身份验证，如果出现提示，然后按照显示的步骤提供多因素身份验证。
5.  尝试再次访问该应用程序。


**10 11 月 2015 Windows 更新**

1.  打开**设置**应用程序。
2.  单击**帐户** > **您的帐户**。
3.  单击**添加工作或学校的帐户**。
4.  向您的组织的身份验证，如果出现提示，然后按照显示的步骤提供多因素身份验证。
5.  尝试再次访问该应用程序。

如果您的设备未加入域的且运行 Windows 8.1，工作场所加入并注册参加 Microsoft Intune 请执行以下步骤︰

1.  打开**PC 设置**。
2.  单击**网络** > **的工作场所**。
3.  单击**连接**。
4.  向您的组织的身份验证，如果出现提示，然后按照显示的步骤提供多因素身份验证。
5.  单击**打开**。
6.  尝试再次访问该应用程序。


### <a name="browser-is-not-supported"></a>浏览器不受支持

如果您尝试通过使用以下浏览器之一来访问应用程序或服务，则可能会被拒绝访问︰

- 铬色、 Firefox 或 Microsoft 边缘或 Microsoft Windows 10 或 Windows 服务器 2016年的 Internet Explorer 以外的任何其他浏览器
- Windows 8.1、 Windows 7、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 中的 Firefox

您将看到错误页面，如下所示︰

!["您不能获得那里从这里开始"的消息不受支持的浏览器](./media/active-directory-conditional-access-device-remediation/02.png "方案")

唯一的补救措施是使用应用程序的设备平台支持的浏览器。

## <a name="next-steps"></a>下一步行动

[Azure Active Directory 条件访问](active-directory-conditional-access.md)
