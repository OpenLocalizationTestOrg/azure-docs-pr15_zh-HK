<properties
    pageTitle="教程︰ Azure Active Directory 集成与 HackerOne |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 HackerOne 之间。"
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a>教程︰ 与 HackerOne 的 Azure Active Directory 集成

在本教程中，您将 HackerOne 集成使用 Azure 活动目录 (AD Azure)。

与 Azure AD 集成 HackerOne 提供了以下好处︰

- 您可以控制有权访问 HackerOne Azure AD 中
- 您可以使用户可以自动获取签名的 HackerOne （单一登录） 到其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件

要配置 HackerOne Azure 广告集成，您需要以下各项︰

- Azure 的订阅
- 在已启用的订阅了 HackerOne 单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="scenario-description"></a>方案说明
在本教程中，您配置和 Azure AD 单一登录测试在测试环境中。  
在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加 HackerOne
2. 配置和测试 Azure AD 单一登录


## <a name="adding-hackerone-from-the-gallery"></a>从库中添加 HackerOne
要将 HackerOne 集成到 Azure 的广告，您需要将 HackerOne 从库添加到托管的 SaaS 应用程序的列表。

**要从库添加 HackerOne，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

![应用程序][4]

6. 在搜索框中，键入**HackerOne**。

![Azure 广告测试用户](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_01.png)

7. 在结果窗格中，选择**HackerOne**，，然后单击**完成**添加应用程序。


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
接下来，配置和测试 Azure AD 单一登录 HackerOne 基于一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 广告需要知道 HackerOne 在 Azure AD 用户对应的用户。 换句话说，Azure AD 用户和 HackerOne 中的相关的用户之间的链接关系需要建立。  
此链接关系的建立在 Azure 广告作为**用户名**HackerOne 中的值指定的**用户名**的值。

要配置和测试 Azure AD 单一登录使用 HackerOne，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
3. **[创建 HackerOne 测试用户](#creating-a-hackerone-test-user)**的认证，链接到她的 Azure 广告表示中具有 Britta Simon 的副本。
4. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure 的广告单登录配置

接下来，您启用 Azure AD single sign-on 等传统门户中，并配置单一登录，HackerOne 应用程序中。

作为此过程的一部分，您将需要创建一个 base 64 编码的证书文件。  
如果您不熟悉此过程，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

**以 HackerOne Azure AD 单一登录配置，请执行以下步骤︰**

1. 在 Azure 传统门户网站，在**HackerOne**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录][6] 

2. 在**您想怎样来登录到 HackerOne 的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![配置单一登录](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_03.png) 

3. 在**配置应用程序设置**对话框页上，执行以下步骤，然后单击**下一步**:

    ![配置单一登录](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_04.png) 


    一。 在**符号上 URL**文本框中，键入您登录到 HackerOne 应用程序使用以下模式用户使用的 URL: **"https://hackerone.com/\<公司名称\>/authentication"**。 

    b。 联系 HackerOne 的支持团队通过[support@hackerone.com](mailto:support@hackerone.com)以获取租户 URL，如果您不知道它。

    c。 在**标识符**文本框中，键入租户 URL。 

    d。 单击**下一步**。


4. 在**配置单一登录 HackerOne 在**页上，执行以下步骤，然后单击**下一步**:

    ![配置单一登录](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_05.png) 

    一。 单击**下载证书**，然后将保存在您的计算机上的文件。

    b。 单击**下一步**。


1. 登录到管理员为您 HackerOne 租户。

1. 在菜单上，单击**设置**。

    ![配置单一登录](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

1. 导航到"**身份验证**"，然后单击"**添加 SAML 设置**"。

    ![配置单一登录](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 


1. 在**SAML 设置**对话框中，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    一。 在**电子邮件域**文本框中，键入注册的域。

    b。 在 Azure 的传统门户网站，复制的**单个登录服务 URL**，，然后将其粘贴到单个符号在 URL 文本框。

    c。 创建您已下载的证书的**base 64 编码**的文件。  

       >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)
    
    d。 在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后粘贴到**X509 证书**文本框。

    电子。 单击**保存**


1. 在身份验证设置对话框中，请执行以下步骤︰

    ![配置单一登录](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    一。 单击**运行测试**。

    b。 如果**状态**的值字段等于**上次测试状态︰ 创建**，请联系 HackerOne 的支持团队通过[support@hackerone.com](mailto:support@hackerone.com)请求查看您的配置。


6. 在 Azure 经典门户中，选择一个登录配置进行确认，，然后单击**下一步**。

    ![Azure 的广告单登录][10]

7. 在**单一登录确认**页上，单击**完成**。  
 
    ![Azure 的广告单登录][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户

接下来，调用 Britta Simon 经典门户中创建一个测试用户。  

![创建 Azure AD 用户][20]

**若要创建在 Azure AD 安全提供测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-hackerone-tutorial/create_aaduser_09.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。
    ![Azure 广告测试用户](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-hackerone-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-hackerone-tutorial/create_aaduser_06.png) 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-hackerone-tutorial/create_aaduser_07.png) 

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-hackerone-tutorial/create_aaduser_08.png) 

    一。 记下**新密码**的值。

    b。 单击**完成**。   


### <a name="creating-a-hackerone-test-user"></a>HackerOne 测试用户

接下来，创建一个名 Britta Simon HackerOne 中的用户。 HackerOne 支持中及时的资源调配，它默认处于启用状态。

没有为您在本部分中的操作项。 当您访问 HackerOne 时，如果它尚不存在则创建新用户。 [Azure 的广告单登录配置](#configuring-azure-ad-single-single-sign-on)。

> [AZURE.NOTE] 如果您需要手动创建用户，您需要与认证支持团队联系。




### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

接下来，您将启用 Britta Simon HackerOne 授予她访问使用 Azure 单一登录。

![分配用户][200] 

**若要分配 Britta Simon HackerOne，请执行以下步骤︰**

1. 在 Azure 的传统门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201] 

2. 在应用程序列表中，选择**HackerOne**。

    ![配置单一登录](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_50.png) 

1. 在菜单上，单击**用户**。

    ![分配用户][203] 

1. 在用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。

    ![分配用户][205]



### <a name="testing-single-sign-on"></a>单一登录测试

最后，您测试您 Azure AD 单一登录配置使用访问权限面板。  
单击访问权限面板中的 HackerOne 平铺时，您应该获取自动签名对 HackerOne 应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_205.png