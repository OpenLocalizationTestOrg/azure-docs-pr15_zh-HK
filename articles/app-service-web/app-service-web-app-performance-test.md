<properties
   pageTitle="测试 Azure 的 web 应用程序的性能 |Microsoft Azure"
   description="运行 Azure 的 web 应用程序的性能测试，以检查您的应用程序如何处理用户负载。 测量响应时间并查找故障可能指示存在问题。"
   services="app-service\web"
   documentationCenter=""
   authors="ecfan"
   manager="douge"
   editor="jimbe"/>

<tags
   ms.service="app-service-web"
   ms.workload="web"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="05/25/2016"
   ms.author="estfan; manasma; ahomer"/>

# <a name="performance-test-your-azure-web-app-under-load"></a>性能测试 Azure 的 web 应用程序在负载下

启动或将更新部署到生产环境之前，请检查您的 web 应用程序的性能。 这种方式，您可以更好地评估您的应用程序准备好发布是否。 更有信心在高峰使用期间或在您下一步市场推广活动，您的应用程序可以处理通信量。

在公共预览时，可以免费在 Azure 门户应用程序的性能测试。
这些测试模拟一段特定的时间在您的应用程序的用户负载，并测量应用程序的响应。 例如，您的测试结果显示您的应用程序对特定数量的用户的响应速度。 这些示例还显示多少请求失败，这可能表示您的应用程序的问题。      

![查找您的 web 应用程序中的性能问题](./media/app-service-web-app-performance-test/azure-np-perf-test-overview.png)

## <a name="before-you-start"></a>在开始之前

