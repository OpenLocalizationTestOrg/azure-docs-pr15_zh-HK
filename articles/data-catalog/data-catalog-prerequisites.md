<properties
   pageTitle="Azure 数据目录系统必备组件 |Microsoft Azure"
   description="Azure 数据目录先决条件-您需要开始使用 Azure 数据目录。"
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-prerequisites"></a>Azure 数据目录系统必备组件

## <a name="what-do-i-need-to-get-started-with-azure-data-catalog"></a>开始使用 Azure 数据目录需要什么？

有几件事情需要之前您可以设置**Azure 数据目录**的关照。 别担心--他们不会花长时间 ！

## <a name="azure-subscription"></a>Azure 的订阅
若要设置 Azure 数据目录，您必须是所有者或订阅了 Azure 的合作者。

Azure 订阅帮助您组织类似 Azure 数据目录的云服务资源的访问。 它们还可以帮助您控制资源使用状况的报告方式，记帐时间，以及支付的。 每个订阅可以具有不同的帐单和付款的安装，因此可以有不同的预订和不同的部门、 项目、 区域办事处，等计划。 每个云服务属于订阅，您需要设置 Azure 数据目录之前订购。 若要了解详细信息，请参阅[管理帐户、 预订和管理角色](../active-directory/active-directory-assign-admin-roles.md)。

## <a name="azure-active-directory"></a>Azure 的活动目录
若要设置 Azure 数据目录，您必须使用 Azure 活动目录的用户帐户登录。

Azure 活动目录 (AD Azure) 可以轻松地为您的业务来管理身份和访问，同时在云和内部。 用户可以使用一个单一的工作或学校账户的单一登录到任何云和内部部署 web 应用程序。 Azure 数据目录使用 Azure 的广告来对登录进行身份验证。 若要了解详细信息，请参阅[什么是 Azure 活动目录](../active-directory/active-directory-whatis.md)。

> [AZURE.NOTE] [Azure 的门户网站](http://portal.azure.com/)，用户可以登录使用 Microsoft 个人帐户或 Azure Active Directory 工作或学校的帐户。 若要设置 Azure 数据目录使用 Azure 门户或使用[数据目录入口](http://www.azuredatacatalog.com)必须记录在使用 Azure Active Directory 帐户，而不是个人帐户。

## <a name="active-directory-policy-configuration"></a>活动目录策略配置

在某些情况下，用户可能会遇到他们从何处可以登录到 Azure 数据目录门户，但在尝试登录到数据源注册工具遇到错误消息，可以阻止登录时的情况。 仅当用户在公司网络上，或者只有当用户从公司网络外部连接时可能会发生，则可能会发生此问题。

数据源注册工具使用窗体身份验证来验证对 Active Directory 用户登录。 对于成功的登录窗体身份验证必须在全局身份验证策略中启用 Active Directory 管理员。

全局身份验证策略允许为企业内部网和外部网的连接，分别启用的身份验证方法，如下所示。 如果未启用窗体身份验证的用户进行连接的网络，则可能会出现登录错误。

 ![Active Directory 全局身份验证策略](./media/data-catalog-prerequisites/global-auth-policy.png)

有关详细信息，请参阅[配置身份验证策略](https://technet.microsoft.com/library/dn486781.aspx)。
