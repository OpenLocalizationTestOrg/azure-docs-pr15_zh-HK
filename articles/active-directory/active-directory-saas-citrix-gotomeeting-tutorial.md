<properties 
    pageTitle="教程︰ Azure Active Directory 集成使用 Citrix GoToMeeting |Microsoft Azure" 
    description="了解如何使用 Citrix GoToMeeting Azure Active Directory 以启用单一登录、 自动化资源调配，和更多。" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a>教程︰ 使用 citrix 服务器 GoToMeeting 的 Azure Active Directory 集成  
适用于︰ Azure

>[AZURE.TIP]反馈，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=522412)。

本教程的目的是显示 Azure 和 citrix 服务器 GoToMeeting 的集成。 在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   在 Citrix GoToMeeting 租户

在本教程中介绍的方案由以下构造块组成︰

1.  启用用于 Citrix GoToMeeting 应用程序集成
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![配置](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768996.png "配置")



##<a name="enabling-the-application-integration-for-citrix-gotomeeting"></a>启用用于 Citrix GoToMeeting 应用程序集成

本部分的目的是概述如何启用用于 Citrix GoToMeeting 应用程序集成。

###<a name="to-enable-the-application-integration-for-citrix-gotomeeting-perform-the-following-steps"></a>若要启用用于 Citrix GoToMeeting 应用程序集成，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序库中](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749322.png "添加应用程序库中")

6.  在**搜索框**中，键入**Citrix GoToMeeting**。

    ![Citrix GoToMeeting](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701006.png "Citrix GoToMeeting")

7.  在结果窗格中，选择**citrix 服务器 GoToMeeting**，，然后单击**完成**添加应用程序。

    ![Citrix GoToMeeting](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701012.png "Citrix GoToMeeting")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 citrix 服务器 GoToMeeting。  
作为此过程的一部分，您都需要将 base 64 编码的证书传到 Citrix GoToMeeting 租户。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在**Citrix GoToMeeting**应用程序集成页上，单击**配置单一登录**以打开对话框中**配置单个符号上**。

    ![启用单一登录](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768997.png "启用单一登录")

2.  **您希望如何对 citrix 服务器 GoToMeeting 上注册用户**在页上，选择**Microsoft Azure AD 单一登录**。

    ![配置单一登录](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768998.png "配置单一登录")


3. 在**配置应用程序设置**页上单击**下一步**。 

    ![启用单一登录](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689981.png "启用单一登录")

4.  在**配置单一登录在 Citrix GoToMeeting**页面上，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768999.png "配置单一登录")

5.  在不同的浏览器窗口中，登录到您 Citrix 组织中心 ([https://account.citrixonline.com/organization/administration/](https://account.citrixonline.com/organization/administration/))。

6. 单击**标识提供程序**选项卡，然后执行以下步骤︰  

    ![SAML 安装程序](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML 安装程序")

    一。 选择**手动**

    
    b。 在 Azure 经典门户中，在**配置单一登录在 Citrix GoToMeeting**对话框页复制的**登录页的 URL**值，，然后将其粘贴到**登录页的 URL**文本框。 

    
    c。 在 Azure 经典门户中，在**配置单一登录在 Citrix GoToMeeting**对话框页面上的**Sign-Out 页面 URL**值，复制，然后将其粘贴到**注销页 URL**文本框。

    
    d。 在 Azure 经典门户中，在**配置单一登录在 Citrix GoToMeeting**对话框页面上，**实体 ID**值，复制，然后将其粘贴到**标识提供程序实体 ID**文本框。

   
    电子。 若要上载您已下载的证书，请单击**上载证书**。

    
    f。 单击**保存**。

6.  在 Azure 的传统门户网站，选择单一登录配置进行确认，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769000.png "配置单一登录")


7. 在**单一登录确认**页上，单击**完成**。

    ![SAML 安装程序](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689982.png "SAML 安装程序")





##<a name="configuring-user-provisioning"></a>配置用户设置

本部分的目的是概述如何启用 Active Directory 用户帐户与 citrix 服务器 GoToMeeting 的资源调配。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**Citrix GoToMeeting**应用程序集成页上，单击**配置用户设置**以打开**配置用户设置**对话框。

    ![配置用户设置](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769001.png "配置用户设置")

2.  在**设置和管理凭据**页中，请执行以下步骤︰

    ![配置用户设置](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769002.png "配置用户设置")

    一。 在**Citrix GoToMeeting 管理员用户名**文本框中，键入管理员的用户名。

    
    b。 在**Citrix GoToMeeting 管理员密码**文本框中，管理员的密码。

    
    c。 单击**下一步**。

3.  在**确认**页上，单击以保存配置的复选标记。

4.  单击**验证**按钮，以验证您的配置。


##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-citrix-gotomeeting-perform-the-following-steps"></a>若要将用户分配到 citrix 服务器 GoToMeeting，请执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Citrix GoToMeeting**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769003.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC767830.png "是")

现在应该等待 10 分钟，并验证该帐户已被同步到收存箱的业务。

作为第一个验证步骤，您可以检查供应状态，通过单击在**Citrix GoToMeeting**应用程序集成页面上 Azure 的传统门户网站在 D 中的仪表板。

![仪表板](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769004.png "仪表板")

由相关的状态指示已成功完成资源调配周期用户︰

![集成状态](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769005.png "集成状态")

如果您想要测试单个登录设置，打开后盖。

关于访问面板的详细信息，请参阅[访问权限面板简介](https://msdn.microsoft.com/library/dn308586)。
