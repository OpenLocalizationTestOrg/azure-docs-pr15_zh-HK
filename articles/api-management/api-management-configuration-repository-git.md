<properties 
    pageTitle="如何保存和配置您使用 Git 的 API 管理服务配置" 
    description="了解如何保存和配置您使用 Git 的 API 管理服务配置。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>如何保存和配置您使用 Git 的 API 管理服务配置

>[AZURE.IMPORTANT] Git 配置 API 管理正在预览。 功能上完成，但处于预览，因为我们正在积极寻求对此功能的反馈。 很可能，我们可能会使更改以响应客户反馈意见，因此建议在生产环境中使用此功能根据不间断。 如果您有任何反馈或问题，请让我们知道`apimgmt@microsoft.com`。

每个 API 管理服务实例维护的配置数据库中包含有关配置和服务实例的元数据信息。 通过在发布服务器上的门户更改设置，使用 PowerShell cmdlet，或 REST API 调用，可以对服务实例进行更改。 除了这些方法，您还可以管理您的服务实例配置使用 Git，如启用服务管理方案︰

-   配置版本控制-下载并存储不同版本的服务配置
-   大容量配置更改-更改服务配置本地存储库中的多个部件并结合单个操作回发到服务器的更改
-   熟悉的 Git toolchain 和工作流-使用 Git 工具和工作流，您已熟悉

下图显示了不同的方式来配置您的 API 管理服务实例的概述。

![Git 配置][api-management-git-configure]

当您使用发布者门户，PowerShell 的 cmdlet 或 REST API 的服务进行了更改时，您所管理服务配置数据库使用`https://{name}.management.azure-api.net`终结点，在该关系图的右侧所示。 图的左侧显示了如何管理您的服务配置使用 Git，Git 存储库服务位于`https://{name}.scm.azure-api.net`。

以下步骤概述了管理 API 管理服务实例使用 Git。

1.  Git 中启用访问您的服务
2.  将您的服务配置数据库保存到 Git 存储库
3.  复制到本地计算机上 Git repo
4.  拉到您本地计算机上，最新的 repo 并提交，并强制更改发回您 repo
5.  将您 repo 所做的更改部署到您的服务配置数据库

本文介绍如何启用和使用 Git 来管理您的服务配置，并提供有关的文件和文件夹 Git 存储库中的参考。

## <a name="to-enable-git-access"></a>若要启用 Git 访问

通过右上角的出版商门户查看的 Git 图标，您可以快速查看 Git 配置的状态。 在此示例中，Git 访问但尚未启用。

![Git 状态][api-management-git-icon-enable]

查看和配置 Git 的配置设置，可以单击 Git 图标，或者可以单击**安全**菜单，然后导航到**配置存储库**选项卡。

![启用 GIT][api-management-enable-git]

要启用访问 Git， **Git 允许访问**复选

更改保存在一段时间后，会显示一条确认消息。 请注意，Git 图标已更改为颜色来指示启用了 Git 访问和状态消息立即指示的某些更改未保存到存储库。 这是因为 API 管理服务配置数据库包含尚未保存到存储库中。

![Git 启用][api-management-git-enabled]

>[AZURE.IMPORTANT] 属性将存储在存储库中，直到您将保留其历史记录中未定义任何机密禁用并重新启用 Git 访问。 属性提供了安全的地方来管理常量字符串值，所以不需要将它们存储在策略语句直接跨所有 API 配置和策略，包括秘密。 有关详细信息，请参阅[如何使用 Azure API 管理策略中的属性](api-management-howto-properties.md)。

启用或禁用使用 REST API，Git 访问的信息，请参阅[启用或禁用使用 REST API，Git 访问](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit)。

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>将服务配置保存到 Git 存储库

之前克隆存储库中的第一步是将服务配置的当前状态保存到存储库中。 单击**保存配置到存储库**。

![保存配置][api-management-save-configuration]

在确认屏幕上进行所需的更改，请单击**确定**以保存。

![保存配置][api-management-save-configuration-confirm]

几分钟后保存配置，并显示存储库的配置状态，包括日期和时间以及上一次的配置更改服务配置和存储库之间的最后一次同步。

![配置状态][api-management-configuration-status]

一旦配置保存到存储库，它可以进行克隆。

有关执行此操作，使用 REST API 的信息，请参阅[提交配置快照使用 REST API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot)。

## <a name="to-clone-the-repository-to-your-local-machine"></a>若要复制到本地计算机上的资源库

要克隆存储库，您需要为您的存储库、 用户名和密码 URL。 用户名和 URL 将显示**配置存储库**选项卡的顶部附近。

![Git 克隆][api-management-configuration-git-clone]

密码生成的**配置存储库**选项卡底部。

![生成密码][api-management-generate-password]

要生成一个密码，请首先确保**到期**设置为所需的到期日期和时间，，然后单击**生成令牌**。

![密码][api-management-password]

>[AZURE.IMPORTANT] 记下此密码。 一旦您离开此页面将不会再显示密码。

