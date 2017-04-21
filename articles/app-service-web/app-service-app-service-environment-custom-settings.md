<properties
    pageTitle="应用程序服务环境的自定义设置"
    description="应用程序服务环境的自定义配置设置"
    services="app-service"
    documentationCenter=""
    authors="stefsch"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="stefsch"/>

# <a name="custom-configuration-settings-for-app-service-environments"></a>应用程序服务环境的自定义配置设置

## <a name="overview"></a>概述 ##
由于应用程序服务环境被隔离到单个客户，有某些配置设置可以应用于应用程序服务环境以独占方式。 本文介绍了各种特定的自定义所提供的应用程序服务的环境。

如果您没有一个应用程序的服务环境，了解[如何创建一个应用程序的服务环境](app-service-web-how-to-create-an-app-service-environment.md)。

可以通过新的**clusterSettings**属性中使用数组存储的自定义应用程序服务环境。 此特性是在*hostingEnvironments*实体 Azure 资源管理器的"属性"词典中找到的。

下面的简化的资源管理器模板代码段显示了**clusterSettings**属性︰


    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

**ClusterSettings**属性可以包含在资源管理器模板来更新应用程序的服务环境。

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>使用 Azure 资源管理器来更新应用程序服务环境
或者，您可以通过使用[Azure 资源管理器](https://resources.azure.com)更新应用程序服务环境。  

1. 在资源管理器中，转到该节点的应用程序服务环境 (**订阅** > **resourceGroups** > **提供商** > **Micrososft.Web** > **hostingEnvironments**)。 然后单击您想要更新的特定应用程序服务环境。

2. 在右窗格中，请单击上面的工具栏允许交互式编辑在资源管理器中的**读/写**。  

3. 单击蓝色的**编辑**按钮，以使资源管理器模板的可编辑。

4. 滚动到底部的右窗格。 **ClusterSettings**属性是在底部，您可以在其中输入或更新其值。

5. 键入 （或复制并粘贴） 的**clusterSettings**属性中所需的配置值的数组。  

6. 单击绿色**放置**按钮的有位于顶部的右窗格中，若要将更改提交到应用程序的服务环境。

但是您提交更改时，它将大约 30 分钟的前端应用程序服务环境中的更改才能生效的数倍。
例如，如果应用程序服务环境具有四个前端，它需要大约两个小时完成的配置更新。 部署配置更改，而其他缩放操作或配置更改操作可以在应用程序的服务环境。

## <a name="disable-tls-10"></a>禁用 TLS 1.0 ##
从客户的周期性问题，尤其是处理 PCI 法规遵从性的客户审核，如何明确为其应用程序中禁用 TLS 1.0。

可以通过下面的**clusterSettings**项禁用 TLS 1.0:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>更改 TLS 密码套件顺序 ##
从客户的另一个问题是如果他们可以修改由其服务器协商的密码的列表，这可以通过如下所示修改**clusterSettings**来实现。 可以从 [此 MSDN 文章] 中检索密码套件可用列表 (https://msdn.microsoft.com/library/windows/desktop/aa374757(v=vs.85\).aspx)。

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [AZURE.WARNING]  如果 SChannel 无法理解的密码套件的设置不正确的值，则所有与您的服务器的 TLS 通信可能停止正常运行。 在这种情况下，您将需要从**clusterSettings**中删除*FrontEndSSLCipherSuiteOrder*条目并提交更新的资源管理器模板用于返回到默认的密码套件设置。  请谨慎使用此功能。

## <a name="get-started"></a>入门
Azure 快速启动资源管理器模板网站包括的模板[创建的应用程序服务环境](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/)的基本定义。


<!-- LINKS -->

<!-- IMAGES -->
