<properties
    pageTitle="教程︰ Azure Active Directory 集成 Soonr 工作场所与 |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 Soonr 场所之间。"
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
    ms.date="10/14/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a>教程︰ 与 Soonr 工作场所的 Azure Active Directory 集成

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成 Soonr 工作场所。  
与 Azure AD 集成 Soonr 工作场所提供了以下好处︰

- 您可以在 Azure AD Soonr 工作具有访问权限控制
- 您可以其 Azure 的广告帐户使用户可以自动获取签名对 Soonr （单一登录） 的工作场所
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站


如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置 Soonr 场所 Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了 Soonr 工作单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。  
在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 Soonr 工作场所
2. 配置和测试 Azure AD 单一登录


## <a name="adding-soonr-workplace-from-the-gallery"></a>从库中添加 Soonr 工作场所
要配置集成 Soonr 工作场所到 Azure 的广告，您需要将 Soonr 工作场所从库添加到托管的 SaaS 应用程序的列表。

**要从库添加 Soonr 工作区，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。
 
    ![应用程序][4]

6. 在搜索框中，键入**Soonr 工作场所**。
 
    ![Azure 广告测试用户](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_01.png)

7. 在结果窗格中，选择**Soonr 工作场所**，，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本节的目的是要向您展示如何配置和 Azure AD 单一登录与 Soonr 工作场所测试基于一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 的广告需要知道在 Azure AD 用户 Soonr 工作场所中对应的用户。 换句话说，Azure AD 用户和 Soonr 工作区中相关的用户之间的链接关系需要建立。  
此链接关系的建立在 Azure 广告作为**用户名**Soonr 工作区中的值指定的**用户名**的值。


要配置和 Azure AD 单一登录与 Soonr 工作场所测试，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建 Soonr 工作场所测试用户](#creating-a-soonr-workplace-test-user)**-Soonr 工作区链接到她的 Azure 广告表示中具有 Britta Simon 的副本。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD 单一登录配置

本节的目的是 Azure AD 单一登录在 Azure 的传统门户网站中启用并 Soonr 工作区应用程序中配置单一登录。



**要 Soonr 场所 Azure AD 单一登录配置，请执行以下步骤︰**

1. 在 Azure 经典门户， **Soonr 工作区**应用程序集成在页上，单击**配置单一登录**来打开**配置单一登录**对话框。

    ![配置单一登录][6] 

2. **您想怎样用户能够登录到 Soonr 场所**在页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_03.png) 

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤:。

    ![配置单一登录](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_04.png) 


    一。 在**符号上 URL**文本框中，键入 URL 的用户，使用于登录到您的应用程序使用以下模式︰ `https://<server-name>.soonr.com/singlesignon/saml/SSO`。

    b。 单击**下一步**。

4. 在**配置单一登录 Soonr 工作场所**页中，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_05.png) 

    一。 单击**下载元数据**，然后保存该文件在您的计算机上。

    b。 **颁发者 URL**

    C。 **SAML SSO URL**

    D. **一个退出服务 URL**


5. Soonr 场所配置单一登录，请建立亲密关系更快地团队到<a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">这里</a>。
   
6. 在 Azure 经典门户中，选择一个登录配置进行确认，，然后单击**下一步**。

    ![Azure 的广告单登录][10]

7. 在**单一登录确认**页上，单击**完成**。  
  
    ![Azure 的广告单登录][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon Azure 经典门户创建的测试用户。  

![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-soonr-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-soonr-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-soonr-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-soonr-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-soonr-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-a-soonr-workplace-test-user"></a>创建 Soonr 工作场所测试用户

 这一节的目标是创建一个名 Britta Simon Soonr 工作区中的用户。 请使用 Soonr 支持团队在平台中创建用户。 您可以提高与 Soonr 从<a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">这里</a>支持票。


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon 她访问权限授予 Soonr 工作场所使用 Azure 单一登录。

![分配用户][200] 

**要 Britta Simon 为 Soonr 工作区，请执行以下步骤︰**

1. 在 Azure 的传统门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**Soonr 工作场所**。

    ![配置单一登录](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_50.png) 

1. 在菜单上，单击**用户**。

    ![分配用户][203] 

1. 在用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。

    ![分配用户][205]



### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。  
Soonr 场所拼贴访问权限面板中的单击时，您应该获取自动签名对 Soonr 办公应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_205.png
