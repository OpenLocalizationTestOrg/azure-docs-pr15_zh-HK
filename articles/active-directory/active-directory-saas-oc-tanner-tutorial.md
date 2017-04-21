<properties
    pageTitle="教程︰ 与 O.C.的 Azure Active Directory 集成 Tanner-AppreciateHub |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 O.C.之间 Tanner-AppreciateHub。"
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a>教程︰ 与 O.C.的 Azure Active Directory 集成 Tanner-AppreciateHub

本教程的目的是向您显示如何集成 O.C. Tanner-AppreciateHub Azure 使用活动目录 (AD Azure)。  
将集成 O.C. Tanner-使用 Azure AD AppreciateHub 为您提供以下好处︰ 

- 您可以控制有权访问 O.C.Azure AD 中 Tanner-AppreciateHub 
- 您可以使用户可以自动获取签名上到 O.C. Tanner-AppreciateHub （单一登录） 其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置的 Azure 的传统门户网站

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>系统必备组件 

要配置 O.C.Azure 广告集成 Tanner-AppreciateHub，您需要以下各项︰

- Azure 广告订阅
- O.C. Tanner-AppreciateHub 单点登录启用订阅


> [AZURE.NOTE] 若要测试步骤在本教程中，我们不建议使用生产环境中。


若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。 

 
## <a name="scenario-description"></a>方案说明
本教程的目的是使您可以在测试环境中测试 Azure AD 单一登录。  
在本教程中介绍的方案由三个主要的构造块组成︰

1. 添加 O.C. Tanner-AppreciateHub 库中 
2. 配置和测试 Azure AD 单一登录


## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a>添加 O.C. Tanner-AppreciateHub 库中
若要配置 O.C.集成 Tanner-您需要添加 O.C.到 Azure AD AppreciateHub， Tanner-库从 AppreciateHub 到托管的 SaaS 应用程序的列表。

**若要添加 O.C.Tanner-AppreciateHub 从库，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1] 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2] 

4. 单击页面底部的**添加**。

    ![应用程序][3] 

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4] 

6. 在搜索框中，键入**O.C.Tanner-AppreciateHub**。

    ![应用程序][5] 

7. 在结果窗格中，选择**O.C.Tanner-AppreciateHub**，，然后单击**完成**添加应用程序。

    ![应用程序][25] 




##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

本节的目的是向您展示如何配置和 Azure AD 单一登录 O.C.与测试 Tanner-AppreciateHub 基于一个名为"Britta Simon"的测试用户。

单一登录工作，Azure 广告需要知道 O.C.中对应的用户 Tanner-是 AppreciateHub 到 Azure AD 中的某个用户。 换句话说，Azure AD 用户和相关的 O.C.用户之间的链接关系 Tanner-AppreciateHub 需要建立。  
通过 Azure AD O.C.**用户名**的值中指定的**用户名**的值建立此链接关系 Tanner-AppreciateHub。
 
若要配置和 Azure AD 单一登录 O.C.与测试 Tanner-AppreciateHub，您需要完成以下构造块︰

