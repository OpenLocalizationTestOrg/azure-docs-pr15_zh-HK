<properties 
    pageTitle="教程︰ Azure Active Directory 集成源代码管理生命周期与 |Microsoft Azure" 
    description="了解如何使用源代码管理生命周期 Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>教程︰ Azure Active Directory 集成与源代码管理生命周期
  
本教程的目的是显示 Azure 和源代码管理生命周期的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   源代码管理生命周期-启用单一登录的订阅
  
后学完本教程，已分配给源代码管理生命周期的 Azure AD 用户将能够到单一登录到应用程序在您的源代码管理生命周期的公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成的源代码管理生命周期
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "方案")
##<a name="enabling-the-application-integration-for-scc-lifecycle"></a>启用应用程序集成的源代码管理生命周期
  
本部分的目的是概述如何启用应用程序集成的源代码管理生命周期。

###<a name="to-enable-the-application-integration-for-scc-lifecycle-perform-the-following-steps"></a>若要启用应用程序集成的源代码管理生命周期，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**源代码管理生命周期**。

    ![应用程序库](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "应用程序库")

7.  在结果窗格中，选择**源代码管理生命周期**，然后单击**完成**添加应用程序。

    ![源代码管理生命周期](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "源代码管理生命周期")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到源代码管理生命周期。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 经典门户中，**源代码管理生命周期**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "配置单一登录")

2.  在**您希望如何登录到源代码管理生命周期的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**符号上 URL**文本框中，键入用户用于登录到源代码管理生命周期应用程序使用以下模式"*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*"的 URL，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "配置应用程序的 URL")

4.  在**配置单一登录源代码管理生命周期在**页上，单击**下载元数据**，并保存在您的计算机上的本地元数据文件。

    ![配置单一登录](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "配置单一登录")

5.  转发到源代码管理生命周期支持团队该元数据文件。

    >[AZURE.NOTE]单一登录已启用源代码管理生命周期的支持团队。

6.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到源代码管理生命周期，他们必须到源代码管理生命周期设置。
  
没有要配置用户的资源调配到源代码管理生命周期操作项。  
当分派的用户试图登录到源代码管理生命周期时，如有必要，将自动创建 SCC 生命周期帐户。

>[AZURE.NOTE]您可以使用任何其他源代码管理生命周期用户帐户创建工具或 Api 来设置 AAD 用户帐户提供源代码管理生命周期。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-scc-lifecycle-perform-the-following-steps"></a>若要将用户分配到源代码管理生命周期，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**源代码管理生命周期**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。