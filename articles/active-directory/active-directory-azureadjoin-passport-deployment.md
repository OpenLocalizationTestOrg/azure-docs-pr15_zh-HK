<properties
    pageTitle="启用 Microsoft Windows Hello 业务为您的组织中 |Microsoft Azure"
    description="若要在您的组织中启用 Microsoft Passport 的部署说明。"
    services="active-directory"
    documentationCenter=""
    keywords="配置 Microsoft Passport，Microsoft Windows Hello 业务部署"
    authors="markusvi"
    manager="femila"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="markvi"/>


# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a>启用 Microsoft Windows Hello 业务为您的组织中

之后[将与 Azure Active Directory Windows 10 加入域的设备连接](active-directory-azureadjoin-devices-group-policy.md)，执行以下操作来启用 Microsoft Windows Hello 为企业组织中︰

1. 部署系统中心配置管理器  

2. 配置策略设置

3. 配置证书模板  




## <a name="deploy-system-center-configuration-manager"></a>部署系统中心配置管理器 

若要部署基于 Windows Hello 业务键的用户证书，您需要以下各项︰

- **系统中心配置管理器当前分支**-您需要安装 1606年或更高版本。 有关详细信息，请参阅[文档系统中心配置管理器](https://technet.microsoft.com/library/mt346023.aspx)和[系统中心配置管理器团队博客](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)。
- **公钥基础结构 (PKI)**的使用来启用 Microsoft Windows Hello 为业务用户证书，您必须制定一个 PKI。 如果您还没有，或者不想使用它的用户证书，您可以部署新的域控制器具有安装 Windows 服务器 2016年生成 10551 （或者更佳）。 为[现有域中的副本域控制器安装](https://technet.microsoft.com/library/jj574134.aspx)或[安装一个新的活动目录林，如果您正在创建一个新的环境](https://technet.microsoft.com/library/jj574166)，请按照步骤。 （Iso 可以下载[Signiant 媒体](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true)交换。）


## <a name="configure-policy-settings"></a>配置策略设置

若要配置 Microsoft Windows Hello 业务策略设置，您可以有两种选择︰

- 在 Active Directory 中的组策略 
- 系统中心配置管理器 


在 Active Directory 中的使用组策略是推荐的方法来配置 Microsoft Windows Hello 业务策略设置。 



您还使用它来部署证书时使用系统中心配置管理器是首选的方法。 这种情况下︰

- 确保兼容性更新的部署方案
- 在客户端窗口版本 1607 10 或更高的要求。

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a>配置 Microsoft Windows Hello 业务通过在 Active Directory 中的组策略

 
**步骤**如下︰

1.  打开服务器管理器，然后定位到**工具** > **组策略管理**。
2.  从组策略管理中，导航到对应于您想要启用 Azure 广告加入的域的域节点。
3.  右键单击**组策略对象**，然后选择**新建**。 为您的组策略对象名称，例如，启用 Windows Hello 业务。 单击**确定**。
4.  新的组策略对象，用鼠标右键单击，然后选择**编辑**。
5.  导航到**计算机配置** > **策略** > **管理模板** > **Windows 组件** > **业务的 Windows Hello**。
6.  右键单击**启用 Windows Hello 业务**，然后再选择**编辑**。
7.  选择**启用**选项按钮，然后单击**应用**。 单击**确定**。
8.  现在，可以将组策略对象链接到您选择的位置。 若要启用此策略的所有加入域的 Windows 10 设备在您的组织中，链接到域组策略。 例如︰
 - 将在其中放置窗口 10 加入域的计算机的 Active Directory 中特定组织单位 (OU)
 - 特定安全组包含 Windows 10 加入域的计算机将使用 Azure 广告自动注册


### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a>配置 Windows Hello 业务使用系统中心配置管理器


**步骤**如下︰


1. 打开**系统中心配置管理器**，然后定位到**资产和法规遵从性 > 合规性设置 > 访问公司资源 > Windows Hello 业务配置文件**。

    ![为业务配置 Windows Hello](./media/active-directory-azureadjoin-passport-deployment/01.png)


2. 在顶部的工具栏中，单击**创建 Windows Hello 业务配置文件**。

    ![为业务配置 Windows Hello](./media/active-directory-azureadjoin-passport-deployment/02.png)

2. 在**常规**对话框，请执行以下步骤︰

    ![为业务配置 Windows Hello](./media/active-directory-azureadjoin-passport-deployment/03.png)

    一。 在**名称**文本框中，键入您的个人资料，例如，**我的 WHfB 配置文件**的名称。

    b。 单击**下一步**。


2. 在**支持的平台**对话框中，选择平台，将配有此 Windows Hello 业务配置文件，然后单击**下一步**。

    ![为业务配置 Windows Hello](./media/active-directory-azureadjoin-passport-deployment/04.png)


2. 在**设置**对话框中，请执行以下步骤︰

    ![为业务配置 Windows Hello](./media/active-directory-azureadjoin-passport-deployment/05.png)

    一。 作为**配置 Windows Hello 业务**，选择**启用**。

    b。 为**使用受信任的平台模块 (TPM)**，请选择**需要**。 

    c。 作为**身份验证方法**，选择**基于证书的**。

    d。 单击**下一步**。



2. 在**摘要**对话框中，单击**下一步**。

2. 在**完成**对话框中，单击**关闭**。


2. 在顶部工具栏上，单击**部署**。

    ![为业务配置 Windows Hello](./media/active-directory-azureadjoin-passport-deployment/06.png)



## <a name="configure-the-certificate-profile"></a>配置证书模板 

如果使用的内部身份验证的基于证书的身份验证，您需要配置和部署证书配置文件。 此任务要求您设置 NDES 服务器和证书注册点站点角色在系统中心配置管理器。 有关详细信息，请参阅[为证书配置文件在配置管理器系统必备组件](https://technet.microsoft.com/library/dn261205.aspx)。

1. 打开**系统中心配置管理器**，然后定位到**资产和法规遵从性 > 合规性设置 > 访问公司资源 > 的证书配置文件**。


2. 选择模板具有智能卡登录扩展密钥用法 (EKU)。

在**SCEP 注册**页上的证书配置文件，您需要选择作为**密钥存储提供程序****安装到护照工作否则为失败**。



## <a name="next-steps"></a>下一步行动
* [企业的 Windows 10︰ 使用设备的工作方法](active-directory-azureadjoin-windows10-devices-overview.md)
* [扩展到 Windows 10 设备通过 Azure 活动目录加入云功能](active-directory-azureadjoin-user-upgrade.md)
* [没有通过 Microsoft Passport 密码身份验证身份](active-directory-azureadjoin-passport.md)
* [了解使用方案 Azure 广告加入](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [将加入域的设备连接到 Windows 10 经验的 Azure 广告](active-directory-azureadjoin-devices-group-policy.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)
