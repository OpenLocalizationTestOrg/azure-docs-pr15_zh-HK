<properties
    pageTitle="Flighting （beta 测试） 在 Azure 应用程序服务的部署"
    description="了解如何飞行在您的应用程序中的新功能或试用版测试您在本教程中的端到端的更新。 它汇集等连续发布、 槽、 通信路由，和见解应用程序集成的应用程序服务功能。"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/02/2016"
    ms.author="cephalin"/>
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Flighting （beta 测试） 在 Azure 应用程序服务的部署

本教程展示了如何进行*flighting 的部署*通过集成[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)和[Azure 应用程序建议](/services/application-insights/)的各种功能。 

*Flighting*是一种验证新功能或更改与有限数量的实际的客户部署过程，主要测试在生产方案中。 它像是 beta 测试，并且有时也称为"受控的测试飞行"。 许多大企业利用 web 内容使用此方法来获取早期验证其应用程序中的更新他们的[敏捷开发](https://en.wikipedia.org/wiki/Agile_software_development)实践。 Azure 应用程序服务使您能够在生产环境中的测试相结合连续发布和应用程序以实现同一 DevOps 方案的见解。 这种方法的优点包括︰

- 之前释放，**获得真实的反馈_之前_更新程序发布到生产环境**的唯一胜过获得反馈，一旦释放获得反馈。 您可以尽早在产品生命周期中所需测试更新行为和真实的用户通信。
- **提高[连续测试驱动开发 (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development) ** -通过并在生产环境中的测试集成到持续集成和应用程序的见解，具有检测用户验证发生早期和自动在产品生命周期。 这有助于减少在手动测试执行的时间投资。
- **优化测试工作流**— 通过自动化测试的连续监视设备，生产中可以有可能完成各种单一过程，比如[集成](https://en.wikipedia.org/wiki/Integration_testing)、[回归](https://en.wikipedia.org/wiki/Regression_testing)、[可用性](https://en.wikipedia.org/wiki/Usability_testing)、 可访问性、 本地化、[性能](https://en.wikipedia.org/wiki/Software_performance_testing)、[安全](https://en.wikipedia.org/wiki/Security_testing)、 和[验收](https://en.wikipedia.org/wiki/Acceptance_testing)测试的目标。

Flighting 部署不只传送实时通信。 在这样的部署中您想要尽可能快地获得的见解，无论是意外的错误，性能下降，用户体验问题。 请记住，您需要处理的真正的客户。 因此呢，您必须确保您已设置 flighting 部署来做出明智的决定，为下一步的操作所需的所有数据都收集。 本教程展示如何收集数据与应用程序的见解，但您可以使用新的 Relic 或其他技术适合您的方案。 

## <a name="what-you-will-do"></a>您将做什么

在本教程中，您将学习如何使以下情况在一起以在生产环境中测试您的应用程序服务的应用程序︰

- [生产通信路由](app-service-web-test-in-production-get-start.md)到您的测试应用程序
- [检测应用程序](../application-insights/app-insights-web-track-usage.md)以获得有用的度量标准
- 部署您的试用版应用程序不间断地跟踪实时应用程序指标
- 将生产应用程序和测试应用程序以查看代码更改如何转换为结果之间的标准比较

## <a name="what-you-will-need"></a>您将需要

-   Azure 帐户
-   [GitHub](https://github.com/)帐户
- Visual Studio 2015-您可以下载[社区版](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx)。
-   Git 外壳 （安装使用[GitHub 的 Windows](https://windows.github.com/)）-这使您可以在同一个会话中运行的 Git 和 PowerShell 命令
-   最新的[Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi)位
-   下列情况的基本了解︰
    -   [Azure 资源管理器](../azure-resource-manager/resource-group-overview.md)模板部署 （请参见[部署预知在 Azure 中复杂的应用程序](app-service-deploy-complex-application-predictably.md)）
    -   [Git](http://git-scm.com/documentation)
    -   [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Azure 帐户来完成本教程，您需要︰
> + 您可以[免费开设 Azure 帐户](/pricing/free-trial/)-获取积分可用于试验 Azure 服务付费，和甚至用完后可以保留该帐户并使用自由 Azure 服务，例如 Web 应用程序。
> + 您可以[激活 Visual Studio 订户权益](/pricing/member-offers/msdn-benefits-details/)-您的 Visual Studio 预订为您提供了积分可用于付费的 Azure 服务每月。
>
> 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## <a name="set-up-your-production-web-app"></a>生产 web 应用程序设置

>[AZURE.NOTE] 在本教程中使用的脚本将自动配置连续发布从 GitHub 资料库。 这就要求，已在 Azure 存储 GitHub 凭据，否则脚本的部署将失败时尝试配置 web 应用程序的源代码管理设置。
>
>在 Azure 存储您 GitHub 的凭据，请[Azure 门户](https://portal.azure.com/)和[GitHub 部署配置](app-service-continuous-deployment.md#Step7)中创建 web 应用程序。 您只需执行一次。

在 DevOps 典型方案中，您必须运行在 Azure，实时的应用程序和您要对其进行更改，通过连续发布。 在此方案中，您将部署到生产环境的模板，您必须开发和测试。

1.  创建您自己分叉的[ToDoApp](https://github.com/azure-appservice-samples/ToDoApp)存储库。 创建您分叉的信息，请参阅[Repo 分叉](https://help.github.com/articles/fork-a-repo/)。 您分叉创建后，您可以在浏览器中看到它。

    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  打开 Git Shell 会话。 如果您没有 Git 外壳，请立即安装[GitHub 的 Windows](https://windows.github.com/) 。
3.  通过执行以下命令来创建您分叉的本地克隆︰

        git clone https://github.com/<your_fork>/ToDoApp.git

4.  一旦您本地克隆，定位到*&lt;repository_root >*\ARMTemplates，然后运行 deploy.ps1 脚本的唯一的后缀，如下所示︰

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix>

4.  出现提示时，键入所需的用户名和密码访问数据库。 因为您将需要指定它们再次更新该资源组时，请记住您数据库的凭据。

    您应该看到 Azure 的各种资源的调配进度。 当部署完成时，该脚本将启动该应用程序在浏览器中并为您提供友好的提示音。
    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)

6.  返回在 Git Shell 会话，运行︰

        .\swap –Name ToDoApp<your_suffix>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)

7.  当脚本完成后时，要返回到浏览前端的地址 (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) 若要查看在生产环境中运行的应用程序。
5.  登录到[Azure 门户](https://portal.azure.com/)并看一看创建的内容。

    您应该能够看到在同一个资源组中，具有两个 web 应用程序`Api`的名称后缀。 如果您看一下资源组视图中，您将看到 SQL 数据库和服务器、 应用程序服务计划中和用于 web 应用程序的临时插槽。 浏览不同的资源，并比较它们与*&lt;repository_root >*\ARMTemplates\ProdAndStage.json 来查看他们在模板中的配置方式。

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

已经设置的生产应用程序。  现在，让我们想象一下，您会收到反馈信息可用性较差的应用程序。 因此，您决定进行调查。 您会检测您的应用程序以便为您提供反馈。

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>调查︰ 检测您的客户端应用程序监控指标

5. 打开*&lt;repository_root >*\src\MultiChannelToDo.sln 在 Visual Studio 中的。
6. 通过右击解决方案还原所有 Nuget 程序包 >**解决方案管理 NuGet 程序包** > **都还原**。
6. 用鼠标右键单击**MultiChannelToDo.Web** > **添加应用程序的见解遥测** > **配置设置**> 更改资源组的 ToDoApp*&lt;your_suffix >* > **向项目中添加应用程序见解**。
7. 在 Azure 门户中，打开刀片式服务器的**MultiChannelToDo.Web**应用程序分析资源。 然后在**应用程序运行状况**部分中，单击**了解如何收集浏览器页面加载数据**> 复制代码。
7. 添加到复制的 JS 检测代码*&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml，只需在关闭之前`<heading>`标记。 它应该包含您的应用程序分析资源的唯一检测键。

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>

11. 自定义事件向发送应用程序的见解鼠标点击通过将下面的代码添加到主体的底部︰

        <script>
            $(document.body).find("*").click(function(event) {

                appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
            });
        </script>

    此 JavaScript 代码段将自定义事件发送到应用程序的见解，每次用户单击 web 应用程序中的任意位置。

12. 在 Git 外壳程序中，提交并将更改推送到您在 GitHub 的分叉。 然后，等待客户端刷新浏览器。

        git add -A :/
        git commit -m "add AI configuration for client app"
        git push origin master

6.  换到生产部署的应用程序更改︰

        .\swap –Name ToDoApp<your_suffix>

13. 浏览到您配置的应用程序理解资源。 单击自定义事件。

    ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

    如果您看不到度量数据的自定义事件，等待几分钟，单击**刷新**。

假设您将看到图表类似下面内容︰

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

和它下面的事件网格︰

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

根据 ToDoApp 应用程序代码中，对应于提交按钮的**按钮**事件和对应于该文本框的**输入**事件。 到目前为止，事情有意义。 但是，它看起来没有大量点击，极少的点击上待办事项 （ **LI**事件）。

基于这一点，则窗体您某些用户的假设相混淆用户界面的哪个部分是可单击的因为它停留在列表项和它们的图标上时将光标的样式设置为所选文本。

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

这可能是一个刻意的例子。 然而，要使改进您的应用程序，然后再执行 flighting 部署以获得实时客户可用性反馈。

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>检测您的服务器应用程序监控指标
这是相切的因为在本教程中演示的方案仅用于处理客户端应用程序。 但是，出于完整性的考虑您将设置服务器端应用程序。

6. 用鼠标右键单击**MultiChannelToDo** > **添加应用程序的见解遥测** > **配置设置**> 更改资源组的 ToDoApp*&lt;your_suffix >* > **向项目中添加应用程序见解**。
12. 在 Git 外壳程序中，提交并将更改推送到您在 GitHub 的分叉。 然后，等待客户端刷新浏览器。

        git add -A :/
        git commit -m "add AI configuration for server app"
        git push origin master

6.  换到生产部署的应用程序更改︰

        .\swap –Name ToDoApp<your_suffix>

就是这样！

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a>调查︰ 将特定插槽的标签添加到您的客户端应用程序指标

在本部分中，您将配置发送到相同的应用程序理解资源特定插槽的遥测的部署不同插槽。 这种方式，可以比较不同的插槽 （部署环境） 中的流量之间的遥测数据可以方便地查看您的应用程序更改的效果。 在同一时间，可以将生产通讯的其余部分使您可以继续监视您的生产应用程序根据需要。

由于您正在收集数据的客户端行为，您将 index.cshtml[添加到您的 JavaScript 代码的遥测初始值设定项](../application-insights/app-insights-api-custom-events-metrics.md#js-initializer)。 如果您想要测试服务器端的性能，例如，您还可以执行同样在服务器代码中 （请参见[自定义事件和指标应用见解 API](../application-insights/app-insights-api-custom-events-metrics.md)。

1. 首先，添加代码 bewteen 两个`//`注释下面在 JavaScript 中的阻止您添加到`<heading>`前面添加标签。

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    此初始值设定项的代码将导致`appInsights`要添加对象的自定义属性调用`Environment`遥测发送给每个。

2. 接下来，添加此自定义属性作为 web 应用程序在 Azure 中的[槽设置](web-sites-staged-publishing.md#AboutConfiguration)。 若要执行此操作，请在 Git Shell 会话中运行以下命令。

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    Web.config 中您的项目已定义`environment`应用程序设置。 使用此设置，当您测试应用程序本地度量标准将标记与`VS Debugger`。 但是，当将更改推到 Azure 时，Azure 将查找和使用`environment`将使用标记相反，在 web 应用程序的配置中设置的应用程序和您的度量标准`Production`。

3. 提交和推 GitHub，代码更改您分叉，然后等待用户使用新的应用程序 （需要刷新浏览器）。 它大约需要 15 分钟才能显示在您的应用程序理解的新属性`MultiChannelToDo.Web`资源。

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master

4. 现在，再次转到**自定义事件**刀片式服务器并筛选指标`Environment=Production`。 您现在应该能够看到与此筛选器的生产插槽中的所有新自定义事件。

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)

5. 单击**收藏夹**按钮以保存当前测量数据资源管理器设置为类似于**自定义事件︰ 生产**。 您可以轻松地此视图和视图之间切换部署插槽后面。

    > [AZURE.TIP] 更具说服力的分析报告，请考虑[集成与电源 BI 应用程序理解资源](../application-insights/app-insights-export-power-bi.md)。

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a>将插槽特定的标记添加到您的服务器应用程序指标
同样，出于完整性的考虑您将设置服务器端应用程序。 与不同的客户端应用程序的检测在 JavaScript 中，服务器应用程序的特定插槽的标记检测与.NET 代码。

1. 打开*&lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs。 添加代码块下面，只是在关闭之前命名空间大括号。

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }

2. 通过添加更正名称解析错误`using`语句下面到开头的文件︰

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;

3. 将下面的代码添加到开始处`Application_Start()`方法︰

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());

3. 提交和推 GitHub，代码更改您分叉，然后等待用户使用新的应用程序 （需要刷新浏览器）。 它大约需要 15 分钟才能显示在您的应用程序理解的新属性`MultiChannelToDo`资源。

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>更新︰ 设置测试分支

2. 打开*&lt;repository_root >*\ARMTemplates\ProdAndStagetest.json 和查找`appsettings`资源 (搜索`"name": "appsettings"`)。 有这些问题，另一个用于每个插槽 4。 

2. 每个`appsettings`资源，添加`"environment": "[parameters('slotName')]"`应用程序设置到末尾的`properties`数组。 不要忘记结束前的一行用逗号分隔。

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)
    
    您只需添加`environment`为模板中的所有插槽的应用程序设置。
    
2. 在同一文件中，发现`slotconfignames`资源 (搜索`"name": "slotconfignames"`)。 有 2 个，另一个用于每个应用程序。

2. 每个`slotconfignames`资源，添加`"environment"`到`appSettingNames`数组。 不要忘记结束前的一行用逗号分隔。

    您刚刚完成`environment`应用程序设置这两个应用程序为其各自部署插槽杆。  

3. 在 Git Shell 会话中，与以前使用的同一资源组后缀中运行下面的命令。

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta

4. 出现提示时，指定相同 SQL 数据库的凭据之前。 然后，当需要更新的资源组，键入`Y`，然后`ENTER`。

    脚本完成后，将保留原始资源组中的所有资源，但一个新槽，名为"测试版"会创建在其中使用相同的配置中开头创建的"转移"槽。

    >[AZURE.NOTE] 此方法创建不同的部署环境是不同的从[Azure 应用程序服务与敏捷软件开发](app-service-agile-software-development.md)的方法。 在这里，可以使用部署插槽，其中一样那里您创建部署环境资源组创建部署环境。 管理部署环境资源组使您能够保持生产环境 off-limits 的开发人员，但并不容易，要不要在生产环境，您可以轻易做到，插槽中进行测试。

如果您愿意，还可以通过运行创建一个 alpha 的应用程序

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

对于本教程，您将只需继续使用您的测试应用程序。

## <a name="update-push-your-updates-to-the-beta-app"></a>更新︰ 将更新推送到测试应用程序

返回到您的应用程序，您希望改进。

1. 请确保您现在处于 beta 分支

        git checkout beta

2. 在*&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml，找到`<li>`标记和添加`style="cursor:pointer"`特性，如下所示。

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)

3. 提交和推到 Azure。

4. 验证的更改会立即反映 beta 插槽中通过导航到 http://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/。 如果您没有更改，则刷新浏览器以获取新的 javascript 代码。

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

现在，您已经运行在测试版的插槽中所做的更改，您就可以执行 flighting 的部署。

## <a name="validate-route-traffic-to-the-beta-app"></a>验证︰ 通信路由到该测试应用程序

在本节中，您将将通信路由到测试应用程序。 为了演示的清晰，您会将路由到该用户通信的重要组成部分。 事实上，要传送的通讯量取决于您的具体情况。 例如，如果您的站点在 microsoft.com 的刻度，然后可能以获得有用的数据需要小于 1%的总通信量。

1. 在 Git Shell 会话中，运行下列命令以将一半生产通信路由到 beta 插槽︰

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

  `ReroutePercentage=50`属性指定 50%的生产通信将被路由到试用版应用程序的 URL (由`ActionHostName`属性)。

2. 现在，请导航到 http://ToDoApp*&lt;your_suffix >*。 azurewebsites.net。 50%的通信流现在应该重定向到 beta 插槽。

3. 在您的应用程序理解资源由环境筛选指标 ="测试版"。

    > [AZURE.NOTE] 如果您将此筛选的视图另存为另一个收藏，然后可以很容易地翻转生产和测试视图之间的度量资源管理器视图。

假设在应用见解可以看到类似于以下内容︰

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

这不只显示上有很多更多的点击`<li>`标记，但似乎那里点击的峰值在`<li>`标记。 然后可以断定人们已经发现的新`<li>`标记是可点击的和现在正在清除应用程序中所有他们以前已完成的任务。

基于 flighting 部署的数据，确定新的用户界面可供生产。

## <a name="go-live-move-your-new-code-into-production"></a>上线︰ 将新代码移动到生产环境

现在您可以将更新移动到生产环境。 重要的是，现在您知道您的更新已经被验证_之前_它推送到生产环境。 因此，现在可以放心地部署它。 到 AngularJS 客户端应用程序中进行更新，因为您仅验证的客户端代码。 如果您要对后端 Web API 应用程序进行更改，您可以同样轻松地验证所做的更改。

1. Git Shell 中运行以下命令删除通信路由规则︰

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()

2. 运行 Git 的命令︰

        git checkout master
        git pull origin master
        git merge beta
        git push origin master

2. 等待几分钟，为新的代码，以将其部署到临时的插槽中，然后启动 http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net，以验证新的更新取暖的暂存槽中。 请记住，分叉的主分支链接到您的应用程序的临时插槽。

3. 现在，交换到生产的转移插槽

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>摘要 ##

Azure 应用程序服务更加方便小型到中型的企业是在生产中，测试其面向客户的应用程序传统上做了大企业。 但愿，本教程已授予您所需的应用程序服务和应用程序的见解 DevOps 世界中使 flighting 的可能的部署和甚至其他生产测试中的方案，将组合在一起的知识。 

## <a name="more-resources"></a>更多的资源 ##

-   [敏捷软件开发与 Azure 应用程序服务](app-service-agile-software-development.md)
-   [设置临时在 Azure 应用程序服务 web 应用程序的环境](web-sites-staged-publishing.md)
-   [预见在 Azure 中复杂的应用程序部署](app-service-deploy-complex-application-predictably.md)
-   [Azure 的资源管理器模板创作](../resource-group-authoring-templates.md)
-   [JSONLint-JSON 验证程序](http://jsonlint.com/)
-   [Git 分支--基本的分支和合并](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [Azure PowerShell](../powershell-install-configure.md)
-   [项目 Kudu Wiki](https://github.com/projectkudu/kudu/wiki)
