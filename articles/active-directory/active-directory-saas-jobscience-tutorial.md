<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 Jobscience |Microsoft Azure" 
    description="了解如何使用 Jobscience Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
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

#<a name="tutorial-azure-active-directory-integration-with-jobscience"></a>教程︰ 与 Jobscience 的 Azure Active Directory 集成
  
本教程的目的是显示 Azure 和 Jobscience 的集成。  
在本教程中介绍的方案假定您已经有以下各项︰

-   有效的 Azure 订购
-   Jobscience-启用单一登录的订阅
  
后学完本教程，您已分配给 Jobscience Azure AD 用户将能够到 Jobscience 公司网站 （服务提供程序初始化登录），或使用[简介访问权限面板](active-directory-saas-access-panel-introduction.md)应用程序的单一登录。
  
在本教程中介绍的方案由以下构造块组成︰

1.  启用应用程序集成为 Jobscience
2.  配置单一登录
3.  配置用户设置
4.  分配用户

![方案](./media/active-directory-saas-jobscience-tutorial/IC784341.png "方案")
##<a name="enabling-the-application-integration-for-jobscience"></a>启用应用程序集成为 Jobscience
  
本部分的目的是概述如何启用应用程序集成为 Jobscience。

###<a name="to-enable-the-application-integration-for-jobscience-perform-the-following-steps"></a>若要启用应用程序集成为 Jobscience，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-jobscience-tutorial/IC700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-jobscience-tutorial/IC700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-jobscience-tutorial/IC749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-jobscience-tutorial/IC749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**jobscience**。

    ![应用程序库](./media/active-directory-saas-jobscience-tutorial/IC784342.png "应用程序库")

7.  在结果窗格中，选择**Jobscience**，，然后单击**完成**添加应用程序。

    ![Jobscience](./media/active-directory-saas-jobscience-tutorial/IC784357.png "Jobscience")
##<a name="configuring-single-sign-on"></a>配置单一登录
  
本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 Jobscience。  
Jobscience 为单一登录配置需要检索证书的指纹值。  
如果您不熟悉此过程，请参阅[如何检索证书的指纹值](http://youtu.be/YKQF266SAxI)。

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤︰

1.  以管理员身份登录到您的 Jobscience 公司网站。

2.  转到**安装程序**。

    ![安装程序](./media/active-directory-saas-jobscience-tutorial/IC784358.png "安装程序")

3.  在左侧的导航窗格中，在**管理**部分中，单击**域管理**，以便展开相关的部分，，然后单击**我的域**打开**我的域**页。 

    ![我的域](./media/active-directory-saas-jobscience-tutorial/IC767825.png "我的域")

4.  要确认您的域已设置正确，请确保它是在"**步骤 4 将部署到用户**"并查看您的"**我的域设置**"。

    ![部署到用户的域](./media/active-directory-saas-jobscience-tutorial/IC784377.png "部署到用户的域")

5.  在不同的 web 浏览器窗口中，登录到 Azure 传统门户网站。

6.  在**Jobscience**应用程序集成页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-jobscience-tutorial/IC784360.png "配置单一登录")

7.  在**您想怎样来登录到 Jobscience 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-jobscience-tutorial/IC784361.png "配置单一登录")

8.  在**配置应用程序 URL**页上，在**Jobscience 号中 URL**文本框中，键入您的 URL 使用以下模式"*http://company.my.salesforce.com*"，然后单击**下一步**。

    ![配置应用程序的 URL](./media/active-directory-saas-jobscience-tutorial/IC784362.png "配置应用程序的 URL")

9.  在**配置单一登录 Jobscience 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机的本地证书文件。

    ![配置单一登录](./media/active-directory-saas-jobscience-tutorial/IC784363.png "配置单一登录")

10. Jobscience 公司在网站上，**安全控件**，请单击，然后单击**单一登录的设置**。

    ![安全控制](./media/active-directory-saas-jobscience-tutorial/IC784364.png "安全控制")

11. 在**单一登录设置**部分中，执行以下步骤︰

    ![单一登录设置](./media/active-directory-saas-jobscience-tutorial/IC781026.png "单一登录设置")

    1.  选择**启用 SAML**。
    2.  单击**新建**。

