<properties 
    pageTitle="演练︰ 监视 Microsoft Dynamics CRM 应用程序的见解与" 
    description="遥测得到 Microsoft Dynamics CRM Online 使用应用程序的见解。 获取数据、 可视化和导出的设置、 演练。" 
    services="application-insights" 
    documentationCenter=""
    authors="mazharmicrosoft" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="awills"/>
 
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>演练︰ 实现遥测的 Microsoft Dynamics CRM Online 使用应用程序的见解

本文演示如何从[Microsoft Dynamics CRM Online](https://www.dynamics.com/)获取遥测数据使用[Visual Studio 应用程序理解](https://azure.microsoft.com/services/application-insights/)。 我们将引导您完成将应用程序的见解脚本添加到您的应用程序的完整过程捕获数据，以及数据可视化。

>[AZURE.NOTE] [浏览示例解决方案](https://dynamicsandappinsights.codeplex.com/)。

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>向新的或现有 CRM Online 的实例中添加应用程序的见解 

要监视您的应用程序，您添加到您的应用程序应用程序深入 SDK。 SDK 将遥测发送到[应用程序的见解门户](https://portal.azure.com)，您可以使用我们的功能强大的分析和诊断工具，或将数据导出到存储。

### <a name="create-an-application-insights-resource-in-azure"></a>在 Azure 中创建的应用程序理解资源

1. 获取[在 Microsoft Azure 中的帐户](http://azure.com/pricing)。 
2. 登录到[Azure 门户](https://portal.azure.com)并添加新的应用程序理解资源。 这是将处理和显示数据。

    ![单击 +，开发商服务，应用程序的见解。](./media/app-insights-sample-mscrm/01.png)

    作为应用程序类型，选择 ASP.NET。

3. 打开快速入门选项卡并打开代码脚本。

    ![](./media/app-insights-sample-mscrm/03.png)

在另一个浏览器窗口中**保持打开状态的代码页**时执行下一步步骤。 您很快就需要该代码。 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>在 Microsoft Dynamics CRM 中创建一个 JavaScript web 资源

1. 使用管理员权限打开您的 CRM Online 的实例和登录。
2. 打开 Microsoft Dynamics CRM 的设置，自定义，自定义系统

    ![](./media/app-insights-sample-mscrm/04.png)
    
    ![](./media/app-insights-sample-mscrm/05.png)


    ![](./media/app-insights-sample-mscrm/06.png)

3. 创建一个 JavaScript 资源。

    ![](./media/app-insights-sample-mscrm/07.png)

    为它指定一个名称，选择**脚本 (JScript)**然后打开文本编辑器。

    ![](./media/app-insights-sample-mscrm/08.png)
    
4. 从应用程序的见解中复制代码。 在复制时一定要忽略脚本标记。 下面的屏幕快照，请︰

    ![](./media/app-insights-sample-mscrm/09.png)

    代码包含标识您的应用程序理解资源的检测项。

5. 保存并发布。

    ![](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>检测窗体

1. 在 Microsoft CRM 联机中打开客户表单

    ![](./media/app-insights-sample-mscrm/11.png)

2. 打开窗体属性

    ![](./media/app-insights-sample-mscrm/12.png)

3. 添加创建 JavaScript web 资源

    ![](./media/app-insights-sample-mscrm/13.png)

    ![](./media/app-insights-sample-mscrm/14.png)

4. 保存并发布窗体自定义项。


## <a name="metrics-captured"></a>捕获的指标

现在已经设置为窗体的遥测捕获。 每当使用它时，数据将发送到您的应用程序理解资源。

下面是示例，您将看到的数据。

#### <a name="application-health"></a>应用程序运行状况

![](./media/app-insights-sample-mscrm/15.png)

![](./media/app-insights-sample-mscrm/16.png)

浏览器异常︰

![](./media/app-insights-sample-mscrm/17.png)

单击该图表以获取更多详细信息︰

![](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>使用

![](./media/app-insights-sample-mscrm/19.png)

![](./media/app-insights-sample-mscrm/20.png)

![](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>浏览器

![](./media/app-insights-sample-mscrm/22.png)

![](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>地理位置

![](./media/app-insights-sample-mscrm/24.png)

![](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>内部页查看请求

![](./media/app-insights-sample-mscrm/26.png)

![](./media/app-insights-sample-mscrm/27.png)

![](./media/app-insights-sample-mscrm/28.png)

![](./media/app-insights-sample-mscrm/29.png)

![](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>示例代码

[浏览代码示例](https://dynamicsandappinsights.codeplex.com/)。

## <a name="power-bi"></a>双电源

可以执行甚至更深层次的分析，如果您[将数据导出到 Microsoft 电源 BI](app-insights-export-power-bi.md)。

## <a name="sample-microsoft-dynamics-crm-solution"></a>示例 Microsoft Dynamics CRM 解决方案

[以下是示例解决方案在 Microsoft Dynamics CRM 的实施](https://dynamicsandappinsights.codeplex.com/)。

## <a name="learn-more"></a>了解更多信息

* [应用程序的见解是什么？](app-insights-overview.md)
* [应用程序的 web 页的见解](app-insights-javascript.md)
* [更多示例和演练](app-insights-code-samples.md)

 
