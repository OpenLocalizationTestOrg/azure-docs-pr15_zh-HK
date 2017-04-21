<properties
    pageTitle="教程︰ 与台卤素软件的 Azure Active Directory 集成"
    description="了解如何配置单一登录 Azure Active Directory 和台卤素软件之间。"
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>教程︰ 与台卤素软件的 Azure Active Directory 集成

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成台卤素软件。

与 Azure AD 集成台卤素软件提供了以下好处︰ 

- 您可以控制有权访问台卤素软件 Azure AD 中 
- 您可以其 Azure 的广告帐户使用户可以自动获取签名上到台卤素软件 （单一登录）
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件 

要配置台卤素软件 Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在已启用的订阅了台卤素软件单点登录


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。 

 
## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。 

在本教程中介绍的方案由两个主要的构造块组成︰

1. 从库中添加台卤素软件 
2. 配置和测试 Azure AD 单一登录


## <a name="adding-halogen-software-from-the-gallery"></a>从库中添加台卤素软件
若要配置台卤素软件集成到 Azure 广告，您需要从库的台卤素软件添加到托管的 SaaS 应用程序的列表。

**要从库添加台卤素软件，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。 

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**台卤素软件**。

    ![应用程序][5]

7. 在结果窗格中，选择**台卤素软件**，然后单击**完成**添加应用程序。



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分的目的是向您展示如何配置和 Azure AD 单一登录测试与台卤素软件基于一个名为"Britta Simon"的测试用户。

对于单一登录工作，Azure 广告需要知道在 Azure AD 用户台卤素软件中对应的用户。 换句话说，Azure AD 用户和相关台卤素软件中的用户之间的链接关系需要建立。

此链接关系的建立在 Azure 广告作为台卤素软件中的**用户名**的值指定的**用户名**的值。
 
要配置和 Azure AD 单一登录与台卤素软件测试，您需要完成以下构造块︰

1. **[Azure AD 配置一个 Single Sign-on](#configuring-azure-ad-single-single-sign-on)** -若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建一台卤素软件测试用户](#creating-a-halogen-software-test-user)**的在台卤素软件链接到她的 Azure 广告表示具有 Britta Simon 的副本。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-single-sign-on"></a>Azure 的广告单单一登录配置

本节的目的是 Azure AD 单一登录在 Azure 的传统门户网站中启用并配置单一登录台卤素软件应用程序中。


**要配置 Azure AD 单一登录台卤素软件，请执行以下步骤︰**

1. 在 Azure 传统门户网站，在**台卤素软件**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录][8]

2. 在**您想怎样来登录到台卤素软件的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![Azure 的广告单登录][9]

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰ ![配置的应用程序设置][10]
 
     一。 在**符号上 URL**文本框中，键入您的 URL 用户用于登录到台卤素软件应用程序使用以下模式︰ *https://global.hgncloud.com/fabrikam/welcome.jsp*

     b。 单击**下一步**。
 
4. 在**配置单一登录在台卤素软件**页面上，单击**下载元数据**，并保存在您的计算机上的本地元数据文件。
    
    ![什么是 Azure AD 连接][11]

5. 在不同的浏览器窗口中，登录到您作为管理员的**台卤素软件**应用程序。

6. 单击**选项**选项卡。 

    ![什么是 Azure AD 连接][12]


7. 在左侧的导航窗格中，单击**SAML 配置**。 

    ![什么是 Azure AD 连接][13]

8. 在**SAML 配置**页上，请执行以下步骤︰  ![Azure AD 连接的是什么][14]

    一。 作为**唯一标识符**，请选择**NameID**。

    b。 作为**唯一的标识符映射到**，选择**用户名**。

    c。 若要上载下载元数据文件，请单击**浏览**来选择该文件，然后**将上载的文件**。

    d。 要测试配置，请单击**运行测试**。 

    > [AZURE.NOTE] 您需要等待消息"*SAML 测试已完成。请关闭此窗口*"。 然后，关闭已打开的浏览器窗口。 测试完成后才会启用**启用 SAML**复选框。

    电子。 选择**启用 SAML**。
    
    f。 单击**保存更改**。 


9. 在 Azure 的传统门户网站，选择单一登录配置进行确认，然后单击**完成**关闭**配置单一登录**对话框。 

    ![什么是 Azure AD 连接][15]

10. 在**单一登录确认**页上，单击**完成**。  

    ![什么是 Azure AD 连接][16]




### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon Azure 经典门户创建的测试用户。

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![什么是 Azure AD 连接][100] 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![什么是 Azure AD 连接][101] 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。 

    ![什么是 Azure AD 连接][102] 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰

    ![什么是 Azure AD 连接][103] 
 
    一。 作为**用户的类型**，选择**您的组织中的新用户**。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击下一步。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰ 

    ![什么是 Azure AD 连接][104] 

    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**最后一名**txtbox，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![什么是 Azure AD 连接][105]  

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![什么是 Azure AD 连接][106]   

    一。 记下**新密码**的值。
    b。 单击**完成**。   
  
 
### <a name="creating-a-halogen-software-test-user"></a>创建台卤素软件测试用户

本节的目的是创建一个名为 Britta Simon 台卤素软件中用户。

**若要创建一个名为 Britta Simon 在台卤素软件用户，请执行以下步骤︰**

1. 到**台卤素软件**应用程序以管理员身份登录。

2. 单击**用户中心**选项卡，然后单击**创建用户**。

    ![什么是 Azure AD 连接][300]  

3. 在**新建用户**对话框页，请执行以下步骤︰

    ![什么是 Azure AD 连接][301]

    一。 在**名字**文本框中，键入**Britta**。 
  
    b。 在**姓氏**文本框中，键入**Simon**。
  
    c。 在**用户名**文本框中，键入**Brita Simon Azure 的经典门户中的用户名称**。
  
    d。 在**密码**文本框中，键入 Britta 密码。
  
    电子。 单击**保存**。


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon 台卤素软件授予其访问权限使用 Azure 单一登录。

![什么是 Azure AD 连接][200]

**要 Britta Simon 为台卤素软件，请执行以下步骤︰**

1. 在 Azure 的传统门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![什么是 Azure AD 连接][201]

2. 在应用程序列表中，选择**台卤素软件**。

    ![什么是 Azure AD 连接][202]

1. 在菜单上，单击**用户**。

    ![什么是 Azure AD 连接][203]

1. 在用户列表中，选择**Britta Simon**。

    ![什么是 Azure AD 连接][204]

2. 在底部工具栏中，单击**指派**。

    ![什么是 Azure AD 连接][205]



### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。

当单击接入面板中的台卤素软件平铺时，您应该获取自动签名对台卤素软件应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_04.png
[5]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_05.png
[6]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_06.png
[7]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_07.png
[8]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_08.png
[9]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_09.png
[10]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_10.png
[11]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_11.png
[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png
[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png
[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png
[15]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_15.png
[16]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_16.png
[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_100.png 
[101]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_101.png 
[102]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_102.png 
[103]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_103.png 
[104]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_104.png 
[105]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_105.png 
[106]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_106.png 
[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_200.png 
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_201.png 
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_203.png
[204]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_204.png
[205]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_205.png
[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png
[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png