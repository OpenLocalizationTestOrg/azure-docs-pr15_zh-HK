<properties
    pageTitle="教程︰ 与 Questetra 的 BPM 套件的 Azure Active Directory 集成 |Microsoft Aure"
    description="了解如何配置单一登录之间 Azure Active Directory 和 Questetra 的 BPM 套件。"
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
    ms.date="10/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>教程︰ 与 Questetra 的 BPM 套件的 Azure Active Directory 集成

本教程的目的是向您展示如何将 Questetra 的 BPM 套件集成使用 Azure 活动目录 (AD Azure)。  
与 Azure AD 集成 Questetra 的 BPM 套件提供了以下好处︰ 

- 您可以控制有权访问 Questetra 的 BPM 套件 Azure AD 中 
- 您可以其 Azure 的广告帐户使用户可以自动获取签名上到 Questetra （单一登录） 的 BPM 套件
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件 

若要配置 Azure 广告集成与 Questetra 的 BPM 套件，您需要以下各项︰

- Azure 广告订阅
- [Questetra 的 BPM 套件](https://senbon-imadegawa-988.questetra.net/)单点登录启用订阅


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。 

 
## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。  
在本教程中介绍的方案由三个主要的构造块组成︰

1. 从库中添加 Questetra 的 BPM 套件 
2. 配置和测试 Azure AD 单一登录


## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>从库中添加 Questetra 的 BPM 套件
若要配置集成到 Azure AD Questetra 的 BPM 套件，您需要将 Questetra 的 BPM 套件从库添加到托管的 SaaS 应用程序的列表。

**要从库中添加 Questetra 的 BPM 套件，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。

    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**Questetra 的 BPM 套件**。

    ![应用程序][5]

7. 在结果窗格中，选择**Questetra 的 BPM 套件**，，然后单击**完成**添加应用程序。



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分的目的是向您展示如何配置和使用基于一个名为"Britta Simon"的测试用户的 Questetra BPM 套件测试 Azure AD 单一登录。

对于单一登录工作，Azure 广告需要知道在 Azure AD 用户 Questetra BPM 套件中对应的用户。 换句话说，Azure AD 用户和在 Questetra 的 BPM 套件相关的用户之间的链接关系需要建立。  
此链接关系的建立在 Azure AD Questetra BPM 套件中的**用户名**的值中指定的**用户名**的值。
 
要配置和测试 Azure AD 单一登录与 Questetra 的 BPM 套件，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建一个 Questetra 的 BPM 套件测试用户](#creating-a-questetra-bpm-suite-test-user)**的链接到她的 Azure 广告表示的 Questetra BPM 套件中的测试具有 Britta Simon 的副本。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure 的广告单登录配置

本节的目的是 Azure AD 单一登录在 Azure 的传统门户网站中启用并配置单一登录应用程序 Questetra 的 BPM 套件中。

**要配置 Azure AD 单一登录 Questetra 的 BPM 套件，请执行以下步骤︰**

1. 在 Azure 传统门户网站，在**Questetra 的 BPM 套件**应用程序集成页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录][8]

2. 在**您想怎样来登录到 Questetra 的 BPM 套件的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![Azure 的广告单登录][9]


3. 在不同的 web 浏览器窗口中，登录公司网站**Questetra 的 BPM 套件**以管理员的身份。

4. 在菜单上，单击**系统设置**。 

    ![Azure 的广告单登录][10]

5. 若要打开**SingleSignOnSAML**页面，单击**SSO (SAML)**。 

    ![Azure 的广告单登录][11]


6. 在 Azure 的传统门户网站，在**配置应用程序设置**对话框页上，执行以下步骤︰ 

    ![配置应用程序设置][13]
 
    一。 在**Questetra 的 BPM 套件**公司网站，在 SP 信息部分中，您复制的**ACS URL**，，然后将其粘贴到该**符号在 URL**文本框。

    b。 在**Questetra 的 BPM 套件**公司网站，在 SP 信息部分中，您复制**的实体标识**，然后将其粘贴到该**颁发者 URL**文本框。

    c。 在**Questetra 的 BPM 套件**公司网站，在 SP 信息部分中，您复制的**ACS URL**，，然后将其粘贴到**回复 URL**文本框。

    d。 单击**下一步**。

 
