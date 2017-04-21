<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 TimeOffManager |Microsoft Azure" 
    description="了解如何使用 TimeOffManager Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>教程︰ 与 TimeOffManager 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 TimeOffManager 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   TimeOffManager-启用单一登录的订阅
  
后学完本教程，您已分配给 TimeOffManager Azure AD 用户将能够到 TimeOffManager 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 TimeOffManager
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-timeoffmanager-tutorial/IC795909.png "方案")

##<a name="enabling-the-application-integration-for-timeoffmanager"></a>启用应用程序集成为 TimeOffManager
  
本部分的目的是概述如何启用应用程序集成为 TimeOffManager。

###<a name="to-enable-the-application-integration-for-timeoffmanager-perform-the-following-steps"></a>若要启用应用程序集成为 TimeOffManager，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-timeoffmanager-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-timeoffmanager-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-timeoffmanager-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-timeoffmanager-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**TimeOffManager**。

    ![应用程序库](./media/active-directory-saas-timeoffmanager-tutorial/IC795910.png "应用程序库")

7.  在结果窗格中，选择**TimeOffManager**，，然后单击**完成**添加应用程序。

    ![TimeOffManager](./media/active-directory-saas-timeoffmanager-tutorial/IC795911.png "TimeOffManager")

##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 TimeOffManager。  
作为此过程的一部分，您都需要将 base 64 编码的证书上传到 TimeOffManager 租户。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**TimeOffManager**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-timeoffmanager-tutorial/IC795912.png "配置单一登录")

2.  在**您想怎样来登录到 TimeOffManager 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-timeoffmanager-tutorial/IC795913.png "配置单一登录")

3.  **配置应用程序 URL**页面上的**TimeOffManager 回复 URL**文本框中，键入您的 TimeOffManager AssertionConsumerService URL (例如:"*示例︰ https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company\_id = IC34216*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-timeoffmanager-tutorial/IC795914.png "配置应用程序的 URL")

    您可以回复 URL 从 TimeOffManager 单一登录设置页。

    ![单一登录设置](./media/active-directory-saas-timeoffmanager-tutorial/IC795915.png "单一登录设置")

4.  在**配置单一登录 TimeOffManager 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-timeoffmanager-tutorial/IC795916.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 TimeOffManager 公司网站。

6.  转到**帐户\>帐户选项\>单一登录设置**。

    ![单一登录设置](./media/active-directory-saas-timeoffmanager-tutorial/IC795917.png "单一登录设置")

7.  在**单一登录设置**部分中，执行以下步骤︰

    ![单一登录设置](./media/active-directory-saas-timeoffmanager-tutorial/IC795918.png "单一登录设置")

    一。  创建您已下载的证书的**Base 64 编码**的文件。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    b。  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将整个证书粘贴到文本框**的 X.509 证书**。
    
    c。  在 Azure 经典门户中，在**配置单一登录在 TimeOffManager**对话框页上，将**颁发者 URL**值，复制，然后将其粘贴到**Idp 颁发者**文本框。
    
    d。  在 Azure 经典门户中，在**配置单一登录在 TimeOffManager**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**IdP 端点 URL**文本框。
    
    电子。  作为**实施 SAML**，选择**否**。
    

    f。  作为**自动创建用户**，选择**是**。
    
    g。  在 Azure 经典门户中，在**配置单一登录在 TimeOffManager**对话框页面上，复制的**远程注销 URL**值，，然后将其粘贴到**注销 URL**文本框。
    
    h。  单击**保存更改**。

8.  在 Azure 经典门户中，在**配置单一登录在 TimeOffManager**对话框页面上，选择单一登录配置进行确认，，然后单击**完成**。

    ![配置单一登录](./media/active-directory-saas-timeoffmanager-tutorial/IC795919.png "配置单一登录")

9.  在顶部菜单中，单击**属性**以打开**SAML 令牌的属性**对话框。

    ![属性](./media/active-directory-saas-timeoffmanager-tutorial/IC795920.png "属性")

10. 若要添加所需的属性映射，请执行以下步骤︰

    ![saml 令牌的属性](./media/active-directory-saas-timeoffmanager-tutorial/123.png "saml 令牌的属性")

  	|属性名称|属性值|
  	|---|---|
  	|电子邮件|User.mail|
  	|名字字段|User.givenname|
  	|姓氏|User.surname|

    一。  上表中每个数据行，请单击**添加用户属性**。

    b。  在**属性名称**文本框中，键入该行显示的属性名称。

    c。  在**属性值**文本框中，选择的行中显示的属性值。

    d。  单击**完成**。

11. 单击**应用更改**。

##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 TimeOffManager，他们必须为 TimeOffManager 配置。  
TimeOffManager 只是在时间用户提供支持。 没有为您的操作项。  
在首次登录上使用单一登录过程中自动添加用户。

>[AZURE.NOTE] 您可以使用任何其他 TimeOffManager 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 TimeOffManager 提供。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-timeoffmanager-perform-the-following-steps"></a>要将用户分配到 TimeOffManager 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**TimeOffManager**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-timeoffmanager-tutorial/IC795922.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-timeoffmanager-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
