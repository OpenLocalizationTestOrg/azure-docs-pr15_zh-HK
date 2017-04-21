<properties
    pageTitle="教程︰ Azure Active Directory 集成与 ADP eTime |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 ADP eTime 之间。"
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
    ms.date="10/17/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a>教程︰ Azure Active Directory 集成与 ADP eTime

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成 ADP eTime。  
与 Azure AD 集成 ADP eTime 提供了以下好处︰

- 您可以控制有权访问 ADP eTime Azure AD 中
- 您可以使用户可以自动获取签名上到其 Azure 的广告帐户的 ADP eTime （单一登录）
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站


如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

若要配置与 ADP eTime Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- ADP eTime 单点登录启用订阅


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。  
在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 ADP eTime
2. 配置和测试 Azure AD 单一登录


## <a name="adding-adp-etime-from-the-gallery"></a>从库中添加 ADP eTime
若要配置 ADP eTime 集成到 Azure 广告，您需要从库的 ADP eTime 添加到托管的 SaaS 应用程序的列表。

**要从库添加 ADP eTime，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**ADP eTime**。

    ![Azure 广告测试用户](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_01.png)

7. 在结果窗格中，选择**ADP eTime**，，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分的目的是向您展示如何配置和 Azure AD 单一登录测试与 ADP eTime 基于一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 广告需要知道 ADP eTime 在 Azure AD 用户对应的用户。 换句话说，Azure AD 用户和 ADP eTime 中的相关的用户之间的链接关系需要建立。  
此链接关系的建立在 Azure 广告作为**用户名**ADP eTime 中的值指定的**用户名**的值。

要配置和 Azure AD 单一登录与 ADP eTime 测试，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建 ADP eTime 测试用户](#creating-a-adpetime-test-user)**-具有在 ADP eTime 链接到她的 Azure 广告表示 Britta Simon 的副本。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure 的广告单登录配置

本节的目的是 Azure AD 单一登录在 Azure 的传统门户网站中启用并配置单一登录 ADP eTime 应用程序中。

ADP eTime 应用程序需要以特定的格式，这就需要你对 SAML 令牌的属性配置中添加自定义属性映射 SAML 断言。 下面的屏幕快照显示此示例。 声明名称将始终为**"PersonImmutableID"** ，则这种情况下，我们已映射到 ExtensionAttribute2，其中包含用户的雇员 Id。 这里将雇员 Id 完成用户映射 fron Azure 广告对 ADP eTime，但您可以映射到一个不同的值，也基于应用程序的设置。 所以请工作与 ADP eTime 团队首先要使用正确的用户标识符和映射的值与**"PersonImmutableID"**的索赔。  

![配置单一登录](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_02.png) 

您可以配置 SAML 断言之前，您需要联系 ADP eTime 的支持团队并请求为您的组织的唯一标识符的属性的值。 您需要此值可配置为应用程序自定义声明。


**以 ADP eTime Azure AD 单一登录配置，请执行以下步骤︰**

1. 在 Azure 传统门户网站，在**ADP eTime**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录][6] 

2. 在**您想怎样来登录到 ADP eTime 的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_03.png) 

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤:。

    ![配置单一登录](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_04.png) 


    一。 在**回复 URL**文本框中，键入您的用户用来登录到您使用以下模式的 ADP eTime 应用程序的 URL: `https://<server name>.adp.com/affwebservices/public/saml2assertionconsumer`。

    b。 单击**下一步**。

4. 在**配置单一登录在 ADP eTime**页面上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_05.png) 

    一。 单击**下载元数据**，然后保存该文件在您的计算机上。

    b。 单击**下一步**。


5. 若要配置为您的应用程序的 SSO，ADP eTime 的支持团队联系，电子邮件下载附加元数据文件中，以便可以将它们配置为 SSO 集成。

    > [AZURE.NOTE] **ADP eTime**团队配置该实例后，从中获得的**RelayState**值。请按照下述步骤对其进行配置。 此配置后，您可以测试集成。 请注意，这是重要的配置，此应用程序集成工作。

6. 若要配置在 Azure AD RelayState 值，请执行以下步骤︰ 
    
    一。 到[Azure 管理门户](https://portal.azure.com)以管理员身份登录。

    b。 在左侧的导航窗格中，单击**其他服务**。 
    
    ![配置单一登录](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_07.png)

    c。 在**搜索**文本框中，键入**Azure Active Directory**，然后单击相关的链接。
    
    ![配置单一登录](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_08.png)

    d。 单击**企业应用程序**。

    ![配置单一登录](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_09.png)

    电子。 在**管理**部分中，单击**所有应用程序**。
    
    ![配置单一登录](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_10.png)

    f。 在**搜索**文本框中，键入**ADP eTime**，，然后单击相关的链接。 
    
    ![配置单一登录](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_11.png)

    g。 在**管理**部分中，单击**单一登录**。

    ![配置单一登录](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_12.png)

    h。 选择**显示高级设置 URL**。
    
    ![配置单一登录](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_13.png)
    
    我。 在**中继状态**文本框中，键入一个值使用以下模式︰
    
    - 生产环境︰`https://fed.adp.com/saml/fedlanding.html?<id>` 
    - 暂存环境︰`https://fed-stag.adp.com/saml/fedlanding.html?PORTAL`

    ![配置单一登录](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_14.png)

    j。 保存设置。

7. 在 Azure 经典门户中，选择一个登录配置进行确认，，然后单击**下一步**。

    ![Azure 的广告单登录][10]

8. 在**单一登录确认**页上，单击**完成**。  

    ![Azure 的广告单登录][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon Azure 经典门户创建的测试用户。  
在用户列表中，选择**Britta Simon**。

![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-adpetime-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-adpetime-tutorial/create_aaduser_05.png) 

    一。 作为**用户的类型**，选择**您的组织中的新用户**。

    b。 在**用户名**文本框中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-adpetime-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-adpetime-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-adpetime-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-a-adp-etime-test-user"></a>ADP eTime 测试用户

本节的目的是创建一个在 ADP eTime 名 Britta Simon 的用户。 请使用 ADP eTime 的支持团队，在 ADP eTime 帐户中添加用户。 


> [AZURE.NOTE]如果您需要手动创建用户，您需要与 ADP eTime 的支持团队联系。


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon 通过授予 ADP eTime 她访问权限使用 Azure 单一登录。

![分配用户][200] 

**要为 ADP eTime Britta Simon，请执行以下步骤︰**

1. 在 Azure 的传统门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**ADP eTime**。

    ![配置单一登录](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_50.png) 

1. 在菜单上，单击**用户**。

    ![分配用户][203] 

1. 在用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。

    ![分配用户][205]



### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。  
当您单击访问权限面板中的 ADP eTime 拼贴时，您应该获取自动签名对 ADP eTime 应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_205.png