1. **[Azure AD 配置单一登录](#configuring-azure-ad-single-single-sign-on)**-若要使用户可以使用此功能。
2. **[创建 Azure 广告测试用户](#creating-an-azure-ad-test-user)**的 Azure AD 单一登录 Britta Simon 与测试。
4. **[创建 O.C.Tanner-AppreciateHub 测试用户](#creating-a-halogen-software-test-user)**-在 O.C.让 Britta Simon 的对应的 Tanner-AppreciateHub，链接到她的 Azure 广告表示。
5. **[分配的 Azure 广告测试用户](#assigning-the-azure-ad-test-user)**-启用 Britta Simon Azure AD 单一登录使用。
5. **[测试单一登录](#testing-single-sign-on)**的验证配置是否有效。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure 的广告单登录配置

本节的目的是 Azure AD 单一登录在 Azure 的传统门户网站中启用并配置单一登录您 O.C. Tanner-AppreciateHub 应用程序。


**Azure AD 单一登录配置与 O.C.Tanner-AppreciateHub，请执行以下步骤︰**

1. 在 Azure 的经典门户， **O.C.Tanner-AppreciateHub**应用程序集成在页上，单击**配置单一登录****配置单一登录**对话框打开。

    ![配置单一登录][6]

2. 在**您希望如何登录到 O.C.Tanner-AppreciateHub 的用户**页上，选择**Azure AD Single Sign-on**，，然后单击**下一步**。

    ![Azure 的广告单登录][7]

3. 在**配置应用程序设置**对话框页面上，请执行以下步骤︰

    ![配置应用程序设置][8]
 
     一。 打开元数据文件使用下面的链接︰ [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata)。

     b。 查找**md:AssertionConsumerService**节点。 

     c。 **位置**属性的值进行复制。 

     ![配置应用程序设置][12]
     
     d。 在**符号上 URL**文本框的值超过获得上一步中。

     > [AZURE.NOTE] 如果您正在答复 URL 获取元数据文件中的 expiriencing 问题，请联系 O.C. Tanner-AppreciateHub 的支持团队，通过[sso@octanner.com](mailto:sso@octanner.com)。

     电子。 单击**下一步**。
 
4. 在**配置单一登录 O.C.Tanner-AppreciateHub 在**页上，单击**下载元数据**，并保存在您的计算机上的本地元数据文件。

    ![什么是 Azure AD 连接][9]

5. 请联系 O.C. Tanner-AppreciateHub 的支持团队，xyz，通过为他们提供的元数据文件，并且他们让他们知道他们应该为您启用 SSO。


6. 在 Azure 的传统门户网站，选择单一登录配置进行确认，，然后单击**下一步**。 

    ![什么是 Azure AD 连接][10]

7. 在**单一登录确认**页上，单击**完成**。  

    ![什么是 Azure AD 连接][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure 广告测试用户
本节的目的是在调用 Britta Simon Azure 经典门户创建的测试用户。  

![创建 Azure AD 用户][20]

**在 Azure AD 中创建一个测试用户，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。

    ![Azure 广告测试用户](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要显示列表的用户，在顶部菜单中，单击**用户**。

    ![Azure 广告测试用户](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 
 
4. 若要打开**添加用户**对话框中，在底部工具栏中，单击**添加用户**。 

    ![Azure 广告测试用户](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

5. 在**告诉我们有关此用户**对话框页面上，请执行以下步骤︰ 

    ![Azure 广告测试用户](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_05.png) 

    一。 作为用户类型，选择您的组织中的新用户。

    b。 在用户名**文本框**中，键入**BrittaSimon**。

    c。 单击**下一步**。

6.  在**用户配置文件**对话框页面上，请执行以下步骤︰ 

    ![Azure 广告测试用户](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_06.png)
 
    一。 在**名字**文本框中，键入**Britta**。  

    b。 在**姓氏**文本框，类型， **Simon**。

    c。 在**显示名称**文本框中，键入**Britta Simon**。

    d。 在**角色**列表中，选择**用户**。
    电子。 单击**下一步**。

7. 在**获得临时密码**对话框页面上，单击**创建**。

    ![Azure 广告测试用户](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_07.png) 
 
8. 在**获得临时密码**对话框页面上，请执行以下步骤︰

    ![Azure 广告测试用户](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_08.png) 
  
    一。 记下**新密码**的值。

    b。 单击**完成**。   

  
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a>创建 O.C. Tanner-AppreciateHub 测试用户

本节的目的是创建用户调用 O.C.Britta Simon Tanner-AppreciateHub。

**要创建一个名为 Britta Simon O.C.Tanner-AppreciateHub，在用户执行以下步骤︰**

1. 询问您的 OC Tanner 支持团队来创建用户如 nameID 属性在 Azure AD Britta Simon 的用户名的值相同。


### <a name="assigning-the-azure-ad-test-user"></a>将 Azure 广告测试用户分配

本节的目的是使 Britta Simon O.C.授予她访问使用 Azure 单一登录 Tanner-AppreciateHub。

![分配用户][200]

**将 Britta Simon 给 O.C.Tanner-AppreciateHub，请执行以下步骤︰**

1. 在 Azure 的传统门户网站，若要打开应用程序视图中，目录视图中，单击**应用程序**顶部的菜单中。

    ![分配用户][201]

2. 在应用程序列表中，选择**O.C.Tanner-AppreciateHub**。

    ![分配用户][202]

1. 在菜单上，单击**用户**。

    ![分配用户][203]

1. 在用户列表中，选择**Britta Simon**。

2. 在底部工具栏中，单击**指派**。

    ![分配用户][205]



### <a name="testing-single-sign-on"></a>单一登录测试

本部分的目的是测试 Azure AD 单一登录配置使用访问权限面板。  
当您单击 O.C. Tanner-AppreciateHub 平铺在访问面板中，您应该获取自动签名上到您 O.C. Tanner-AppreciateHub 应用程序。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_01.png
[25]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_25.png


[6]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_02.png
[8]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_03.png
[9]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_04.png
[10]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_05.png
[11]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_06.png
[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png
[20]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_07.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_205.png






