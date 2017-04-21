<properties
    pageTitle="开始使用 Web 应用程序的生产环境中测试"
    description="了解生产 （提示） 功能在 Azure 应用程序服务 Web 应用程序中的测试。"
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
    ms.date="01/13/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-test-in-production-for-web-apps"></a>开始使用 Web 应用程序的生产环境中测试

在生产环境中，测试或实时测试您的 web 应用程序使用客户的实际流量是应用程序开发人员越来越多地集成到他们的[敏捷开发](https://en.wikipedia.org/wiki/Agile_software_development)方法的测试策略。 它使您能够在生产环境中，而不是合成数据的测试环境中测试您的应用程序与实际用户的通信质量。 通过公开给真正的用户您新的应用程序，可以通知部署后，您的应用程序可能会遇到的实际问题。 您可以验证的功能、 性能和数量、 速度、 和各种真实的用户通信，您可以从未接近在测试环境中对您的应用程序更新的值。

## <a name="traffic-routing-in-app-service-web-apps"></a>应用程序服务 Web 应用程序中的路由选择的流量

使用[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)中的通信路由功能，可以直接到一个或多个[部署的插槽](web-sites-staged-publishing.md)，实时用户通信的一部分，然后分析[Azure 应用程序见解](/services/application-insights/)或[Azure HDInsight](/services/hdinsight/)或如[新 Relic](/marketplace/partners/newrelic/newrelic/)的第三方工具来验证所做的更改与您的应用程序。 例如，可以与应用程序服务实现以下方案︰

- 发现功能 bug 或查明在您整个站点部署之前的更新中的性能瓶颈
- 通过测量测试应用程序的 usibility 指标执行"受控的测试航班"的所做的更改
- 最新的更新，逐步量产和正常下来到当前版本出错时 
- 优化您的应用程序的业务成果通过运行[A / B 测试](https://en.wikipedia.org/wiki/A/B_testing)或[多变量测试](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing)的多个部署插槽中

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>在 Web 应用程序中使用通信路由的要求

- 根据需要为多个部署插槽，您的 web 应用程序必须运行在**标准**或**特优**层。
- 要正常工作，通信路由需要在用户的浏览器中启用 cookie。 通信路由使用 cookie 来锁定客户端会话的客户端浏览器的整个生命周期部署插槽。
- 高级提示方案通过 Azure PowerShell cmdlet 的流量路由支持。

## <a name="route-traffic-segment-to-a-deployment-slot"></a>对部署槽路由通信段

基本级别的每个提示方案中，您将预定义的实时流量百分比路由到非生产部署插槽。 若要执行此操作，请按照下面的步骤操作︰

>[AZURE.NOTE] 此处的步骤假定您已经有一个[非生产部署插槽](web-sites-staged-publishing.md)和所需的 web 应用程序内容已向其[部署](web-sites-deploy.md)。

1. 登录到[Azure 的门户](https://portal.azure.com/)。
2. 在 web 应用程序的刀片式服务器，请单击**设置** > **通信路由**。
  ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. 选择要传送到通讯和您的期望，然后单击**保存**，总的通信量的百分比的插槽。

    ![](./media/app-service-web-test-in-production/02-select-slot.png)

4. 转到此部署槽的刀片式服务器。 您现在应该看到实时通信被发送到该文件。

    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

一旦通信路由配置，客户端指定的百分比将随机路由到非生产插槽。 但是，务必请注意一旦客户端会自动路由到特定的插槽中，它将"固定，"该槽为该客户端会话的生存期。 此操作完成后使用 cookie 来锁定该用户会话。 如果您检查 HTTP 请求时，您会发现`TipMix`在每个后续请求中的 cookie。

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a>强制客户端请求转换为特定的插槽

除了自动通信路由，应用程序服务将请求路由到特定的插槽。 当您希望用户能够选择到或退出测试应用程序时，这非常有用。 若要执行此操作，请使用`x-ms-routing-name`查询参数。

重新路由到特定的插槽使用用户`x-ms-routing-name`，您必须确保插槽已加入通信路由列表。 由于您想要显式路由到一个插槽，您设置的实际路由百分比并不重要。 如果需要，您可以手工创建"测试链接"，用户可以单击来访问测试应用程序。

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>选择测试应用程序用户

若要使用户可以选择取消您的试用版应用程序，例如，可以将放此链接在 web 页中︰

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>

该字符串`x-ms-routing-name=self`指定生产插槽。 一旦客户端浏览器访问该链接，将它重定向到生产插槽中，不仅每个后续请求将包含`x-ms-routing-name=self`针生产插槽会话的 cookie。

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a>自愿到试用版应用程序中的用户

若要使用户可以选择使用您的试用版应用程序，例如设置为非生产插槽中，名称相同的查询参数︰

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>更多的资源 ##

-   [设置临时在 Azure 应用程序服务 web 应用程序的环境](web-sites-staged-publishing.md)
-   [预见在 Azure 中复杂的应用程序部署](app-service-deploy-complex-application-predictably.md)
-   [敏捷软件开发与 Azure 应用程序服务](app-service-agile-software-development.md)
-   [对于 web 应用程序有效地使用 DevOps 环境](app-service-web-staged-publishing-realworld-scenarios.md)