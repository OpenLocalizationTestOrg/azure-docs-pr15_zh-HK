<properties
pageTitle="Azure 的活动目录应用程序和服务主体对象 |Microsoft Azure"
description="应用程序和服务在 Azure Active Directory 中的主体对象之间的关系的讨论"
documentationCenter="dev-center-name"
authors="bryanla"
manager="mbaldwin"
services="active-directory"
editor=""/>

<tags
ms.service="active-directory"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="identity"
ms.date="08/10/2016"
ms.author="bryanla;mbaldwin"/>

# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>应用程序和服务在 Azure Active Directory 中的主体对象
当您阅读有关 Azure 活动目录 (AD)"应用程序"时，并不总是清楚确切所指作者。 本文的目的是使它更清晰，通过定义与注册和同意的情况下为[多租户应用程序](active-directory-dev-glossary.md#multi-tenant-application)的示例 Azure AD 应用程序集成的概念和具体方面。

## <a name="overview"></a>概述
Azure AD 应用程序就是比一张只是软件的范围更广。 它是概念术语，不仅指应用程序软件，而且其注册 (亦即︰ 标识配置) 与 Azure 的广告，这使得它能够参与身份验证和授权的"对话"在运行时。 根据定义，应用程序可以在[客户端](active-directory-dev-glossary.md#client-application)角色 （使用资源）、[资源服务器](active-directory-dev-glossary.md#resource-server)角色 (公开 Api 客户端)，或甚至两个正常。 [OAuth 2.0 授权授予流](active-directory-dev-glossary.md#authorization-grant)，并允许客户机资源的目标来访问/保护资源的数据分别由定义对话协议。 现在让我们更深，一个级别，请参阅 AD Azure 应用程序模型内部表示应用程序如何。 

## <a name="application-registration"></a>应用程序注册
在[Azure 的传统门户网站]注册应用程序时[AZURE-Classic-Portal]，在 Azure AD 租户中创建两个对象︰ 一个应用程序和服务的主要对象。

#### <a name="application-object"></a>应用程序对象
Azure AD 应用程序就是通过它一个*定义*，只有应用程序对象，驻留在应用程序中注册 Azure AD 租户，称为应用程序的"家庭"租户。 应用程序对象提供与身份相关的信息，对于应用程序，并且从中及其相应的服务的主要对象是用于在运行时*派生*模板。 

您可以将*全局*（用于跨所有承租人），应用程序的表示形式的应用程序和服务主体 （用于特定租户）*本地*表示形式。 Azure 广告图形[应用程序实体][AAD-Graph-App-Entity]定义的应用程序对象的架构。 应用程序对象因此具有 1:1 的关系与软件应用程序，并为 1:*n*关系及其相应的*n*服务主体对象。

#### <a name="service-principal-object"></a>服务主体对象
服务主体对象定义的策略和权限的应用程序，提供安全主体访问在运行时的资源时，则表示该应用程序的基础。 Azure 广告图形[实体 ServicePrincipal] [AAD-Graph-Sp-Entity]定义了一个服务主体对象的架构。 

服务主体对象为其应用程序的用法的一个实例必须表示，启用对来自该租户的用户帐户所拥有资源的安全访问每个租户的需要。 单租户应用程序 （在其住宅租户） 都只有一个服务主体。 多租户[Web 应用程序](active-directory-dev-glossary.md#web-client)中，管理员或用户从该租户已授予同意的情况下，允许它访问其资源每个租户还有服务主体。 之后同意的情况下，将已用于以后的授权请求咨询服务的主要对象。 

> [AZURE.NOTE] 对您的应用程序对象所做的任何更改也反映在其服务主体对象在应用程序的主租户只 （注册该租户） 中。 对于多租户应用程序，对应用程序对象的更改不会反映在任何使用者租户的服务主体对象，直到使用者租户移除访问权限并再次授予访问权限。

## <a name="example"></a>示例
下图演示应用程序的应用程序对象之间对应关系服务主体对象，称为**HR 应用程序**示例多租户应用程序的上下文中。 在这种情况下有三个 Azure AD 承租人︰ 

- **Adatum** -租户公司开发**人力资源应用程序**的使用
- **Contoso** -Contoso 组织，即**人力资源应用程序**的使用者使用的租户
- **Fabrikam**的 Fabrikam 组织，还消耗**HR 应用程序**使用的租户

![应用程序对象和服务主体对象之间的关系](./media/active-directory-application-objects/application-objects-relationship.png)

在上图中，第 1 步是在应用程序的主租户中创建的应用程序和服务的主体对象的过程。

在步骤 2 中，Contoso 和 Fabrikam 管理员完成后同意的情况下，服务的主要对象是公司的 Azure AD 租户中创建并分配了管理员授予的权限。 此外请注意，HR 应用程序无法配置/设计成使同意单独使用的用户。

在步骤 3 中，HR 应用程序 （Contoso 和 Fabrikam） 每个使用者承租人有自己服务的主要对象。 由各自的管理员每种代表他们的应用程序在运行时，受权限的实例使用许可。

## <a name="next-steps"></a>下一步行动
由其 OData[应用程序实体]，可以通过 Azure 广告图形 API，访问应用程序的应用程序对象[AAD-Graph-App-Entity]

可以通过 Azure 广告图形 API，访问应用程序服务的主要对象，由其 OData [ServicePrincipal 实体][AAD-Graph-Sp-Entity]



<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Classic-Portal]: https://manage.windowsazure.com