* 您将需要[Azure 预订](https://account.windowsazure.com/subscriptions)，如果您还没有。 了解如何您可以[免费开设 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。

* 您将需要一个[Visual Studio 团队服务](https://www.visualstudio.com/products/what-is-visual-studio-online-vs)帐户以保持性能测试历史记录。 设置性能测试时，将自动创建合适的帐户。 或者，您可以创建一个新帐户或使用现有的帐户，如果您是帐户的所有者。 

* 部署您的应用程序在非生产环境中进行测试。 有您的应用程序使用应用程序服务计划不在生产中使用的计划。 这样一来，不会影响任何现有客户或减慢您的应用程序在生产环境中。 

## <a name="set-up-and-run-your-performance-test"></a>设置和运行性能测试

0.  登录到[Azure 的门户](https://portal.azure.com)。 若要使用自己的 Visual Studio 团队服务帐户，请为帐户所有者登录。

0.  转到您的 web 应用程序。

    ![转到浏览所有，Web 应用程序，您的 web 应用程序](./media/app-service-web-app-performance-test/azure-np-web-apps.png)

0.  转到**性能测试**。

    ![请转到工具，性能测试](./media/app-service-web-app-performance-test/azure-np-web-app-details-tools-expanded.png)
 
0. 现在您将链接的[Visual Studio 团队服务](https://www.visualstudio.com/products/what-is-visual-studio-online-vs)帐户以保持性能测试历史记录。

    如果您有要使用的团队服务帐户，则选择该帐户。 如果不这样做，请创建一个新帐户。

    ![选择现有的团队服务帐户或创建一个新帐户](./media/app-service-web-app-performance-test/azure-np-no-vso-account.png)

0.  创建您的性能测试。 设置详细信息并运行测试。 

在测试运行时，您可以监视实时结果。

例如，假设我们有一个应用程序，提供给出优惠，在去年的节日销售。 该事件持续了 15 分钟，并且峰值负载的 100 个并发客户。 我们想要增加一倍的客户数今年。 我们还想提高客户满意度，通过将页面加载时间从 5 秒减少到 2 秒。 因此，我们将测试我们更新应用程序的性能具有 250 名用户为 15 分钟。

我们将在我们的应用程序上模拟负载，通过生成虚拟用户 （客户） 他们在同一时间访问我们的网站。 这将显示我们多少请求失败或响应速度很慢。

  ![创建、 设置和运行性能测试](./media/app-service-web-app-performance-test/azure-np-new-performance-test.png)

   *  自动添加 web 应用程序的默认 URL。 
   您可以更改该 URL 来测试其他页面 （仅 HTTP GET 请求）。

   *  若要模拟本地的条件和减少延迟，用于生成负载选择最接近您的用户的位置。

  这是正在进行测试。 期间的第一分钟，我们的页面将加载比我们希望的要慢。

  ![在使用实时数据进行的性能测试](./media/app-service-web-app-performance-test/azure-np-running-perf-test.png)

  完成测试后，我们学习后的第一分钟更快地加载网页。 这有助于确定我们可能想要开始排除故障。

  ![已完成的性能测试显示的结果，包括失败的请求](./media/app-service-web-app-performance-test/azure-np-perf-test-done.png)

## <a name="test-multiple-urls"></a>测试多个 Url

您还可以运行性能测试包含多个 Url 表示的端到端用户方案通过 Visual Studio Web 测试文件上传。 几种方法可以创建文件的 Visual Studio Web 测试︰

* [捕获的通讯使用 Fiddler 并导出为 Visual Studio Web 测试文件](http://docs.telerik.com/fiddler/Save-And-Load-Traffic/Tasks/VSWebTest)
* [在 Visual Studio 中创建一个负载测试文件](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release)

若要上载和运行 Visual Studio Web 测试文件︰
 
0. 请按照上述步骤以打开**新的性能测试**刀片式服务器。
   在此刀片式服务器，请选择打开**配置测试使用**刀片式服务器的测试使用 CONFIGFURE 选项。  

    ![打开配置负载测试刀片式服务器](./media/app-service-web-app-performance-test/multiple-01-authoring-blade.png)

0. 检查测试类型被设置为**Visual Studio 的 Web 测试**，然后选择 HTTP 存档文件。
    使用"文件夹"图标来打开文件选择器对话框。

    ![上传多个 URL Visual Studio Web 测试文件](./media/app-service-web-app-performance-test/multiple-01-authoring-blade2.png)

    文件已上载后，您可以看到测试 URL 的详细信息部分中的 Url 的列表。
 
0. 指定的用户负载和测试持续时间，然后选择**运行测试**。

    ![选择的用户负载和持续时间](./media/app-service-web-app-performance-test/multiple-01-authoring-blade3.png)

    在测试完成后，您将看到两个窗格中的结果。 左的窗格显示的一系列图表形式的 performnace 信息。

    ![性能结果窗格](./media/app-service-web-app-performance-test/multiple-01a-results.png)

    右窗格中显示列表的请求失败，错误的类型和它出现的次数。

    ![请求失败窗格](./media/app-service-web-app-performance-test/multiple-01b-results.png)

0. 选择顶部的右窗格中的**重新运行**图标，请重新运行测试。

    ![重新运行测试](./media/app-service-web-app-performance-test/multiple-rerun-test.png)

##  <a name="q--a"></a>问与答

#### <a name="q-is-there-a-limit-on-how-long-i-can-run-a-test"></a>问︰ 有没有限制在多长时间，我可以运行测试？ 

**A**︰ 是的您可以在 Azure 门户运行测试最多一个小时的时间。

#### <a name="q-how-much-time-do-i-get-to-run-performance-tests"></a>问︰ 多长时间才能运行性能测试？ 

**A**︰ 后公共预览，您得到 20000 虚拟用户分钟 (VUMs) 免费每月使用 Visual Studio 的团队服务帐户。 VUM 是虚拟用户在测试中的分钟数乘以数。 如果您需要超出了自由的限制，可以购买更多的时间，并只支付您的使用。

#### <a name="q-where-can-i-check-how-many-vums-ive-used-so-far"></a>问︰ 哪里可以检查到目前为止，我已经使用多少 VUMs？

**A**︰ 您可以检查此金额在 Azure 门户。

![转到您的团队服务帐户](./media/app-service-web-app-performance-test/azure-np-vso-accounts.png)

![检查使用的 VUMs](./media/app-service-web-app-performance-test/azure-np-vso-accounts-vum-summary.png)

#### <a name="q-what-is-the-default-option-and-are-my-existing-tests-impacted"></a>问︰ 什么是默认选项，我现有的测试受到影响？

**A**︰ 性能负载测试的默认选项手动测试的多个 URL 之前测试选项相同被添加到门户。
现有测试继续使用已配置的 URL，并将像以前一样工作。

#### <a name="q-what-features-not-supported-in-the-visual-studio-web-test-file"></a>问︰ 哪些功能的 Visual Studio 的 Web 测试中不受支持？

**A**︰ 目前此功能不支持 Web 测试插件程序、 数据源和提取规则。 您必须编辑 Web 测试文件以删除这些文字。 我们希望添加支持，这些功能在以后的更新中的。

#### <a name="q-does-it-support-any-other-web-test-file-formats"></a>问︰ 它支持任何其他 Web 测试文件格式？
  
**A**︰ 支持存在唯一 Visual Studio Web 测试格式的文件。
我们将很乐意听到您，如果您需要为其他文件格式的支持。 电子邮件地址与我们联系[vsoloadtest@microsoft.com](mailto:vsoloadtest@microsoft.com)。

#### <a name="q-what-else-can-i-do-with-a-visual-studio-team-services-account"></a>问︰ 使用 Visual Studio 的团队服务帐户可以执行还有什么？

**A**︰ 要了解您的新帐户，请转到```https://{accountname}.visualstudio.com```。 共享代码、 生成、 测试、 跟踪工作和装运软件 – 所有在云中使用任何工具或语言。 了解有关[Visual Studio 团队服务](https://www.visualstudio.com/products/what-is-visual-studio-online-vs)功能和服务如何帮助您的团队更容易地进行协作和连续部署。

## <a name="see-also"></a>请参见

* [运行简单的云性能测试](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-simple-cloud-load-test)
* [运行 Apache Jmeter 性能测试](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-jmeter-test)
* [记录和重放的基于云的负载测试](https://www.visualstudio.com/docs/test/performance-testing/getting-started/record-and-replay-cloud-load-tests)
* [性能测试您的应用程序在云中](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing)
