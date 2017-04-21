<properties 
    pageTitle="教程︰ 与合并 Thoughtworks 的 Azure Active Directory 集成 |Microsoft Azure" 
    description="了解如何使用 Thoughtworks 合并与 Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>教程︰ 与合并 Thoughtworks 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和合并 Thoughtworks 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   合并 Thoughtworks 的租户
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用用于 Thoughtworks 合并应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "方案")

##<a name="enabling-the-application-integration-for-thoughtworks-mingle"></a>启用用于 Thoughtworks 合并应用程序集成
  
本部分的目的是概述如何启用应用程序集成为 Thoughtworks 合并。

###<a name="to-enable-the-application-integration-for-thoughtworks-mingle-perform-the-following-steps"></a>若要启用用于 Thoughtworks 合并应用程序集成，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**thoughtworks 合并**。

    ![应用程序库](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "应用程序库")

7.  在结果窗格中，选择**Thoughtworks 合并**，然后单击**完成**添加应用程序。

    ![Thoughtworks 合并](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "Thoughtworks 合并")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Thoughtworks 合并。  
作为此过程的一部分，您需要将证书传到 Thoughtworks 合并。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 经典门户， **Thoughtworks 合并**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "配置单一登录")

2.  在**您想怎样用户登录 Thoughtworks 合并到**页面上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "配置单一登录")

3.  在**配置应用程序 URL**页，在**合并 Thoughtworks 的租户 URL**文本框中，键入您的 URL 使用以下模式"*http://company.mingle.thoughtworks.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "配置应用程序的 URL")

4.  在**配置单一登录在 Thoughtworks 合并**页上，单击下载元数据，然后将其保存在您的计算机上。

    ![配置单一登录](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "配置单一登录")

5.  以管理员身份登录到您**Thoughtworks 合并**的公司网站。

6.  单击**管理**选项卡，然后单击**SSO 配置**。

    ![SSO 配置](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "SSO 配置")

7.  在**SSO 配置**部分中，执行以下步骤︰

    ![SSO 配置](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "SSO 配置")

    1.  若要上载的元数据文件，请单击**选择文件**。
    2.  单击**保存更改**。

8.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "配置单一登录")

##<a name="configuring-user-provisioning"></a>配置用户设置
  
AAD 用户能够登录，他们必须使用 Azure Active Directory 用户名的 Thoughtworks 合并应用设置。  
在单人 Thoughtworks 的情况下调配是手动任务。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您 Thoughtworks 合并的公司网站。

2.  单击**配置文件**。

    ![第一个项目](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "第一个项目")

3.  单击**管理**选项卡，然后单击**用户**。

    ![用户](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "用户")

4.  单击**新用户**。

    ![新用户](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "新用户")

5.  在**新建用户**对话框页，请执行以下步骤︰

    ![新用户](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "新用户")

    1.  键入**登录名称**、**显示名称**、**选择密码**、**确认密码**有效 AAD 帐户，您希望提供到相关的文本框。
    2.  作为**用户类型**，选择**完整的用户**。
    3.  单击**创建此配置文件**。

>[AZURE.NOTE] 您可以使用任何其他 Thoughtworks 合并用户帐户创建工具或 Api 提供的 Thoughtworks 合并来设置 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-thoughtworks-mingle-perform-the-following-steps"></a>若要将用户分配到 Thoughtworks 合并，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Thoughtworks 合并**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
