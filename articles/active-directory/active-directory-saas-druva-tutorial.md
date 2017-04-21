<properties 
    pageTitle="教程︰ Azure Active Directory 集成集成与 Druva |Microsoft Azure" 
    description="了解如何使用 Druva Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-druva"></a>教程︰ 与 Druva 的 Azure Active Directory 集成集成

本教程的目的是显示 Azure 和 Druva 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Druva-启用单一登录的订阅

后学完本教程，您已分配给 Druva Azure AD 用户将能够到 Druva 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。

在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Druva
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-druva-tutorial/IC795084.png "方案")
##<a name="enabling-the-application-integration-for-druva"></a>启用应用程序集成为 Druva

本部分的目的是概述如何启用应用程序集成为 Druva。

###<a name="to-enable-the-application-integration-for-druva-perform-the-following-steps"></a>若要启用应用程序集成为 Druva，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-druva-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-druva-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-druva-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-druva-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**Druva**。

    ![应用程序库](./media/active-directory-saas-druva-tutorial/IC795085.png "应用程序库")

7.  在结果窗格中，选择**Druva**，，然后单击**完成**添加应用程序。

    ![Druva](./media/active-directory-saas-druva-tutorial/IC795086.png "Druva")
##<a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Druva。  
作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

Druva 应用程序需要以特定的格式，这就需要你对**saml 令牌属性**配置中添加自定义属性映射 SAML 断言。  
下面的屏幕快照显示此示例。

![SAML 令牌的属性](./media/active-directory-saas-druva-tutorial/IC795087.png "SAML 令牌的属性")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  在 Azure 传统门户网站，在**Druva**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录](./media/active-directory-saas-druva-tutorial/IC795027.png "配置单一登录")

2.  在**您想怎样来登录到 Druva 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-druva-tutorial/IC795088.png "配置单一登录")

3.  在**配置应用程序 URL**页面上的**Druva 号上的 URL**文本框中，键入用户用于登录到您的 Druva 应用程序的 URL (例如:"*https://cloud.druva.com/home/*")，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-druva-tutorial/IC795089.png "配置应用程序的 URL")

4.  在**配置单一登录 Druva 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-druva-tutorial/IC795090.png "配置单一登录")

5.  在不同的 web 浏览器窗口中，以管理员身份登录到 Druva 公司网站。

6.  转到**管理\>设置**。

    ![设置](./media/active-directory-saas-druva-tutorial/IC795091.png "设置")

7.  在单一登录设置对话框中，请执行以下步骤︰

    ![Singl 登录设置](./media/active-directory-saas-druva-tutorial/IC795092.png "Singl 登录设置")

    1.  在 Azure 经典门户中，在**配置单一登录在 Druva**对话框页复制的**远程登录 URL**值，，然后将其粘贴到**ID 提供程序登录 URL**文本框。
    2.  在 Azure 经典门户中，在**配置单一登录在 Druva**对话框页面上，复制的**远程注销 URL**值，，然后将其粘贴到**ID 提供程序注销 URL**文本框。
    3.  创建您已下载的证书的**base 64 编码**的文件。  

        >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)

    4.  在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**ID 提供程序证书**文本框
    5.  若要打开**设置**页面，单击**保存**。

8.  在**设置**页上，单击**生成 SSO 标记**。

    ![设置](./media/active-directory-saas-druva-tutorial/IC795093.png "设置")

9.  在**单一登录身份验证令牌**的对话框中，请执行以下步骤︰

    ![SSO 标记](./media/active-directory-saas-druva-tutorial/IC795094.png "SSO 标记")

    1.  单击**复制**。
    2.  单击**关闭**。

10. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-druva-tutorial/IC795095.png "配置单一登录")

11. 在顶部菜单中，单击**属性**以打开**SAML 令牌的属性**对话框。

    ![属性](./media/active-directory-saas-druva-tutorial/IC795096.png "属性")

12. 若要添加所需的属性映射，请执行以下步骤︰

  	|属性名称|属性值|
  	|---|---|
  	|insync\_身份验证\_令牌|<*剪贴板值*>|

    1.  上表中每个数据行，请单击**添加用户属性**。
    2.  在**属性名称**文本框中，键入该行显示的属性名称。
    3.  在**属性值**文本框中，键入该行所显示的属性值。
    4.  单击**完成**。

13. 单击**应用更改**。
##<a name="configuring-user-provisioning"></a>配置用户设置

为了使 Azure AD 用户能够登录到 Druva，他们必须到 Druva 配置。  
对于 Druva，资源调配是手动任务。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的**Druva**公司网站。

2.  转到**管理\>用户**。

    ![管理用户](./media/active-directory-saas-druva-tutorial/IC795097.png "管理用户")

3.  单击**新建**。

    ![管理用户](./media/active-directory-saas-druva-tutorial/IC795098.png "管理用户")

4.  在创建新用户对话框中，请执行以下步骤︰

    ![创建新](./media/active-directory-saas-druva-tutorial/IC795099.png "创建新")

    1.  键入的电子邮件地址和要到相关文本框设置有效 Azure Active Directory 用户帐户的名称。
    2.  单击**创建用户**。

>[AZURE.NOTE] 您可以使用任何其他 Druva 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Druva 提供。

##<a name="assigning-users"></a>分配用户

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-druva-perform-the-following-steps"></a>要将用户分配到 Druva 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Druva**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-druva-tutorial/IC795100.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-druva-tutorial/IC767830.png "是")

如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。
