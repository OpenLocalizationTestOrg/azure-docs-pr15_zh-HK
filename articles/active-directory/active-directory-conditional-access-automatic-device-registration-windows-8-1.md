<properties
    pageTitle="配置设备自动注册 Windows 8.1 加入域设备 |Microsoft Azure"
    description=" 要配置 Windows 8.1 加入域的设备使用 Azure 广告自动注册组策略的步骤。 "
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
    ms.date="09/21/2016"
    ms.author="Markvi"/>

# <a name="configure-automatic-device-registration-for-windows-81-domain-joined-devices"></a>配置 Windows 8.1 加入域设备的设备自动注册

可以使用 Active Directory 组策略来配置 Windows 8.1 加入域设备自动注册 Azure 的广告。 要配置组策略，您必须安装组策略管理功能与至少一个域加入的 Windows Server 2012 R2 或 Windows 8.1 的计算机。 为您的域中启用了此组策略后，任何登录到该计算机的域用户，将自动地默默使用注册设备对象在 Azure 的广告。 在 Azure AD 为物理设备的每个已注册用户都有一个设备对象。请务必仔细阅读并完成从 Azure Active Directory for Windows Domain-Joined 设备自动设备注册的前提条件。

>[AZURE.NOTE]
 有关如何设置自动设备注册的最新说明，请参阅[如何安装 Windows 域的自动注册加入 Azure Active Directory 的设备](active-directory-conditional-access-automatic-device-registration-setup.md)。



## <a name="configure-the-group-policy-for-your-windows-81-domain-joined-devices"></a>配置组策略窗口 8.1 加入域设备

1. 打开服务器管理器，然后定位到**工具** > **组策略管理**。
2. 从组策略管理中，导航到对应于您要在其中启用**自动工作场所加入**的域的域节点。
3. 用鼠标右键单击**组策略对象**，然后选择**新建**。 为您的组策略对象提供一个名称，例如，**自动工作场所加入**。 单击**确定**。
4. 用鼠标右键单击新建的组策略对象，然后选择**编辑**。
5. 导航到**计算机配置** > **策略** > **管理模板** > **Windows 组件** > **联接的工作场所**。
6. 用鼠标右键单击自动联接的工作场所中的客户端计算机，然后选择**编辑**。
7. 选择已启用单选按钮，然后单击应用。 单击**确定**。
8. 现在可能到您选择的位置链接的组策略对象。 若要启用该策略，所有在您的组织的加入域 Windows 8.1 设备，链接到域组策略。

## <a name="unregistering-your-windows-81-domain-joined-devices"></a>注销您的 Windows 8.1 域加入设备

您可以按以下方法注销您加入域的 Windows 8.1 设备︰ 修改在上一节中创建的工作区加入组策略设置。 设置自动工作场所连接客户端计算机策略配置为禁用。 这将防止新设备自动联接的工作场所。

注销现有的加入域 Windows 8.1 机下面的两个选项之一︰

* 选项 1︰**注销 Windows 8.1 域加入使用 PC 设置的设备**
  1. 在 Windows 8.1 设备上，导航到**PC 设置** > **网络** > **的工作场所**
  2. 选择**离开**。
为每个域用户已登录到该计算机并已被自动加入的工作区，必须重复此过程。

* 选项 2︰ 注销 Windows 8.1 域连接的设备，使用脚本
    1. 打开命令提示符窗口 8.1 机器上，执行以下命令︰` %SystemRoot%\System32\AutoWorkplace.exe leave`
   
到计算机，必须在已签名的每个域用户的上下文中运行该命令。

##<a name="event-viewer--errors-for-windows-81-domain-joined-devices"></a>事件查看器和错误的 Windows 8.1 加入域设备

Windows 8.1 计算机上的 Windows 事件日志显示设备注册到相关的消息。 成功和不成功的事件，将查找邮件。 

事件日志找不到在事件查看器的应用程序和服务**日志**下 > **Microsoft** > **窗口 > 工作场所加入**。

##<a name="additional-details"></a>更多详细信息

使用组策略，可以计划任务在用户的上下文中运行，并会触发用户登录的系统上。 任务将自动注册的用户和设备 Azure AD 签到完毕后。 在任务计划程序库下**Microsoft**Windows 8.1 设备上找不到任务计划 > **Windows** > **联接的工作场所**。 该任务将运行并注册到该符号的所有 Active Directory 用户。 

## <a name="additional-topics"></a>其他主题
- [Azure 的活动目录设备注册概述](active-directory-conditional-access-device-registration-overview.md)
- [设备自动注册 Azure 活动目录的 Windows 10 加入域的设备](active-directory-conditional-access-automatic-device-registration.md)
- [配置 Windows 7 加入域设备的设备自动注册](active-directory-conditional-access-automatic-device-registration-windows7.md)