下面的示例使用[Git 的 Windows](http://www.git-scm.com/downloads)中的 Git Bash 工具，但您可以使用任何您熟悉的 Git 工具。

在所需的文件夹中打开 Git 工具并运行以下命令来复制到本地计算机上，使用由出版商门户提供命令的 git 存储库。

    git clone https://bugbashdev4.scm.azure-api.net/ 

提供的用户名和密码提示时。

如果您收到任何错误，请尝试修改您`git clone`包括用户名称和密码，如下面的示例中所示的命令。

    git clone https://username:password@bugbashdev4.scm.azure-api.net/

如果这提供了一个错误，请尝试 URL 编码命令的密码部分。 一个快速的方法就是打开 Visual Studio，并在**立即窗口**中执行以下命令。 若要打开**立即窗口**，请在 Visual Studio 中打开的任何解决方案或项目 （或创建新的空控制台应用程序），并选择**Windows****即时**从**调试**菜单。

    ?System.NetWebUtility.UrlEncode("password from publisher portal")

使用编码的密码以及用户名称和存储库位置 git 命令构造。

    git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/

一旦克隆存储库可以查看和使用它在您的本地文件系统中。 有关详细信息，请参阅[本地 Git 存储库中的文件和文件夹结构的引用](#file-and-folder-structure-reference-of-local-git-repository)。

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>若要更新您的本地存储库中最新的服务实例配置

如果更改在发布服务器上的门户或使用 REST API，您 API 管理服务实例，您必须向存储库中保存这些更改之前可以使用最新的更改来更新本地资源库。 做到这一点，在出版商门户中，**配置存储库**选项卡上单击**保存到存储库的配置**，然后发出以下命令在本地资源库中。

    git pull

在运行前`git pull`确保您在该文件夹中的本地资源库。 如果您刚完成`git clone`命令，则您必须通过运行下面的命令将目录更改为您 repo。

    cd bugbashdev4.scm.azure-api.net/

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>若要将更改从您的本地 repo 推到服务器 repo

若要将更改从您的本地存储库中推送到服务器存储库，必须提交更改，并随后将它们推送到服务器存储库。 提交您的更改，请打开您的 Git 命令工具、 切换到的目录的本地存储库，以及发出下列命令。

    git add --all
    git commit -m "Description of your changes"

若要强制所有提交到服务器，请运行以下命令。

    git push

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>若要部署到的 API 管理服务实例的服务配置的任何更改

本地更改被提交并被推送到服务器存储库后，可以将它们部署到您的 API 管理服务实例。

![部署][api-management-configuration-deploy]

有关执行此操作，使用 REST API 的信息，请参阅[部署 Git 使用 REST API 的配置数据库更改](https://msdn.microsoft.com/library/dn781420.aspx#DeployChanges)。

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>文件和文件夹结构参照本地 Git 存储库

文件和文件夹的本地 git 存储库中包含该服务实例的配置信息。

| 项目                       | 说明                                                                                |
|-------------------------   |--------------------------------------------------------------------------------------------|
| 根 api 管理文件夹 | 包含顶级配置的服务实例                                  |
| api 文件夹                | 包含配置的服务实例中的 api                            |
| 组文件夹              | 包含该服务实例中的组的配置                          |
| 策略文件夹            | 包含该服务实例中的策略                                              |
| portalStyles 文件夹        | 包含开发人员门户中的自定义服务实例的配置 |
| 产品文件夹            | 包含在服务实例的产品配置                        |
| 模板文件夹           | 包含该服务实例中的电子邮件模板的配置                 |

每个文件夹可以包含一个或多个文件，并在某些情况下的一个或多个文件夹，例如每个 API、 产品或组的文件夹。 每个文件夹中的文件是特定文件夹名称所描述的实体类型。

| 文件类型 | 目的                                                                |
|-----------|------------------------------------------------------------------------|
| json      | 配置各自的实体信息                  |
| html      | 关于实体，通常开发人员门户中显示说明 |
| xml       | 策略声明                                                      |
| css       | 开发人员门户的自定义样式表                        |

可以创建、 删除、 编辑和管理在本地文件系统上，这些文件和部署更改发回您的 API 管理服务实例。

>[AZURE.NOTE] 以下实体 Git 存储库中不包含和不能被配置为使用 Git。
>
>-    用户
>-    订阅
>-    属性
>-    开发人员门户实体以外的其他样式

### <a name="root-api-management-folder"></a>根 api 管理文件夹

根`api-management`文件夹中包含`configuration.json`文件，其中包含顶级信息按以下格式的服务实例。

    {
      "settings": {
        "RegistrationEnabled": "True",
        "UserRegistrationTerms": null,
        "UserRegistrationTermsEnabled": "False",
        "UserRegistrationTermsConsentRequired": "False",
        "DelegationEnabled": "False",
        "DelegationUrl": "",
        "DelegatedSubscriptionEnabled": "False",
        "DelegationValidationKey": ""
      },
      "$ref-policy": "api-management/policies/global.xml"
    }

前四个设置 (`RegistrationEnabled`， `UserRegistrationTerms`， `UserRegistrationTermsEnabled`，和`UserRegistrationTermsConsentRequired`) 映射到**安全性**部分中的**标识**选项卡上的下列设置。

| 标识设置                     | 映射到                                               |
|--------------------------------------|-------------------------------------------------------|
| RegistrationEnabled                  | **将重定向到登录页的匿名用户**的复选框 |
| UserRegistrationTerms                | **在用户注册使用规定的**文本框               |
| UserRegistrationTermsEnabled         | **显示在登录页上使用的条款**复选框         |
| UserRegistrationTermsConsentRequired | **需要同意**复选框                          |

![标识设置][api-management-identity-settings]

接下来四个设置 (`DelegationEnabled`， `DelegationUrl`， `DelegatedSubscriptionEnabled`，和`DelegationValidationKey`) 映射到**安全性**部分中的**委派**选项卡上的下列设置。

| 委派设置           | 映射到                                    |
|------------------------------|--------------------------------------------|
| DelegationEnabled            | **登录和注册代理**复选框    |
| DelegationUrl                | **委派端点 URL**文本框        |
| DelegatedSubscriptionEnabled | **代理产品订购**复选框 |
| DelegationValidationKey      | **代理验证密钥**文本框        |

![委派设置][api-management-delegation-settings]

最终的设置中， `$ref-policy`，映射到服务实例的全局策略语句文件。

### <a name="apis-folder"></a>api 文件夹

`apis`文件夹包含一个文件夹，其中包含以下各项的服务实例中每个 api。

-   `apis\<api name>\configuration.json`-这是 API 的配置，其中包含有关后端服务 URL 和操作的信息。 这是相同的信息，如果您要调用以[获取特定的 API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI)将返回`export=true`在`application/json`格式。
-   `apis\<api name>\api.description.html`-这是 API 的说明，并且对应于`description` [API 实体](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties)的属性。
-   `apis\<api name>\operations\`-此文件夹包含`<operation name>.description.html`映射到 API 中的操作的文件。 每个文件包含在 API 中映射到单个操作的描述`description`REST API 中的[操作实体](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties)的属性。

### <a name="groups-folder"></a>组文件夹

`groups`文件夹包含有关每个服务实例中定义的组的文件夹。

-   `groups\<group name>\configuration.json`-这是一组配置。 这是如果您要调用[获取一组特定](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup)的操作将返回相同的信息。
-   `groups\<group name>\description.html`-这是组的描述，并与`description`[组实体](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties)的属性。

### <a name="policies-folder"></a>策略文件夹

`policies`文件夹中包含的策略语句的服务实例。

-   `policies\global.xml`-包含在服务实例的全局范围内定义的策略。
-   `policies\apis\<api name>\`-如果您有任何在 API 范围内定义的策略，它们包含在该文件夹中。
-   `policies\apis\<api name>\<operation name>\`文件夹-如果您有任何在操作范围内定义的策略，它们都在此文件夹中包含`<operation name>.xml`映射到每个操作的策略语句的文件。
-   `policies\products\`-如果您有任何在产品范围内定义的策略，它们包含在该文件夹中，其中包含`<product name>.xml`映射到每个产品的策略语句的文件。

### <a name="portalstyles-folder"></a>portalStyles 文件夹

`portalStyles`文件夹包含开发人员门户的自定义项的服务实例的配置和样式表。

-   `portalStyles\configuration.json`-包含开发人员门户所使用的样式表的名称
-   `portalStyles\<style name>.css`-每个`<style name>.css`文件包含样式的开发人员门户 (`Preview.css` ， `Production.css` ，默认情况下)。

### <a name="products-folder"></a>产品文件夹

`products`文件夹包含文件夹，用于在服务实例中定义的每个产品。

-   `products\<product name>\configuration.json`-这是该产品的配置。 这是如果您要调用[获取特定产品](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct)的操作将返回相同的信息。
-   `products\<product name>\product.description.html`-这是产品的说明，并与`description`的 REST API 中的[产品实体](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product)的属性。

### <a name="templates"></a>模板

`templates`文件夹中包含的[电子邮件模板](api-management-howto-configure-notifications.md)的服务实例的配置。

-   `<template name>\configuration.json`-这是该电子邮件模板的配置。
-   `<template name>\body.html`-这是该电子邮件模板的正文。

## <a name="next-steps"></a>下一步行动

其他方法来管理您的服务实例的信息，请参阅︰

-   管理您使用以下 PowerShell cmdlet 的服务实例
    -   [服务部署 PowerShell cmdlet 引用](https://msdn.microsoft.com/library/azure/mt619282.aspx)
    -   [服务管理 PowerShell cmdlet 引用](https://msdn.microsoft.com/library/azure/mt613507.aspx)
-   管理您在 publisher 门户的服务实例
    -   [管理您的第一个 API](api-management-get-started.md)
-   管理您使用 REST API 的服务实例
    -   [API 管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>观看视频概述

> [AZURE.VIDEO configuration-over-git]

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




