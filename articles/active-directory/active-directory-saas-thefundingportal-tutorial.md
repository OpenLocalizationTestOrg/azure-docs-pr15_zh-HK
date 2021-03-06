<properties
    pageTitle="教程︰ Azure Active Directory 集成与融资门户 |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和融资门户之间。"
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-the-funding-portal"></a>教程︰ 与融资门户的 Azure Active Directory 集成

在本教程中，您将学习如何使用 Azure 活动目录 (AD Azure) 集成投资门户。

与 Azure AD 集成投资门户提供了以下好处︰

- 您可以控制有权访问融资门户 Azure AD 中
- 您可以其 Azure 的广告帐户使用户可以自动获取签名上到融资门户 （单一登录）
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置融资门户 Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了**融资门户**单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
在本教程中，您将测试 Azure AD 单一登录在测试环境中。 在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加融资门户
2. 配置和测试 Azure AD 单一登录


## <a name="adding-the-funding-portal-from-the-gallery"></a>从库中添加融资门户
若要配置融资门户集成到 Azure 的广告，您需要将融资门户从库添加到托管的 SaaS 应用程序的列表。

**若要从库中添加融资门户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**融资门户**。

    ![Azure 广告测试用户](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_01.png)

7. 在结果窗格中，选择**融资门户**，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本节中，您配置和融资门户网站上一个名为"Britta Simon"的测试用户基于 Azure AD 单一登录测试。

对于单一登录工作，Azure 的广告需要知道融资门户中的对应的用户到用户在 Azure 的广告。 换句话说，Azure AD 用户和相关融资门户用户之间的链接关系需要建立。
此链接关系的建立在 Azure AD 作为融资门户中的**用户名**的值中指定的**用户名**的值。

要配置和测试 Azure AD 单一登录方式进行融资的门户网站，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建融资门户测试用户](#creating-a-the-funding-portal-test-user)**的融资在门户网站中，链接到她的 Azure 广告表示具有 Britta Simon 的副本。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD 单一登录配置

本节的目的是 Azure AD 单一登录在 Azure 的传统门户启用并配置单一登录融资门户应用程序中。

融资的门户应用程序期望的 SAML 断言包含名为"externalId1"的特性。 "ExternalId1"的值应为公认的 studentID。 请配置该应用程序的"externalId1"声明。 您可以从应用程序的**"Atrributes"**选项卡上管理这些属性的值。 下面的屏幕快照显示此示例。

![配置单一登录](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_03.png) 

**要融资门户 Azure AD 单一登录配置，请执行以下步骤︰**

1. 在 Azure 经典门户中，**融资门户**应用程序集成在页上，在菜单上，单击**属性**。
     
    ![配置单一登录][5]

2. 在 SAML 令牌的属性对话框中，添加"externalId1"属性。

    一。 单击**添加用户属性**以打开**添加用户属性**对话框。 
    
    ![配置单一登录](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_05.png)

    b。 在**属性名称**文本框中，键入属性名称"externalId1"。

    c。 从**属性值**列表中，选择要用于您的实现的特性。 例如，如果您已经在 ExtensionAttribute1 中存储的 StudentID 值，然后选择 user.extensionattribute1。

    d。 单击**完成**。 然后，单击**应用更改**。

1. 在顶部菜单中，单击**快速启动**。

    ![配置单一登录][6]

2. 在经典的门户，在**融资门户**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录][7] 

3. 在**您想怎样来登录到融资门户的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。
    
    ![配置单一登录](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_06.png)

4. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰ 

    ![配置单一登录](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_07.png)


    一。 在符号在 URL 文本框中，键入 URL 使用以下模式︰ `https://<subdomain>.regenteducation.net/`。

    b。 单击**下一步**。

5. 在**配置单一登录在融资门户网站**页上，单击**下载元数据**，然后将保存在您的计算机上的文件。

    ![配置单一登录](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_08.png)

6. 若要获得 SSO 配置您的应用程序，请联系投资门户支持。 他们将正确的渠道帮助配置 SSO。 请注意，您必须发送电子邮件并将其附加下载元数据文件为info@regenteducation.com。

7. 在经典的门户中，选择一个登录配置进行确认，，然后单击**下一步**。
    
    ![Azure 的广告单登录][10]

8. 在**单一登录确认**页上，单击**完成**。  
    
    ![Azure 的广告单登录][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
在本节中，您可以创建称为 Britta Simon 传统门户的测试用户。

![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。
    
    ![Azure 广告测试用户](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰
 
    ![Azure 广告测试用户](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-a-the-funding-portal-test-user"></a>创建融资门户测试用户

在本节中，您可以创建称为 Britta Simon 融资门户中的用户。 如果您不知道如何在融资门户中添加 Britta Simon，请与团队合作，融资门户支持添加测试用户和启用 SSO。 联系他们在info@regenteducation.com。

### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

在本节中，您可以启用 Britta Simon 她访问权限授予融资门户使用 Azure 单一登录。

![分配用户][200] 

**若要分配 Britta Simon 融资门户，请执行以下步骤︰**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**融资门户**。

    ![配置单一登录](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_09.png) 

1. 在菜单上，单击**用户**。

    ![分配用户][203] 

1. 在所有用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。

    ![分配用户][205]


### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。

当您单击访问权限面板中的融资门户拼贴时，您应该获得自动签名对融资门户应用程序。

## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_205.png
