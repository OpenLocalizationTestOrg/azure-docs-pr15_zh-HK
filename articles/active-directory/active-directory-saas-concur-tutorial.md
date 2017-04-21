<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Concur |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory Concur 启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-concur"></a>教程︰ 与 Concur 的 Azure Active Directory 集成  


本教程的目的是显示的 Azure，Concur 集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   在 Concur 租户

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Concur
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-concur-tutorial/IC769766.png "方案")

>[AZURE.NOTE] Concur 订购的联盟通过 SAML SSO 配置为单独的任务，您必须联系 Concur 来执行。

##<a name="enabling-the-application-integration-for-concur"></a>启用应用程序集成为 Concur

本部分的目的是概述如何启用 Concur 用于应用程序集成。

###<a name="to-enable-the-application-integration-for-concur-perform-the-following-steps"></a>若要启用 Concur 用于应用程序集成，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-concur-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-concur-tutorial/IC700994.png "应用程序")

4.  若要打开**应用程序库**，**添加应用程序**，请单击，然后单击**添加我的组织使用的应用程序**。

    ![要做什么？](./media/active-directory-saas-concur-tutorial/IC700995.png "要做什么？")

5.  在**搜索框**中，键入**Concur**。

    ![应用程序库](./media/active-directory-saas-concur-tutorial/IC721727.png "应用程序库")

6.  在结果窗格中，选择**Concur**，，然后单击**完成**添加应用程序。

    ![同时存在](./media/active-directory-saas-concur-tutorial/IC721728.png "同时存在")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Concur。

>[AZURE.NOTE] Concur 订购的联盟通过 SAML SSO 配置为单独的任务，您必须联系 Concur 来执行。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 的经典门户， **Concur**应用程序集成在页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-concur-tutorial/IC769767.png "配置单一登录")

2.  在**您想怎样用户能够登录到 Concur**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-concur-tutorial/IC769768.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**同时存在符号在 URL**文本框中，键入您 concur 租户登录 URL，并单击**下一步**: 

    ![配置登录 URL](./media/active-directory-saas-concur-tutorial/IC769769.png "配置登录 URL")

4.  在**配置单一登录 Concur 在**页上，请执行以下步骤。

    ![配置登录 URL](./media/active-directory-saas-concur-tutorial/IC769770.png "配置登录 URL")

    1.  单击下载元数据，然后再安全数据文件到您的计算机。
    2.  与为您的租户配置 SSO Concur 支持团队联系。
    3.  选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。  

    >[AZURE.NOTE] Concur 订购的联盟通过 SAML SSO 配置为单独的任务，您必须联系 Concur 来执行。

##<a name="configuring-user-provisioning"></a>配置用户设置

本部分的目的是概述如何启用 Concur 到 Active Directory 用户帐户的资源调配。

若要启用应用程序，在支出服务中，那里必须正确安装和使用 Web 服务管理配置文件。 不只是添加到现有的管理员配置文件用于管理功能，T & E WS 管理角色。

同时存在的顾问或客户端管理员必须创建一个不同的 Web 服务管理员配置文件和客户端的管理员必须使用此配置文件 Web 服务管理员功能 （例如，启用应用程序）。 这些配置文件必须保持独立于客户端管理员的日常 T & E 管理员配置文件 （T & E 管理员配置文件不应具有分配的 WSAdmin 角色）。

创建要用于启用应用程序的配置文件时，用户配置文件字段中输入客户端管理器的名称。 这会将所有权分配给配置文件。创建配置文件后，必须使用此配置文件来为一个合作伙伴应用程序 Web 服务菜单中单击"*启用*"按钮登录客户端。

由于以下原因，此操作不应与他们使用普通的 T & E 管理配置文件。

1.  客户端必须是在对话窗口中显示某个应用程序启用后，单击"*是*"。 请单击确认客户端愿意合作伙伴应用程序才能访问其数据，因此您或伙伴不能单击是按钮。
2.  如果已启用应用程序客户端管理器使用 T & E 管理员配置文件离开了公司 （导致非活动状态的配置文件），使用配置文件将无法正常，直到应用程序启用了另一个活动的 WS 管理配置文件启用任何应用程序。 这是为什么呢来创建不同的 WS 管理配置文件。
3.  如果管理员离开公司，到 WS 管理配置文件相关联的名称如果需要而又不影响已启用的应用程序，因为该配置文件不需要禁用可以变为替换管理员

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  登录到**Concur**租户。

2.  从**管理**菜单中，选择**Web 服务**。

    ![Concur 租户](./media/active-directory-saas-concur-tutorial/IC721729.png "Concur 租户")

3.  在左侧，从**Web 服务**窗格中，选择**启用应用程序合作伙伴**。

    ![使合作伙伴应用程序](./media/active-directory-saas-concur-tutorial/IC721730.png "使合作伙伴应用程序")

4.  **启用应用程序**列表中，选择**Azure Active Directory**，，然后单击**启用**。

    ![Microsoft Azure 活动目录](./media/active-directory-saas-concur-tutorial/IC721731.png "Microsoft Azure 活动目录")

5.  单击**是**以**确认操作**对话框关闭。

    ![确认操作](./media/active-directory-saas-concur-tutorial/IC721732.png "确认操作")

6.  在 Azure 经典门户中，从应用程序列表来打开**Concur**对话框页面中选择**Concur** 。

7.  要打开**配置用户设置**对话框页，请单击**配置用户设置**。

8.  输入用户名和密码 Concur 管理员，然后单击**下一步**。

9.  若要完成配置，在**确认**页上，单击**完成**按钮。

您现在可以创建一个测试帐户、 等待 10 分钟并验证帐户已被同步到 Concur。
##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-concur-perform-the-following-steps"></a>若要将用户分配到 Concur，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Concur**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-concur-tutorial/IC769771.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-concur-tutorial/IC767830.png "是")

您现在应等待 10 分钟，并验证该帐户已被同步到 Concur。

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
