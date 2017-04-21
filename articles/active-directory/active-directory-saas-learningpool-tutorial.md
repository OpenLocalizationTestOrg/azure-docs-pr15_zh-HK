<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Learningpool |Microsoft Azure" 
    description="了解如何使用 Learningpool Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-learningpool"></a>教程︰ 与 Learningpool 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 Learningpool 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Learningpool-启用单一登录的订阅
  
后学完本教程，您已分配给 Learningpool Azure AD 用户将能够到 Learningpool 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Learningpool
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-learningpool-tutorial/IC791166.png "方案")
##<a name="enabling-the-application-integration-for-learningpool"></a>启用应用程序集成为 Learningpool
  
本部分的目的是概述如何启用应用程序集成为 Learningpool。

###<a name="to-enable-the-application-integration-for-learningpool-perform-the-following-steps"></a>若要启用应用程序集成为 Learningpool，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-learningpool-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-learningpool-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-learningpool-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-learningpool-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Learningpool**。

    ![应用程序库](./media/active-directory-saas-learningpool-tutorial/IC795073.png "应用程序库")

7.  在结果窗格中，选择**Learningpool**，，然后单击**完成**添加应用程序。

    ![Learningpool](./media/active-directory-saas-learningpool-tutorial/IC809577.png "Learningpool")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Learningpool。
  
Learningpool 应用程序需要以特定的格式，这就需要你对**saml 令牌属性**配置中添加自定义属性映射 SAML 断言。  
下面的屏幕快照显示此示例。

![SAML 令牌的属性](./media/active-directory-saas-learningpool-tutorial/IC795074.png "SAML 令牌的属性")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 经典门户中， **Learningpool**应用程序集成在页上，在菜单上，单击**属性**以打开**SAML 令牌的属性**对话框。

    ![属性](./media/active-directory-saas-learningpool-tutorial/IC795075.png "属性")

2.  若要添加所需的属性映射，请执行以下步骤︰

    ###  

  	|属性名称                |属性值            |
  	|------------------------------|---------------------------|

     urn: oid:1.2.840.113556.1.4.221 | User.userprincipalname
  	|-------------------------------|--------------------------|  
     urn: oid:2.5.4.42|User.givenname   
  	|urn: oid:0.9.2342.19200300.100.1.3|User.mail
  	|urn: oid:2.5.4.4|User.surname

    1.  上表中每个数据行，请单击**添加用户属性**。
    2.  在**属性名称**文本框中，键入该行显示的属性名称。
    3.  从**属性值**列表中，选择的行中显示的属性值。
    4.  单击**完成**。

3.  单击**应用更改**。

4.  在浏览器中，单击**后**再次打开**快速入门**对话框中。

5.  **配置单一登录**，打开**配置单一登录**对话框中单击。

    ![配置 Singel 登录](./media/active-directory-saas-learningpool-tutorial/IC795076.png "配置 Singel 登录")

6.  在**您想怎样来登录到 Learningpool 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-learningpool-tutorial/IC795077.png "配置单一登录")

7.  在**配置应用程序 URL**页面上的**Learningpool 号上的 URL**文本框中，键入用户用于登录到您的 Learningpool 应用程序的 URL (例如︰ https://parliament.preview.learningpool.com/auth/shibboleth/index.php)，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-learningpool-tutorial/IC795078.png "配置应用程序的 URL")

8.  在**配置单一登录 Learningpool 在**页面上，要下载元数据，请单击**下载元数据**，然后保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-learningpool-tutorial/IC795079.png "配置单一登录")

9.  转发到 Learningpool 的支持团队该元数据文件。

    >[AZURE.NOTE]单一登录必须启用的 Learningpool 的支持团队。

10. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-learningpool-tutorial/IC795080.png "配置单一登录")
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 Learningpool，他们必须到 Learningpool 配置。
  
没有为您配置用户供应到 Learningpool 操作项。  
用户需要创建的 Learningpool 的支持团队。

>[AZURE.NOTE]您可以使用任何其他 Learningpool 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Learningpool 提供。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-learningpool-perform-the-following-steps"></a>要将用户分配到 Learningpool 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Learningpool**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-learningpool-tutorial/IC795081.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-learningpool-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。