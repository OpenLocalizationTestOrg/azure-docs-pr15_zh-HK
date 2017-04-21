<properties
    pageTitle="使用声明感知应用程序在应用程序代理"
    description="介绍如何使用 Azure AD 应用程序代理获取启动并运行。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>



# <a name="working-with-claims-aware-apps-in-application-proxy"></a>使用声明感知应用程序在应用程序代理

声明感知应用程序执行重定向到安全令牌服务 (STS)，其中又以标记交换用户请求凭据，将用户重定向到应用程序之前。 若要启用应用程序代理服务器以使用这些重定向的工作，需要采取以下步骤。

## <a name="prerequisites"></a>系统必备组件
之前执行此过程，请确保声明感知应用程序重定向到 STS 有您的内部网络之外。

## <a name="azure-classic-portal-configuration"></a>Azure 经典的入口配置

1. 发布您的应用程序，根据所述[应用程序代理的发布应用程序](active-directory-application-proxy-publish.md)的说明。
2. 在应用程序的列表中，选择声明感知应用程序并单击**配置**。
3. 如果您选择**直通**作为**预身份验证方法**，确保为**外部 URL**方案选择**HTTPS** 。
4. 如果您为您的**预身份验证方法**选择**Azure Active Directory** ，作为**内部身份验证方法**选择**无**。


## <a name="adfs-configuration"></a>ADFS 配置

1. 打开 ADFS 管理。
2. 转到**信赖方信任**，右键单击该应用程序使用应用程序代理，发布并选择**属性**。  
  ![信赖方信任右键单击应用程序名称-screentshot](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  
3. 在**终结点**选项卡上的**终结点的类型**，选择**WS 联合身份验证**。
4. 在**受信任的 URL**下输入应用程序代理服务器在**外部 URL**中输入的 URL 并单击**确定**。  
  ![添加终结点的设置受信任 URL 值的屏幕抓图](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="see-also"></a>请参见

- [发布应用程序与应用程序代理](active-directory-application-proxy-publish.md)
- [启用单一登录](active-directory-application-proxy-sso-using-kcd.md)
- [解决您在具有应用程序代理时遇到的问题](active-directory-application-proxy-troubleshoot.md)
- [启用本机客户端应用程序与代理服务器应用程序进行交互](active-directory-application-proxy-native-client.md)

最新的新闻和更新，为签出[应用程序代理日志](http://blogs.technet.com/b/applicationproxyblog/)
