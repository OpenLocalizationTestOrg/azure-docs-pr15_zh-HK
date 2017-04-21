<properties 
    pageTitle="教程︰ Azure Active Directory 集成与 ClickTime |Microsoft Azure" 
    description="了解如何使用 ClickTime Azure Active Directory 以启用单一登录、 自动化资源调配，以及更多 ！" 
    services="active-directory" 
    authors="jeevansd"
    documentationCenter="na" 
    manager="femila" />
<tags
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-clicktime"></a>教程︰ 与 ClickTime 的 Azure Active Directory 集成

在本教程中，您将学习如何使用 Azure 活动目录 (AD Azure) 集成 ClickTime。

与 Azure AD 集成 ClickTime 提供了以下好处︰

- 您可以控制有权访问 ClickTime Azure AD 中
- 您可以使用户可以自动获取签名的 ClickTime （单一登录） 到其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置 ClickTime Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了 ClickTime 单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
在本教程中，您将测试 Azure AD 单一登录在测试环境中。

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 ClickTime
2. 配置和测试 Azure AD 单一登录

##<a name="adding-clicktime-from-the-gallery"></a>从库中添加 ClickTime

本部分的目的是概述如何启用应用程序集成为 ClickTime。

###<a name="to-enable-the-application-integration-for-clicktime-perform-the-following-steps"></a>若要启用应用程序集成为 ClickTime，请执行以下步骤︰

1.  在 Azure 中经典的门户，在左侧的导航窗格中，单击**目录活动**。

    ![活动目录](./media/active-directory-saas-clicktime-tutorial/tic700993.png "活动目录")

2.  从**目录**列表中，选择要为其启用目录集成的目录。

3.  若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序](./media/active-directory-saas-clicktime-tutorial/tic700994.png "应用程序")

4.  单击页面底部的**添加**。

    ![添加应用程序](./media/active-directory-saas-clicktime-tutorial/tic749321.png "添加应用程序")

5.  在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![添加应用程序从 gallerry](./media/active-directory-saas-clicktime-tutorial/tic749322.png "添加应用程序从 gallerry")

6.  在**搜索框**中，键入**ClickTime**。

    ![应用程序库](./media/active-directory-saas-clicktime-tutorial/tic777275.png "应用程序库")

7.  在结果窗格中，选择**ClickTime**，，然后单击**完成**添加应用程序。

    ![ClickTime](./media/active-directory-saas-clicktime-tutorial/tic777276.png "ClickTime")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，配置和测试 Azure AD 单一登录 ClickTime 基于一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 的广告需要知道对应的用户在 ClickTime 中是什么给用户在 Azure 的广告。 换句话说，Azure AD 用户和 ClickTime 中的相关的用户之间的链接关系需要建立。

此链接关系的建立在 Azure 广告作为**用户名**ClickTime 中的值指定的**用户名**的值。

要配置和测试 Azure AD 单一登录使用 ClickTime，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
3. **[创建 ClickTime 测试用户](#creating-a-clicktime-test-user)**的链接到她的 Azure 广告表示的 ClickTime 中具有 Britta Simon 的副本。
4. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。


### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD 单一登录配置

本部分的目的是概述如何使用户能够使用基于 SAML 协议联合 Azure AD 中使用他们的帐户验证到 ClickTime。  


>[AZURE.IMPORTANT] 为了能够在 ClickTime 租户上配置单一登录，您需要首先联系 ClickTime 技术支持以获取启用此功能。

**以 ClickTime Azure AD 单一登录配置，请执行以下步骤︰**

1.  在 Azure 传统门户网站，在**ClickTime**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![启用单一登录](./media/active-directory-saas-clicktime-tutorial/tic777277.png "启用单一登录")

2.  在**您想怎样来登录到 ClickTime 的用户**页上**Microsoft Azure AD 单一登录**，选择，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-clicktime-tutorial/tic777278.png "配置单一登录")

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-clicktime-tutorial/tic777286.png) 

    一。 在**IdentifierL**文本框中，键入 URL 使用以下模式︰ **https://app.clicktime.com/sp/**
    
    b。 在**回复 URL**文本框中，键入 URL 使用以下模式︰ **https://app.clicktime.com/Login/**

    c。 单击**下一步**

4.  在**配置单一登录 ClickTime 在**页上，下载您的证书，单击**下载证书**，然后将保存在您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-clicktime-tutorial/tic777279.png "配置单一登录")

4.  在不同的 web 浏览器窗口中，以管理员身份登录到 ClickTime 公司网站。

5.  在顶部的工具栏中，单击**首选项**，然后单击**安全设置**。

6.  在**单一登录首选项**配置部分中，执行以下步骤︰

    ![安全设置](./media/active-directory-saas-clicktime-tutorial/tic777280.png "安全设置")

    一。  选择**允许**使用**Azure AD**的单一登录 (SSO) 登录。
    
    b。  在 Azure 经典门户中，在**配置单一登录在 ClickTime**对话框页面上，将**单一登录服务 URL**值，复制，然后将其粘贴到文本框中**的身份提供方终结点**。

    c。  在**记事本**中打开 base 64 编码的证书，复制的内容，然后将其粘贴到文本框中**的 X.509 证书**。
    
    d。  单击**保存**。

7.  在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。

    ![配置单一登录](./media/active-directory-saas-clicktime-tutorial/tic777281.png "配置单一登录")

##<a name="configuring-user-provisioning"></a>配置用户设置

为了使 Azure AD 用户能够登录到 ClickTime，他们必须到 ClickTime 配置。  
对于 ClickTime，资源调配是手动任务。

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>若要设置用户帐户，请执行以下步骤︰

1.  登录到**ClickTime**租户。

2.  在顶部的工具栏中，单击**公司**，，然后单击**人员**。

    ![人员](./media/active-directory-saas-clicktime-tutorial/tic777282.png "人员")

3.  单击**添加用户**。

    ![添加人员](./media/active-directory-saas-clicktime-tutorial/tic777283.png "添加人员")

4.  在新人部分中，执行以下步骤︰

    ![人员](./media/active-directory-saas-clicktime-tutorial/tic777284.png "人员")

    一。  在**电子邮件地址**文本框中，键入您的 Azure 的广告帐户的电子邮件地址。
    
    b。  在**全名**文本框中，键入 Azure 的广告帐户的名称。  

    >[AZURE.NOTE] 如果您愿意，您可以设置新的 person 对象的其他属性。

    c。  单击**保存**。

>[AZURE.NOTE] 您可以使用任何其他 ClickTime 用户帐户创建工具或 Api 提供的 ClickTime 设置 Azure AD 用户帐户。

### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

在本节中，您可以启用 Britta Simon ClickTime 授予她访问使用 Azure 单一登录。

![分配用户][200]

若要测试您的配置，您需要将 Azure 的 AD 用户授予您要允许使用通过将他们分配到您应用程序的访问权限。

**若要分配 Britta Simon ClickTime，请执行以下步骤**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**ClickTime**。

    ![配置单一登录](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_50.png) 

3. 在菜单上，单击**用户**。

    ![分配用户][203]

4. 在用户列表中，选择**Britta Simon**。

5. 在底部工具栏中，单击**指派**。

    ![分配用户][205]

## <a name="testing-single-sign-on"></a>单一登录测试
在本节中，您将测试 Azure AD 单一登录配置使用访问权限面板。

单击访问权限面板中的 ClickTime 平铺时，您应该获取自动签名对 ClickTime 应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_205.png