12. 在**SAML 单一登录设置编辑**对话框中，请执行以下步骤︰

    ![SAML 单一登录设置](./media/active-directory-saas-jobscience-tutorial/IC784365.png "SAML 单一登录设置")

    1.  在**名称**文本框中，键入您的配置的名称。
    2.  在 Azure 经典门户中，在**配置单一登录在 Jobscience**的对话页上，将**颁发者 URL**值，复制，然后将其粘贴到**颁发者**文本框
    3.  在**实体 Id**文本框中，键入**https://salesforce-jobscience.com**
    4.  单击**浏览**上载 Azure AD 证书。
    5.  作为**SAML 标识类型**，选择**断言包含从用户对象的联合身份验证 ID**。
    6.  作为**SAML 标识位置**，选择**标识是主题语句的 NameIdentfier 元素中**。
    7.  在 Azure 经典门户中，在**配置单一登录在 Jobscience**的对话页上，将复制的**远程登录 URL**值，然后将其粘贴到**标识提供程序的登录 URL**文本框
    8.  在 Azure 经典门户中，在**配置单一登录在 Jobscience**的对话页上，将**远程注销 URL**值，复制，然后将其粘贴到**标识提供程序注销 URL**文本框
    9.  单击**保存**。

13. 在左侧的导航窗格中，在**管理**部分中，单击**域管理**，以便展开相关的部分，，然后单击**我的域**打开**我的域**页。 

    ![我的域](./media/active-directory-saas-jobscience-tutorial/IC767825.png "我的域")

14. 在**我的域**页面上，在**登录页署名**部分中，单击**编辑**。

    ![品牌的登录页](./media/active-directory-saas-jobscience-tutorial/IC767826.png "品牌的登录页")

15. 在**登录页署名**页上，在**身份验证服务**部分中，将显示**SAML SSO 设置**的名称。 选择它，并单击**保存**。

    ![品牌的登录页](./media/active-directory-saas-jobscience-tutorial/IC784366.png "品牌的登录页")

16. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-jobscience-tutorial/IC784367.png "配置单一登录")
  
要获取 SP 启动单一登录的登录 URL，请单击明**安全控制**菜单中的**单一登录的设置**。

![安全控制](./media/active-directory-saas-jobscience-tutorial/IC784368.png "安全控制")
  
单击上一步中创建的 SSO 配置文件。  
此页显示单一登录 url 为您的公司 (如*https://companyname.my.salesforce.com?so=companyid*)。
##<a name="configuring-user-provisioning"></a>配置用户设置
  
为了使 Azure AD 用户能够登录到 Jobscience，他们必须到 Jobscience 配置。  
对于 Jobscience，资源调配是手动任务。

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤︰

1.  以管理员身份登录到您的**Jobscience**公司网站。

2.  转到安装程序

    ![安装程序](./media/active-directory-saas-jobscience-tutorial/IC784358.png "安装程序")

3.  转到**管理用户\>用户**。

    ![用户](./media/active-directory-saas-jobscience-tutorial/IC784369.png "用户")

4.  单击**新用户**。

    ![所有用户](./media/active-directory-saas-jobscience-tutorial/IC784370.png "所有用户")

5.  在**编辑用户**对话框中，请执行以下步骤︰

    ![用户编辑](./media/active-directory-saas-jobscience-tutorial/IC784371.png "用户编辑")

    1.  键入名字、 姓氏、 别名、 电子邮件、 要到相关文本框设置 Azure AD 用户的用户名称和别名属性。
    2.  单击**保存**。

    >[AZURE.NOTE] Azure 的广告帐户持有人将获得包括确认该帐户之前它被激活的链接的电子邮件。

>[AZURE.NOTE] 您可以使用任何其他 Jobscience 用户帐户创建工具或 Api 来设置 AAD 用户帐户由 Jobscience 提供。

##<a name="assigning-users"></a>分配用户
  
若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

###<a name="to-assign-users-to-jobscience-perform-the-following-steps"></a>要将用户分配到 Jobscience 中，执行以下步骤︰

1.  在 Azure 的传统门户网站，将创建一个测试帐户。

2.  在**Jobscience**应用程序集成页上，单击**将用户分配**。

    ![分配用户](./media/active-directory-saas-jobscience-tutorial/IC784372.png "分配用户")

3.  选择测试用户，单击**分配**，然后单击**是**以确认您的分配。

    ![是](./media/active-directory-saas-jobscience-tutorial/IC767830.png "是")
  
如果您想要测试单个登录设置，打开后盖。 关于访问面板的详细信息，请参阅[访问权限面板简介](active-directory-saas-access-panel-introduction.md)。