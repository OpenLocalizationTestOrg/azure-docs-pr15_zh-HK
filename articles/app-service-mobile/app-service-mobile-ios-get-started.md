<properties
    pageTitle="在 Azure 应用程序服务移动应用程序上创建 iOS 应用程序 |Microsoft Azure"
    description="按照本教程中使用 Azure 的移动应用程序 backends 为目标 C 或 Swift 的 iOS 开发入门"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

#<a name="create-an-ios-app"></a>创建 iOS 应用程序

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>概述

本教程展示如何将[Azure 移动应用程序](app-service-mobile-value-prop.md)，云后端服务，添加到 iOS 应用程序。 首先，我们将创建新的移动后端。 然后，我们将使用一个简单的_Todo 列表_iOS 应用程序在 Azure 存储数据。

若要完成本教程，您需要一个 Mac 和[Azure 帐户](https://azure.microsoft.com/pricing/free-trial/)


## <a name="step-i-create-a-new-azure-mobile-app-backend"></a>单步执行 i︰ 创建新 Azure 的移动应用程序的后端

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="step-ii-configure-the-backend-project"></a>第二步︰ 配置后端项目

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="step-iii-download-and-run-the-ios-app"></a>第三步︰ 下载并运行 iOS 应用程序

[AZURE.INCLUDE [app-service-mobile-ios-run-app](../../includes/app-service-mobile-ios-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