7. 在**配置单一登录 Questetra 的 BPM 套件在**页上，单击**下载证书**，然后保存在您的计算机的本地证书文件。

    ![配置单一登录][14]


8. 在您公司网站**Questetra 的 BPM 套件**，请执行以下步骤︰ 

    ![配置单一登录][15]

    一。 选择**启用单一登录**。
     
    b。 在 Azure 的传统门户网站，将**颁发者 URL**值，复制，然后将其粘贴到**实体 ID**文本框。

    c。 在 Azure 的传统门户网站，将**单一登录服务 URL**值，复制，然后将其粘贴到**登录页的 URL**文本框。

    d。 在 Azure 的传统门户网站，复制**单个 Sign-Out 服务 URL**值，然后将其粘贴到**注销页 URL**文本框。

    电子。 在**NameID 格式**文本框中，键入**urn: oasis︰ 姓名︰ tc: SAML:1.1:nameid 的电子邮件地址格式︰**。


    f。 从您的下载证书创建 base 64 编码的文件。 

    >[AZURE.TIP] 有关详细信息，请参阅[如何将转换到一个文本文件的二进制证书](http://youtu.be/PlgrzUZ-Y1o)。

    g。 在记事本中打开您的 base 64 编码的证书，将其内容复制到剪贴板，然后将其粘贴到**检验证书**文本框。 

    h。 单击**保存**。


9. 在 Azure 的传统门户网站，选择单一登录配置进行确认，，然后单击**下一步**。 

    ![什么是 Azure AD 连接][17]


10. 在**单一登录确认**页上，单击**完成**。  

    ![什么是 Azure AD 连接][18]




### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon Azure 经典门户创建的测试用户。

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![创建 Azure 广告测试用户][100] 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![创建 Azure 广告测试用户][101] 

4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。 

    ![创建 Azure 广告测试用户][102] 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰

    ![创建 Azure 广告测试用户][103]
 
    一。 作为**用户的类型**，选择**您的组织中的新用户**。
  
    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击下一步。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰ 

    ![创建 Azure 广告测试用户][104] 
  
    一。 在**名字**文本框中，键入**Britta**。 
 
    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。

    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![创建 Azure 广告测试用户][105]  

8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![创建 Azure 广告测试用户][106]   
  
    一。 记下**新密码**的值。
  
    b。 单击**完成**。   
  
 
### <a name="creating-a-questetra-bpm-suite-test-user"></a>创建一个 Questetra 的 BPM 套件测试用户

本节的目的是创建一个名为 Britta Simon Questetra BPM 套件中的测试用户。

**若要创建一个名为 Britta Simon Questetra BPM 套件中的测试用户，请执行以下步骤︰**

1.  登录到您的 Questetra 的 BPM 套件公司网站管理员身份。
2.  转到**系统设置 > 用户列表 > 新用户**。 
3.  在新建用户对话框中，请执行以下步骤︰ 

    ![创建测试用户][300] 

    一。 在**名称**文本框中，键入 Britta 的用户名在 Azure AD 上。

    b。 在**电子邮件**文本框中，键入 Britta 的用户名在 Azure 的广告上。

    c。 在**密码**文本框中，键入一个密码。

4.  单击**添加新用户**。



### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon Questetra 的 BPM 套件中授予她访问使用 Azure 单一登录。

![什么是 Azure AD 连接][200]

**若要分配 Britta Simon Questetra 的 BPM 套件，请执行以下步骤︰**

1. 在 Azure 的传统门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![什么是 Azure AD 连接][201]

2. 在应用程序列表中，选择**Questetra 的 BPM 套件**。

    ![什么是 Azure AD 连接][205]

1. 在菜单上，单击**用户**。

    ![什么是 Azure AD 连接][202]

1. 在用户列表中，选择**Britta Simon**。

    ![什么是 Azure AD 连接][203]

2. 在底部工具栏中，单击**指派**。

    ![什么是 Azure AD 连接][204]



### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。  
当您单击访问权限面板中的 Questetra 的 BPM 套件拼贴时，您应该获取自动签名对 Questetra 的 BPM 套件应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_11.png 