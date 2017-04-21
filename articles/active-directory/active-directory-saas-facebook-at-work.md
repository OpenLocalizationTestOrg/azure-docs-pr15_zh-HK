<properties
    pageTitle="教程︰ Azure Active Directory 集成与 Facebook 在工作 |Microsoft Azure"
    description="了解如何配置单一登录 Azure Active Directory 和 Facebook 之间在工作。"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-facebook-at-work"></a>教程︰ Azure Active Directory 集成与 Facebook 在工作

本教程的目的是向您展示如何使用 Azure 活动目录 (AD Azure) 集成 Facebook 在工作。

与 Azure AD 集成 Facebook 在工作提供了以下好处︰ 

- 您可以控制在 Azure 上班时有权访问 Facebook 的广告 
- 您可以自动配置帐户的用户已被授予访问 Facebook 在工作
- 您可以使用户可以自动获取签名上到 Facebook （单一登录） 的工作在其 Azure 的广告帐户
- 您可以管理您的帐户在一个中心位置 

如果您想要了解有关使用 Azure AD 的 SaaS 应用程序集成的更多详细信息，请参阅[什么是应用程序访问和使用 Azure Active Directory 的单一登录](active-directory-appssoaccess-whatis.md)。


## <a name="prerequisites"></a>系统必备组件 

若要配置与 CS 星 Azure 广告集成，您需要以下各项︰

- Azure 广告订阅
- 在使用单一登录 Facebook 启用

若要测试步骤在本教程中，您应按照这些建议︰

- 您不应使用生产环境中，除非这是必要的。
- 如果没有 Azure AD 试用环境，您可以获得一个月试用版在[这里](https://azure.microsoft.com/pricing/free-trial/)。 


## <a name="adding-facebook-at-work-from-the-gallery"></a>从库中添加 Facebook 在工作
若要配置的 Facebook 集成到 Azure 广告的工作，需要 Facebook 在工作从库向列表中添加托管的 SaaS 应用程序。

**若要在工作中从库添加 Facebook，请执行以下步骤︰**

1. 在**Azure 的传统门户网站**，在左侧的导航窗格中，单击**活动目录**。 

    ![活动目录][1]

2. 从**目录**列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图中，目录视图中，单击顶部的菜单中的**应用程序**。

    ![应用程序][2]

4. 单击页面底部的**添加**。
    
    ![应用程序][3]

5. 在**您想要执行**对话框中，单击**添加应用程序从库**。

    ![应用程序][4]

6. 在搜索框中，键入**Facebook 在工作**。

7. 在结果窗格中，选择**工作时的 Facebook**，，然后单击**完成**添加应用程序。


### <a name="configuring-azure-ad-single-sign-on"></a>Azure 的广告单登录配置

这一节概述如何使用户能够与他们 Azure Active Directory，使用联合身份验证基于 SAML 协议中的帐户验证到 Facebook 在工作。

**要配置 Azure AD 单一登录 Facebook 在工作，请执行以下步骤︰**

1.  添加之后 Facebook 在工作在 Azure 经典门户，单击**配置单一登录**。

2.  在**配置应用程序 URL**屏幕中，输入的 URL，用户将登录到您的工作应用程序在 Facebook。 这是您工作租户 URL 处的 Facebook (示例︰ https://example.facebook.com/)。 完成后，单击**下一步**。

3.  在不同的 web 浏览器窗口中，以管理员身份登录到您工作的公司网站上的 Facebook。

4. 按照说明从下面的 URL 配置 Facebook 在工作作为标识提供程序使用 Azure 的广告︰ [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)

5.  一旦完成，返回到浏览器窗口，显示 Azure 的传统门户网站，请单击该复选框以确认您已完成该过程，然后单击**下一步**和**完成**。


## <a name="automatically-provisioning-users-to-facebook-at-work"></a>自动资源调配工作的 Facebook 用户

Azure 的广告支持自动 synchonize 的能力到 Facebook 在工作分配的用户的帐户详细信息。 此自动同步使 Facebook 工作授权用户进行访问，之前他们正在尝试登录的第一次所需的数据。 它还消除规定工作的 Facebook 用户已在 Azure AD 中撤销访问权限时。

自动资源调配可以为通过单击设置 Azure 的经典门户窗口中**配置帐户设置**。

有关如何配置自动提供的附加详细信息，请参见[https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)


## <a name="assigning-users-to-facebook-at-work"></a>将用户分配到 Facebook 在工作

调配 AAD 用户能够在其接入面板上看到 Facebook 在工作，他们必须将分配在 Azure 的传统门户网站的访问。

**若要将用户分配到 Facebook 在工作︰**

1.  在 Facebook 在工作在 Azure 的传统门户网站的起始页，单击**指定帐户**。

2.  在**显示**菜单中，选择是否要为 Facebook 在工作分配的用户或组，然后单击复选标记按钮。

3.  在结果列表中，选择要向其分配工作在 Facebook 的用户或组。

4.  在页面页脚中，请单击**分配**按钮。


## <a name="additional-resources"></a>其他资源

* [如何将与 Azure Active Directory 集成在 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png




