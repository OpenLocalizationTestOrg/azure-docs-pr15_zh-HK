<properties
    pageTitle="教程︰ Azure Active Directory 集成与 IBM Kenexa 调查企业 |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 IBM Kenexa 调查企业之间。"
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>教程︰ 使用 IBM Kenexa 调查企业的 Azure Active Directory 集成

在本教程中，您将学习如何使用 Azure 活动目录 (AD Azure) 集成 IBM Kenexa 调查企业。

与 Azure AD 集成 IBM Kenexa 调查企业提供以下好处︰

- 您可以控制有权访问 IBM Kenexa 调查企业 Azure AD 中
- 您可以启用用户自动获得签名的 （单一登录） 的 IBM Kenexa 调查企业到其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置 IBM Kenexa 调查企业 Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了 IBM Kenexa 调查企业单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
在本教程中，您将测试 Azure AD 单一登录在测试环境中。 在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 IBM Kenexa 调查企业
2. 配置和测试 Azure AD 单一登录


## <a name="adding-ibm-kenexa-survey-enterprise-from-the-gallery"></a>从库中添加 IBM Kenexa 调查企业
若要配置 IBM Kenexa 调查企业集成到 Azure 广告，您需要将 IBM Kenexa 调查企业从库添加到托管的 SaaS 应用程序的列表。

**要从库添加 IBM Kenexa 调查企业，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**IBM Kenexa 调查企业**。

    ![Azure 广告测试用户](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_01.png)

7. 在结果窗格中，选择**IBM Kenexa 调查企业**中，，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本节中，您配置和 Azure AD 单一登录测试与 IBM Kenexa 调查企业基于一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 广告需要知道 IBM Kenexa 调查企业中对应的用户到用户在 Azure 的广告。 换句话说，Azure AD 用户和相关的 IBM Kenexa 调查企业中用户之间的链接关系需要建立。

此链接关系的建立在 Azure 广告作为 IBM Kenexa 调查企业**的用户名**的值指定的**用户名**的值。

要配置和 Azure AD 单一登录与 IBM Kenexa 调查企业测试，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建 IBM Kenexa 调查企业测试用户](#creating-an-kenexasurvey-test-user)**的链接到她的 Azure 广告表示 IBM Kenexa 调查企业中具有 Britta Simon 的副本。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
6. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD 单一登录配置

在本节中，您启用 Azure AD 单一登录的传统门户和 IBM Kenexa 调查企业应用程序中配置单一登录。


**要使用 IBM Kenexa 调查企业配置 Azure AD 单一登录，请执行以下步骤︰**

1. 在经典的门户，在**IBM Kenexa 调查企业**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录][6] 

2. 在**您想怎样来登录到 IBM Kenexa 调查企业的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。
 
    ![配置单一登录](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_03.png)

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_04.png)

    一。 在**标识符**文本框中，键入 URL 使用以下模式︰`https://surveys.kenexa.com/<company code>`

    b。 在**回复 URL**文本框中，键入 URL 使用以下模式︰`https://surveys.kenexa.com/<company code>/tools/sso.asp`

    c。 单击**下一步**。

    > [AZURE.NOTE] 请注意，这些不是实际值。 您必须使用实际的标识符和回复 URL 更新这些值。 请联系 IBM Kenexa 调查企业支持团队以获取这些值。

4. 在**配置单一登录 IBM Kenexa 调查企业在**页上，单击**下载证书**，然后将保存在您的计算机上的文件︰

    ![配置单一登录](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_05.png) 

5. 若要配置为您的应用程序的 SSO，与 IBM Kenexa 支持团队联系并提供以下︰

    • 下载的证书文件

    •**颁发者 URL**

    • **SAML SSO URL**

    •**一个注销服务 URL**

    > [AZURE.NOTE] 请注意 NameID 声明响应中的值没有以 Kenexa 系统中配置 SSO id 匹配。 所以请工作与 Kenexa 支持团队能够映射 SSO id 您组织中的相应用户标识符 默认情况下 Azure 广告将作为 UPN 值设置 NameIdentifier。 可以从下面的屏幕快照中所示的属性选项卡中更改此数据。 在完成正确的映射后仅适用集成。 
    
    ![配置单一登录](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_51.png)

6. 在经典的门户中，选择一个登录配置进行确认，，然后单击**下一步**。

    ![Azure 的广告单登录][10]

7. 在**单一登录确认**页上，单击**完成**。  
  
    ![Azure 的广告单登录][11]

8. 在 Azure 经典门户中， **IBM Kenexa 调查企业**应用程序集成在页上，在菜单上，单击**属性**。

    ![配置单一登录](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_06.png)

9. 在**SAML 令牌的属性**对话框中，请执行以下步骤︰

    一。 选择**NameIdentifier**的属性并单击**编辑**图标。

    ![配置单一登录](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_07.png)
    
    b。 从**属性值**列表中，键入在 Kenexa 系统中配置 SSO ID 的属性值。
    
    c。 单击**完成**

### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon 的经典门户创建的测试用户。

![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。
 
    ![Azure 广告测试用户](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰
 
    ![Azure 广告测试用户](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-an-ibm-kenexa-survey-enterprise-test-user"></a>IBM Kenexa 调查企业测试用户

在本节中，您创建一个名 Britta Simon IBM Kenexa 调查企业中的用户。 请使用 IBM Kenexa 的支持小组来为所有用户 SSO ID 映射。 也该 SSO ID 值应该从 Azure AD 映射到 NameIdentifier 值中。 您可以更改此默认设置，属性选项卡中。


> [AZURE.NOTE] 如果您需要手动创建用户，您需要与 IBM Kenexa 调查企业支持团队联系。


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

在此部分中，您可以启用 Britta Simon 通过授予她访问 IBM Kenexa 调查企业使用 Azure 单一登录。

![分配用户][200] 

**要为 IBM Kenexa 调查企业 Britta Simon，请执行以下步骤︰**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**IBM Kenexa 调查企业**。

    ![配置单一登录](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_50.png) 

1. 在菜单上，单击**用户**。

    ![分配用户][203] 

1. 在用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。
    
    ![分配用户][205]



### <a name="testing-single-sign-on"></a>单一登录测试

在本节中，您将测试 Azure AD 单一登录配置使用访问权限面板。

当您单击访问权限面板中的 IBM Kenexa 调查企业拼贴时，您应该获取自动签名对 IBM Kenexa 调查企业应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_205.png
