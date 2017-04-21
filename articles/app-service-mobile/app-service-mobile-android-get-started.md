<properties
    pageTitle="在 Azure 应用程序服务移动应用程序上创建一个 Android 的应用程序 |Microsoft Azure"
    description="按照本教程中使用 Azure 的移动应用程序 backends 为 Android 开发入门"
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

#<a name="create-an-android-app"></a>创建一个 Android 的应用程序

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>概述

本教程展示如何使用 Azure 的移动应用程序的后端 Android 的移动应用程序添加一个基于云的后端服务。  您将创建新的移动应用程序后端和一个简单的_Todo 列表_在 Azure 存储应用程序数据的 Android 应用程序。

学完本教程是在 Azure 应用程序服务使用移动应用程序功能有关的所有其他 Android 教程的先决条件。

## <a name="prerequisites"></a>系统必备组件

若要完成本教程，您需要以下各项︰

* [Android 开发人员工具](https://developer.android.com/sdk/index.html)，其中包括 Android Studio 集成的开发环境，以及最新的 Android 平台。
* Azure 手机 Android SDK，引用时自动为您下载快速启动项目的一部分。
* [活动的 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="create-a-new-azure-mobile-app-backend"></a>创建新 Azure 的移动应用程序的后端

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>配置服务器项目

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-android-app"></a>下载并运行 Android 的应用程序

[AZURE.INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
