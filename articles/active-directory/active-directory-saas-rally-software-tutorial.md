<properties 
    pageTitle="教程︰ Azure Active Directory 集成与位置软件 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory 位置软件启用单一登录、 自动化资源调配，以及更多 ！" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-rally-software"></a>教程︰ Azure Active Directory 集成与位置的软件
  
本教程的目的是显示 Azure 和位置软件的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   位置的软件租户
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用位置的软件应用程序的集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-rally-software-tutorial/IC769525.png "方案")
##<a name="enabling-the-application-integration-for-rally-software"></a>启用位置的软件应用程序的集成
  
本部分的目的是概述如何启用位置的软件应用程序集成。

###<a name="to-enable-the-application-integration-for-rally-software-perform-the-following-steps"></a>要启用位置的软件应用程序集成，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-rally-software-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-rally-software-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-rally-software-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-rally-software-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**rally**。

    ![应用程序库](./media/active-directory-saas-rally-software-tutorial/IC769526.png "应用程序库")

7.  在结果窗格中，选择**位置的软件**，，然后单击**完成**添加应用程序。

    ![位置软件](./media/active-directory-saas-rally-software-tutorial/IC769527.png "位置软件")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到位置的软件。  
作为此过程的一部分，您需要将证书传到位置的软件。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**位置的软件**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-rally-software-tutorial/IC749323.png "配置单一登录")

2.  **如何将用户注册到位置要**在页上，选择**Microsoft Azure AD 单一登录**，，然后单击**下一步**。

    ![Microsoft Azure 广告单登录](./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure 广告单登录")

3.  **配置应用程序 URL**页面上的**位置软件租户 URL**文本框中，键入您的 URL 使用以下模式"*https://\<租户名称\>。 rally.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-rally-software-tutorial/IC769529.png "配置应用程序的 URL")

4.  在**配置单一登录位置在**页面上，单击下载元数据，然后将其保存在您的计算机上。

    ![配置单一登录](./media/active-directory-saas-rally-software-tutorial/IC769530.png "配置单一登录")

5.  登录到**位置软件**租户。

6.  在顶部的工具栏中，单击**设置**，然后选择**订阅**。

    ![订阅](./media/active-directory-saas-rally-software-tutorial/IC769531.png "订阅")

7.  单击在右侧顶部工具栏中的**动作**按钮，然后选择**编辑预订**。

8.  在**订阅**对话框页上，执行下面的步骤，然后单击**保存并关闭**:

    ![身份验证](./media/active-directory-saas-rally-software-tutorial/IC769542.png "身份验证")

    1.  从身份验证下拉列表中选择**位置或 SSO 身份验证**
    2.  在 Azure 经典门户中，在**配置单一登录位置软件**对话框页的**标识提供程序 ID**值，复制，然后将其粘贴到**标识提供程序 URL**文本框
    3.  在 Azure 传统门户网站，在**配置单一登录位置软件**对话框页复制的**远程注销 URL**值。

9.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-rally-software-tutorial/IC769547.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
对于 AAD 用户能够登录，他们必须使用 Azure Active Directory 用户名称的位置软件应用设置。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  登录到位置软件租户。

2.  转到**安装程序\>用户**，然后单击**+ 添加新建**。

    ![用户](./media/active-directory-saas-rally-software-tutorial/IC781039.png "用户")

3.  在新建用户文本框中，键入名称，然后单击**添加的详细信息**。

4.  在**创建用户**部分中，执行以下步骤︰

    ![创建用户](./media/active-directory-saas-rally-software-tutorial/IC781040.png "创建用户")

    1.  在**用户名**文本框中，键入您要提供的 Azure AD 用户的名称。
    2.  在**电子邮件地址**文本框中，键入您要提供的 Azure AD 用户的电子邮件地址。
    3.  单击**保存并关闭**。

>[AZURE.NOTE]您可以使用任何其他位置软件用户帐户创建工具或 Api 所提供位置的软件来设置 AAD 用户帐户。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-rally-software-perform-the-following-steps"></a>若要将用户分配到位置的软件，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**位置的软件**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-rally-software-tutorial/IC769548.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-rally-software-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。




