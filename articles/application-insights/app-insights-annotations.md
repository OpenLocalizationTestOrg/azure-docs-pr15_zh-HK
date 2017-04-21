<properties
    pageTitle="释放应用程序理解的批注 |Microsoft Azure"
    description="添加部署或生成度量资源管理器中应用程序的见解图表的标记。"
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="awills"/>

# <a name="release-annotations-in-application-insights"></a>释放应用程序的见解中的注释

释放对[测量数据资源管理器](app-insights-metrics-explorer.md)生成新的部署位置的图表显示的批注。 使用母版页可以方便地查看所做的更改是否有任何对您的应用程序性能的影响。 它们可以通过[Visual Studio 团队服务生成系统](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs)，自动创建，也可以[创建它们从 PowerShell](#create-annotations-from-powershell)。

![批注与可见相关服务器的响应时间的示例](./media/app-insights-annotations/00.png)

版本注释是云生成的功能，然后松开 Visual Studio 团队服务的服务。 

## <a name="install-the-annotations-extension-one-time"></a>安装注释扩展 （一次）

为了能够创建版本注释，将需要安装一个可用的许多团队服务扩展在 Visual Studio 的市场。

1. 登录到您的[Visual Studio 团队服务](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online)项目。
2. 在 Visual Studio 的市场上，[获取版本注释扩展名](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations)，并将其添加到您的团队服务帐户。

![在右上团队服务 web 页上，打开市场。 选择视觉团队服务，然后在生成和释放下，选择查看更多。](./media/app-insights-annotations/10.png)

您只需执行一次此 Visual Studio 团队服务帐户。 现在可以为您的帐户中的任何项目配置版本注释。 

## <a name="get-an-api-key-from-application-insights"></a>从应用程序的见解获得 API 密钥

您需要为您想要创建版本注释每个发行版模板执行此操作。


1. 登录到[Microsoft Azure 门户](https://portal.azure.com)并打开监视应用程序的应用程序理解资源。 （或[现在创建一个](app-insights-overview.md)，如果您还没有这样操作）。
2. 打开**API 访问**，并需要一份**应用程序的见解 Id**。

    ![在 portal.azure.com 中，打开您的应用程序理解资源并选择设置。 打开 API 访问。 将复制的应用程序 ID](./media/app-insights-annotations/20.png)

2. 在单独的浏览器窗口中打开 （或创建） 从 Visual Studio 团队服务管理部署发布模板。 

    添加任务，并从菜单中选择该应用程序的见解版本注释任务。

    将粘贴从 API 访问刀片式服务器中复制的**应用程序 Id** 。

    ![在 Visual Studio 的团队服务，打开发布、 选择一个发行版的定义，并选择编辑。 单击添加任务，然后选择应用程序的见解版本注释。 粘贴该应用程序的见解 id。](./media/app-insights-annotations/30.png)

3. 将**APIKey**字段设置为变量`$(ApiKey)`。

4. 在回到 API 访问刀片式服务器，创建一个新的 API 密钥并且采取它的一个副本。

    ![在 Azure 窗口中 API 访问刀片式服务器，请单击创建 API 密钥。 提供的注释、 检查写注释，并单击生成密钥。 复制新的密钥。](./media/app-insights-annotations/40.png)

4. 打开发布模板的配置选项卡。

    创建变量定义为`ApiKey`。

    粘贴到 ApiKey 变量定义的 API 密钥。

    ![在团队服务窗口中，选择配置选项卡并单击添加变量。 将名称设置为 ApiKey，并转化为价值，再粘贴您刚生成的密钥。](./media/app-insights-annotations/50.png)


5. 最后，**保存**版本定义。

## <a name="create-annotations-from-powershell"></a>从 PowerShell 创建注释

您还可以从任何您喜欢 （不使用 VS Team System） 的进程创建批注。 

获取[从 GitHub Powershell 脚本](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)。

像这样使用它︰

    .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }

获得`applicationId`和`apiKey`从您的应用程序理解资源︰ 打开设置、 API 访问和复制应用程序 id。 然后单击创建 API 密钥复制的项。 

## <a name="release-annotations"></a>版本注释

现在，只要使用发布模板部署新版本的批注将发送到应用程序的见解。 注释将出现在测量数据资源管理器中的图表。

单击任何批注标记打开详细信息的发布，包括请求程序，源代码管理分支，释放定义、 环境和更多。


![单击任何版本注释标记。](./media/app-insights-annotations/60.png)
