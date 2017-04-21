<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 LogicMonitor |Microsoft Azure" 
    description="了解如何使用 LogicMonitor Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>教程︰ 与 LogicMonitor 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 LogicMonitor 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   LogicMonitor 租户
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 LogicMonitor
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "方案")
##<a name="enabling-the-application-integration-for-logicmonitor"></a>启用应用程序集成为 LogicMonitor
  
本部分的目的是概述如何启用应用程序集成为 LogicMonitor。

###<a name="to-enable-the-application-integration-for-logicmonitor-perform-the-following-steps"></a>若要启用应用程序集成为 LogicMonitor，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**logicmonitor**。

    ![应用程序库](./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "应用程序库")

7.  在结果窗格中，选择**LogicMonitor**，，然后单击**完成**添加应用程序。

    ![LogicMonitor](./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 LogicMonitor。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**LogicMonitor**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "配置单一登录")

2.  在**您想怎样来登录到 LogicMonitor 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "配置单一登录")

3.  **配置应用程序 URL**页面上的**符号在 URL**文本框中，键入用户用于登录到 LogicMonitor 您的 URL \(e、 g:"*http://company.logicmonitor.com*"\)，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "配置应用程序的 URL")

4.  在**配置单一登录 LogicMonitor 在**页上，单击**下载元数据**，然后将其保存在您的计算机上。

    ![配置单一登录](./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "配置单一登录")

5.  以管理员身份登录到您的**LogicMonitor**公司网站。

6.  在菜单上，单击**设置**。

    ![设置](./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "设置")

7.  在左侧导航受戒，单击**单一登录**

    ![单一登录](./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "单一登录")

8.  在**单一登录 (SSO) 设置**部分中，执行以下步骤︰

    ![单一登录设置](./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "单一登录设置")

    1.  选择**启用单一登录**。
    2.  作为**默认角色分配**中，选择**只读**。
    3.  在记事本中打开下载元数据文件，然后将文件的内容粘贴到文本框中**的身份提供程序元数据**。
    4.  单击**保存更改**。

9.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
对于 AAD 用户能够登录，他们必须使用 Azure Active Directory 用户名的 LogicMonitor 应用设置。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的 LogicMonitor 公司网站。

2.  在顶部菜单中，单击**设置**，然后单击**角色和用户**。

    ![角色和用户](./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "角色和用户")

3.  单击**添加**。

4.  在**添加帐户**部分中，执行以下步骤︰

    ![添加帐户](./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "添加帐户")

    1.  键入您要提供到相关文本框的 Azure Active Directory 用户的**用户名**、**电子邮件**、**密码**和**重新键入密码**值。
    2.  选择**角色**、**查看权限**和**状态**。
    3.  单击**提交**。

>[AZURE.NOTE]您可以使用任何其他 LogicMonitor 用户帐户创建工具或 Api 提供的 LogicMonitor 设置 Azure 活动目录的用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-logicmonitor-perform-the-following-steps"></a>要将用户分配到 LogicMonitor 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**LogicMonitor**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。




