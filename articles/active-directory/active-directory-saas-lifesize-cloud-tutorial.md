<properties
    pageTitle="教程︰ Azure Active Directory 集成 Lifesize 云与 |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 Lifesize 云之间。"
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
    ms.date="10/04/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>教程︰ 与 Lifesize 云的 Azure Active Directory 集成

在本教程中，您将学习如何使用 Azure 活动目录 (AD Azure) 集成 Lifesize 云。

与 Azure AD 集成 Lifesize 云提供了以下好处︰

- 可以在 Azure AD 拥有 Lifesize 云访问控制
- 您可以其 Azure 的广告帐户使用户可以自动获取签名对 Lifesize 云 （单一登录）
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置 Lifesize 云 Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了 Lifesize 云单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
在本教程中，您将测试 Azure AD 单一登录在测试环境中。

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 Lifesize 云
2. 配置和测试 Azure AD 单一登录


## <a name="adding-lifesize-cloud-from-the-gallery"></a>从库中添加 Lifesize 云
若要配置 Lifesize 云集成到 Azure 广告，您需要将 Lifesize 云从库添加到托管的 SaaS 应用程序的列表。

**要从库添加 Lifesize 云，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![活动目录][1]
2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**Lifesize 云**。

    ![Azure 广告测试用户](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_01.png)

7. 在结果窗格中，选择**Lifesize 云**，，然后单击**完成**添加应用程序。

    ![Azure 广告测试用户](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本节中，您将配置和 Lifesize 云 Azure AD 单一登录测试基于一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 的广告需要知道 Lifesize 群中对应的用户到用户在 Azure 的广告。 换句话说，Azure AD 用户和相关的 Lifesize 群中用户之间的链接关系需要建立。

此链接关系的建立在 Azure AD Lifesize 群中的**用户名**的值指定的**用户名**的值。

要配置和 Azure AD 单一登录 Lifesize 云与测试，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
3. **[创建 Lifesize 云测试用户](#creating-a-lifesize-cloud-test-user)**的链接到她的 Azure 广告表示 Lifesize 群中具有 Britta Simon 的副本。
4. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD 单一登录配置

在本节中，您启用 Azure AD 单一登录的传统门户和 Lifesize 云应用程序中配置单一登录。


**要 Lifesize 云 Azure AD 单一登录配置，请执行以下步骤︰**

1. 在经典的门户，在**Lifesize 云**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。
     
    ![配置单一登录][6] 

2. 在**您想怎样登录 Lifesize 云的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_03.png) 

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_04.png) 

    一。 在**符号上 URL**文本框中，键入您登录到 Lifesize 云应用程序使用以下模式用户使用的 URL: **https://login.lifesizecloud.com/ls/?acs**。
    
    b。 单击**下一步**
 
4. 在**配置单一登录 Lifesize 云在**页上，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_05.png)

    一。 单击**下载证书**，然后将保存在您的计算机上的文件。

    b。 单击**下一步**。


5. 获取为您的应用程序，登录到具有管理员权限 Lifesize 云应用程序配置 SSO。

6. 在右上角中单击您的名称，然后单击**高级设置**上

    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

7. 在高级设置现在**SSO 配置**链接上单击。 这将打开您的实例的 SSO 配置页。

    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

8. 现在配置 SSO 配置 UI 中的以下值。    

    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    • 颁发者 URL 的值从 Azure AD 中拷贝和粘贴该**标识提供商颁发者**文本框。

    • 远程登录 URL 的值从 Azure AD 中拷贝和粘贴的**登录 URL**文本框。

    • 在记事本中打开下载的证书和证书，不包括开始证书和最终证书行的内容复制、 粘贴的**X.509 证书**文本框中。

    • 在 SAML 属性映射中的**名字**文本框中输入的值为**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**

    • 在 SAML 属性映射中的**姓氏**文本框中输入的值为**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**

    • SAML 属性映射**电子邮件**文本框中的输入的值为**http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

9. 检查配置您可以单击**测试**按钮。

    > [AZURE.NOTE] 为测试的成功需要在 Azure 广告完成配置向导还提供给用户或组可以执行测试的访问。
    
10. 通过选中**启用 SSO**按钮启用 SSO。

11. 现在单击**更新**按钮，以便将保存所有的设置。 这将生成的 RelayState 值。 复制 RelayState 值的文本框中生成。 我们需要在接下来的步骤中此值。

12. 在经典的门户中，选择一个登录配置进行确认，，然后单击**下一步**。
    
    ![Azure 的广告单登录][10]

13. 在**单一登录确认**页上，单击**完成**。  
 
    ![Azure 的广告单登录][11]

14. 现在到 Azure 管理门户**https://portal.azure.com**使用管理凭据登录

15. 单击左侧的导航窗格中的**更多服务**链接
    
    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_09.png)

16. Azure 的 Active Directory 和**Azure Active Directory**链接上的单击搜索
    
    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_10.png)

17. 您会发现在**企业应用程序**按钮下的所有 SaaS 应用程序。

    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_11.png)

18. 现在，单击下一步的刀片式服务器中的**所有应用程序**链接
    
    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_12.png)

19. 搜索您要为其设置 RelayState Lifesize 应用程序。 
    
    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_13.png)

20. 现在，单击刀片式服务器中的**单一登录**链接

    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_14.png)

21. 您将看到**显示高级 URL 设置**复选框。 单击复选框。
    
    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_15.png)
    
22. 现在，配置应用程序，您看到 Lifesize 应用 SSO 配置页中的 RelayState。 

    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_16.png)

23. 保存设置。

### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
在本节中，您可以创建称为 Britta Simon 传统门户的测试用户。


![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰  ![Azure 广告测试用户](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰ ![Azure 广告测试用户](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   



### <a name="creating-an-lifesize-cloud-test-user"></a>创建 Lifesize 云测试用户

在本节中，您创建一个名 Britta Simon Lifesize 云中的用户。 Lifesize 云自动用户提供支持。 后在 Azure AD 身份验证成功，用户将自动设置应用程序中。 


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

在本节中，您可以启用 Britta Simon Lifesize 云授予她访问使用 Azure 单一登录。

![分配用户][200] 

**要赋予 Lifesize 云 Britta Simon，请执行以下步骤︰**

1. 在传统的门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**Lifesize 云**。

    ![配置单一登录](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_50.png) 

3. 在菜单上，单击**用户**。

    ![分配用户][203]

4. 在用户列表中，选择**Britta Simon**。

5. 在底部工具栏中，单击**指派**。

    ![分配用户][205]


### <a name="testing-single-sign-on"></a>单一登录测试

在本节中，您将测试 Azure AD 单一登录配置使用访问权限面板。

Lifesize 云拼贴访问权限面板中的单击时，您应该获取自动签名对 Lifesize 云应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_205.png
