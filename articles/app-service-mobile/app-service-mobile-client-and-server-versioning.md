<properties
  pageTitle="在移动应用程序和移动服务的客户端和服务器 SDK 版本 |Azure 应用程序服务"
  description="客户端 Sdk 的列表以及与移动服务和 Azure 移动应用程序的服务器 SDK 版本兼容性"
  services="app-service\mobile"
  documentationCenter=""
  authors="adrianhall"
  manager="erikre"
  editor=""/>

<tags
  ms.service="app-service-mobile"
  ms.workload="mobile"
  ms.tgt_pltfrm="mobile-multiple"
  ms.devlang="dotnet"
  ms.topic="article"
  ms.date="10/01/2016"
  ms.author="adrianha"/>

# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>在移动应用程序和移动服务的客户端和服务器版本控制

最新版本的 Azure 移动服务是 Azure 应用程序服务的**移动应用程序**功能。

移动应用程序客户端和服务器 Sdk 最初基于那些在移动服务，但它们*不*互相兼容。
也就是说，必须使用*移动应用程序*客户端 SDK 同样使用 SDK 的*移动应用程序*服务器和*移动服务*。 通过使用客户端和服务器的 Sdk，特殊标头值强制执行此合同`ZUMO-API-VERSION`。

注意︰ 只要本文档涉及到*移动服务*后端，它不一定需要驻留在移动服务。 现在可能要迁移而无需改动代码，运行的应用程序服务移动服务但仍会将服务使用*移动服务*SDK 版本。

若要了解有关迁移到应用程序服务，无需进行任何代码更改的详细信息，请参阅文章[迁移到 Azure 应用程序服务移动服务]。

## <a name="header-specification"></a>标头说明

键`ZUMO-API-VERSION`可能指定的 HTTP 标头或查询字符串中。 值为**x.y.z**窗体中的版本字符串。

例如︰

获得 https://service.azurewebsites.net/tables/TodoItem

标题︰ ZUMO-API-版本︰ 2.0.0

开机自检 https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>选择不进行版本检查

您可以选择不进行版本检查通过设置**为**应用程序设置**MS_SkipVersionCheck**的值。 在 web.config 或在 Azure 门户应用程序设置部分中指定此文件。

> [AZURE.NOTE] 有数间移动服务和移动应用程序，特别是在脱机同步、 身份验证和推式通知区域的行为改变。 您应只选择不版本检查后完整的测试，以确保这些行为上的改变不会中断应用程序的功能。

## <a name="summary-of-compatibility-for-all-versions"></a>所有版本的兼容性摘要

下面的图表显示了客户端和服务器的所有类型之间的兼容性。 后端被归类为移动**服务**或移动**应用程序**基于服务器使用的 SDK。

|                           | **移动服务**Node.js 或.NET | **移动应用程序**Node.js 或.NET |
| ----------                | -----------------------             |   ----------------              |
| [移动服务客户端] | 还行                                  | 错误\*                         |
| [移动应用程序客户端]     | 错误\*                             | 还行                              |

\*这可以通过指定**MS_SkipVersionCheck**来控制。


<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>移动服务客户端和服务器

下表中的客户端 Sdk 都与**移动服务**兼容。

注意︰ 移动服务客户端 Sdk*并不*发送标头值`ZUMO-API-VERSION`。 如果该服务收到此标头或查询字符串值，将返回错误，除非您已经明确选择出按上文所述。

### <a name="MobileServicesClients"></a>移动*服务*客户端 Sdk

| 客户端平台                   | 版本                                                                   | 版本的标头值 |
| -------------------               | ------------------------                                                  | -------------------  |
| 托管客户端 （Windows，Xamarin） | [1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) | n/a                  |
| iOS                               | [2.2.2](http://aka.ms/gc6fex)                                             | n/a                  |
| Android                           | [2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126)                   | n/a                  |
| HTML                              | [1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) | n/a     |

### <a name="mobile-services-server-sdks"></a>移动*服务*服务器的 Sdk

| 服务器平台  | 版本                                                                                                        | 已接受的版本标头 |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [WindowsAzure.MobileServices.Backend.* 版本 1.0.x 版](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) | **没有版本标头** |
| Node.js          | （即将提供）                        | **没有版本标头** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>移动服务 backends 的行为

| ZUMO API 版本 | MS_SkipVersionCheck 的值 | 响应 |
| ---------------- | ---------------------------- | -------- |
| 未指定    | 任何                          | 200 OK |
| 任何值        | 真                         | 200 OK |
| 任何值        | 假/未指定          | 400-错误的请求 |

## <a name="2.0.0"></a>Azure 的移动应用程序客户端和服务器

### <a name="MobileAppsClients"></a>移动*应用程序*客户端 Sdk

用下面的客户端 SDK 版本开始用于**Azure 的移动应用程序**引入版本检查︰

| 客户端平台                   | 版本                   | 版本的标头值 |
| -------------------               | ------------------------  | -----------------    |
| 托管客户端 （Windows，Xamarin） | [2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) | 2.0.0 |
| iOS                               | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) | 2.0.0  |
| Android                           | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) | 3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>移动*应用程序*服务器的 Sdk

版本检查包括在以下服务器 SDK 版本︰

| 服务器平台  | SDK                                                                                                        | 已接受的版本标头 |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) | 2.0.0 |
| Node.js          | [azure 移动应用）](https://www.npmjs.com/package/azure-mobile-apps)                         | 2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>移动应用程序 backends 的行为

| ZUMO API 版本 | MS_SkipVersionCheck 的值 | 响应 |
| ---------------- | ---------------------------- | -------- |
| x.y.z 或空值    | 真                         | 200 OK |
| 空值             | 假/未指定          | 400-错误的请求 |
| 1.x.y            | 假/未指定          | 400-错误的请求 |
| 2.0.0-2.x.y      | 假/未指定          | 200 OK |
| 3.0.0-3.x.y      | 假/未指定          | 400-错误的请求 |


## <a name="next-steps"></a>下一步行动

- [将移动服务迁移到 Azure 应用程序服务]


[移动服务客户端]: #MobileServicesClients
[移动应用程序客户端]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[将移动服务迁移到 Azure 应用程序服务]: app-service-mobile-migrating-from-mobile-services.md

