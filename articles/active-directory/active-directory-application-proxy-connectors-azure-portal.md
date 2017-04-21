<properties
    pageTitle="使用 Azure AD 应用程序代理连接器 |Microsoft Azure"
    description="介绍如何创建和管理组的 Azure AD 应用程序代理中的连接器。"
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
    ms.date="09/09/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups---public-preview"></a>发布应用程序在独立的网络和位置使用连接器组-公共预览

> [AZURE.SELECTOR]
- [Azure 门户](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure 的传统门户网站](active-directory-application-proxy-connectors.md)


接口组可用于各种方案，包括︰

- 具有多个相互连接的数据中心站点。 在这种情况下，要尽可能保持尽可能对数据中心内的通信，因为跨数据中心链接是昂贵和速度慢。 您可以部署中每个数据中心，以服务于驻留在数据中心内应用程序的接口。 这种方法减少跨数据中心链接，为用户提供一种完全透明的体验。
- 管理隔离不是主要的公司网络的一部分的网络上安装的应用程序。 可以使用连接器组还找出应用程序到网络的隔离网络上安装专用的连接器。
- 对于安装在 IaaS 云访问的应用程序，连接器组提供公共服务来保护对所有应用程序的访问权限。 接口组不在公司网络上创建附加依赖项或片段应用程序体验。 连接器可以安装在每个云数据中心，从而为仅驻留在此网络中的应用程序服务。 您可以安装多个连接器来实现高可用性。
- 多目录林环境中的特定连接器可以部署每个林和设置来满足特定应用程序的支持。
- 连接器组用于在灾难恢复站点，或者检测到故障转移或作为备份的主站点。
- 连接器组还可以用于从单个租户提供多家公司。

## <a name="prerequisite-create-your-connectors"></a>前提条件︰ 创建连接器
若要组合连接器，您必须确保您[安装多个连接器](active-directory-application-proxy-enable.md)。 安装一个新的连接器时，它自动连接的**默认**接口组。

## <a name="step-1-create-connector-groups"></a>步骤 1︰ 创建接口组
您可以创建任意多个连接器的组。 创建接口组是[Azure 门户](https://portal.azure.com)中完成的。

1. 选择**Azure Active Directory**以转到您的目录管理仪表板。 据此，选择**企业应用程序** > **应用程序代理**。

2. 选择**连接符组**按钮。 此时将显示新连接器组刀片。

3. 为您新的连接器组指定一个名称，然后使用下拉菜单中选择属于此组的连接器。

4. 完成您的连接器组时，请选择**保存**。

## <a name="step-2-assign-applications-to-your-connector-groups"></a>步骤 2︰ 分配应用程序到您的接口组
最后一步是设置为将提供该连接器组的每个应用程序。

1. 从目录管理仪表板，选择**企业应用程序** > **的所有应用程序**> 应用程序要将分配给接口组 >**应用程序代理**。
2. 在**连接器组**，使用下拉菜单中选择您想要使用的应用程序的组。
3. 选择**保存**以应用更改。


## <a name="see-also"></a>请参见

- [启用应用程序代理](active-directory-application-proxy-enable.md)
- [启用单一登录](active-directory-application-proxy-sso-using-kcd.md)
- [启用条件访问](active-directory-application-proxy-conditional-access.md)
- [解决您在具有应用程序代理时遇到的问题](active-directory-application-proxy-troubleshoot.md)

最新的新闻和更新，为签出[应用程序代理日志](http://blogs.technet.com/b/applicationproxyblog/)
