<properties 
    pageTitle="教程︰ Azure Active Directory 集成与温室 |Microsoft Azure" 
    description="了解如何使用 Azure Active Directory 温室启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-greenhouse"></a>教程︰ 与温室的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和温室的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   温室单个登录订阅
  
后学完本教程，您已分配给温室 Azure AD 用户将能够到温室公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成的温室
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-greenhouse-tutorial/IC790783.png "方案")
##<a name="enabling-the-application-integration-for-greenhouse"></a>启用应用程序集成的温室
  
本部分的目的是概述如何启用应用程序集成的温室。

###<a name="to-enable-the-application-integration-for-greenhouse-perform-the-following-steps"></a>若要启用应用程序集成的温室，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-greenhouse-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-greenhouse-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-greenhouse-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-greenhouse-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**温室**。

    ![应用程序库](./media/active-directory-saas-greenhouse-tutorial/IC790784.png "应用程序库")

7.  在结果窗格中，选择**温室**，，然后单击**完成**添加应用程序。

    ![温室](./media/active-directory-saas-greenhouse-tutorial/IC790785.png "温室")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够验证到温室使用他们的帐户在 Azure 使用基于 SAML 协议联盟的广告。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**温室**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-greenhouse-tutorial/IC790786.png "配置单一登录")

2.  在**您想怎样来登录到温室的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-greenhouse-tutorial/IC790787.png "配置单一登录")

3.  在**配置应用程序 URL**页上，在**符号上 URL**文本框中，键入您的 URL 使用以下模式"*https://company.greenhouse.io*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-greenhouse-tutorial/IC790788.png "配置应用程序的 URL")

4.  在**配置单一登录温室在**页上，单击**下载元数据**，并保存在您的计算机上的本地元数据文件。

    ![配置单一登录](./media/active-directory-saas-greenhouse-tutorial/IC790789.png "配置单一登录")

5.  转发到温室支持团队该元数据文件。

    >[AZURE.NOTE] 单一登录必须启用的温室的支持团队。

6.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-greenhouse-tutorial/IC790790.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到温室，它们必须调配到温室。  
在温室，资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  以管理员身份登录到**温室**公司网站。

2.  在菜单上，单击**配置**，然后单击**用户**。

    ![用户](./media/active-directory-saas-greenhouse-tutorial/IC790791.png "用户")

3.  单击**新用户**。

    ![新用户](./media/active-directory-saas-greenhouse-tutorial/IC790792.png "新用户")

4.  在**添加新用户**部分中，执行以下步骤︰

    ![添加新用户](./media/active-directory-saas-greenhouse-tutorial/IC790793.png "添加新用户")

    1.  在**输入用户的电子邮件**文本框中，键入您要提供一个有效的 Azure Active Directory 帐户的电子邮件地址。
    2.  单击**保存**。
        
        >[AZURE.NOTE] Azure Active Directory 帐户持有人将收到一封电子邮件，包括确认该帐户之前将变为活动状态的链接。

>[AZURE.NOTE] 您可以使用任何其他温室用户帐户创建工具或 Api 来设置 AAD 用户帐户提供温室。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-greenhouse-perform-the-following-steps"></a>若要将用户分配到温室，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**温室**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-greenhouse-tutorial/IC790794.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-greenhouse-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。