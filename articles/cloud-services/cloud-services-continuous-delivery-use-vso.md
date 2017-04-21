<properties
    pageTitle="使用 Visual Studio 团队 Azure 服务不断提供 |Microsoft Azure"
    description="了解如何配置 Visual Studio 团队服务团队项目来自动生成和部署对 Azure 应用程序服务或云服务中的 Web 应用程序功能。"
    services="cloud-services"
    documentationCenter=".net"
    authors="mlearned"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="mlearned"/>

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>连续传递到 Azure 使用 Visual Studio 的团队服务

您可以配置 Visual Studio 团队服务团队项目来自动生成并部署到 Azure 的 web 应用程序或云服务。  （有关如何设置连续生成和部署使用*内部*Team Foundation Server 的系统的信息，请参阅[在 Azure 的云服务不断提供](cloud-services-dotnet-continuous-delivery.md)）。

本教程假设您拥有 Visual Studio 2013年和 Azure SDK 安装。 如果您尚没有 Visual Studio 2013年，通过选择的**入门免费**链接可以在[www.visualstudio.com](http://www.visualstudio.com)下载。 从[此处](http://go.microsoft.com/fwlink/?LinkId=239540)安装 Azure SDK。

> [AZURE.NOTE]您需要一个 Visual Studio 团队服务帐户来完成本教程︰ 您可以[打开 Visual Studio 团队服务的客户免费](http://go.microsoft.com/fwlink/p/?LinkId=512979)。

若要设置自动生成并使用 Visual Studio 的团队服务部署到 Azure 的云服务，请按照下列步骤。

## <a name="1-create-a-team-project"></a>1︰ 创建团队项目

按照[此处](http://go.microsoft.com/fwlink/?LinkId=512980)创建团队项目并将其链接到 Visual Studio。 本演练假定您使用的团队的基础版本控制 (TFVC) 作为您的源控制解决方案。 如果您想要使用 Git 版本控制，请参见[本演练的 Git 版本](http://go.microsoft.com/fwlink/p/?LinkId=397358)。

## <a name="2-check-in-a-project-to-source-control"></a>2︰ 签入到源代码管理的项目

1. 在 Visual Studio 中，打开您要部署的解决方案，或新建一个。
可以在本演练中的步骤来部署 web 应用程序或云服务 （Azure 应用程序）。
如果您想要创建新的解决方案，创建新的 Azure 云服务项目或一个新的 ASP.NET MVC 项目。 请确保项目面向.NET Framework 4 或 4.5，，如果您正在创建一个云服务项目，添加 ASP.NET MVC web 角色和一个辅助的角色，然后选择 web 角色的互联网应用程序。 出现提示时，选择**Internet 应用程序**。
如果您想要创建一个 web 应用程序，ASP.NET Web 应用程序项目模板中，选择，然后选择 MVC。 请参阅[创建在 Azure 应用程序服务的 ASP.NET web 应用程序](../app-service-web/web-sites-dotnet-get-started.md)。

    > [AZURE.NOTE] Visual Studio 团队服务仅支持这一次的 CI 的 Visual Studio 的 Web 应用程序的部署。 网站项目超出范围。

1. 打开的解决方案中，上下文菜单并选择**将解决方案添加到源代码管理**。

    ![][5]

1. 接受或更改默认设置，然后选择**确定**按钮。 完成此过程后，源代码管理图标将出现在**解决方案资源管理器**中。

    ![][6]

1. 打开的解决方案中，快捷菜单，然后选择**签入**。

    ![][7]

1. 在**团队资源管理器**的**挂起的更改**区域中，键入签入的注释，然后选择**签入**按钮。

    ![][8]

    请注意包含或排除特定的更改，当您签入选项。 如果被排除所需的更改，选择**全部包括**链接。

    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: 项目连接到 Azure

1. 现在，有的与一些源代码 VS 团队服务团队项目，您就可以将您的团队项目连接到 Azure。  在[Azure 的传统门户网站](http://go.microsoft.com/fwlink/?LinkID=213885)中，选择您的云服务或 web 应用程序，或创建一个新的选择**+**在左下角和选择**云服务**或**Web 应用程序**，然后**快速创建**图标。 选择的**设置与 Visual Studio 团队服务发布**链接。

    ![][10]

1. 在向导中，在文本框中键入您的 Visual Studio 团队服务帐户的名称，请单击**立即授权**链接。 您可能需要在每次登录。

    ![][11]

1. 在**连接请求**弹出对话框中，选择**接受**按钮，授权 Azure VS 团队服务中配置团队项目。

    ![][12]

1. 授权成功后，您将看到包含 Visual Studio 团队服务团队项目的列表的下拉列表。 选择在前面的步骤中创建的团队项目的名称，然后选择向导的复选标记按钮。

    ![][13]

1. 您的项目链接后，您将看到更改签入到 Visual Studio 团队服务团队项目的说明。  在您下次签入时，Visual Studio 团队服务将生成并将项目部署到 Azure。  试试这个立即通过单击**签入从 Visual Studio**链接，然后**启动 Visual Studio**链接 （或门户屏幕底部的等效**Visual Studio**按钮）。

    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4︰ 触发重新构建和重新部署您的项目

1. 在 Visual Studio 的**团队资源管理器**，选择**解决方案资源管理器**链接。

    ![][15]

1. 定位到您的解决方案文件并打开它。

    ![][16]

1. 在**解决方案资源管理器**中打开的文件并对其进行更改。 例如，更改文件`_Layout.cshtml`在视图\\MVC web 角色中的共享文件夹。

    ![][17]

1. 编辑站点徽标并选择**Ctrl + S**可保存该文件。

    ![][18]

1. 在**团队资源管理器**中，选择**挂起的更改**链接。

    ![][19]

1. 输入注释，然后选择**签入**按钮。

    ![][20]

1. 选择**主页**按钮以返回到**团队资源管理器**主页上。

    ![][21]

1. 选择**生成**链接来查看正在进行的生成。

    ![][22]

    **团队资源管理器**显示已为您签入触发的生成。

    ![][23]

1. 双击正在查看详细的日志生成过程生成的名称。

    ![][24]

1. 正在进行生成时，看一下当您链接到 Azure 使用向导 TFS 创建的生成定义。  打开该生成定义的快捷菜单，然后选择**编辑生成定义**。

    ![][25]

    在**触发器**选项卡中，您将看到，生成定义设置为在每次签入生成默认。

    ![][26]

    在**进程**选项卡，您可以看到部署环境设置为您的云服务或 web 应用程序的名称。 如果使用 web 应用程序时，您会看到属性将不同于此处所示。

    ![][27]

1. 如果您希望与默认值不同的值，请指定属性的值。 在**部署**部分中的 Azure 的发布属性。

    下表显示在**部署**部分中的可用属性︰

  	|属性|默认值|
  	|---|---|
  	|允许不受信任的证书|如果为 false，则必须由根证书颁发机构签名的 SSL 证书。|
  	|允许升级|允许更新现有部署而不是创建一个新的部署。 保留的 IP 地址。|
  	|请不要删除|如果为 true，则不会覆盖现有的相关的部署 （允许升级）。|
  	|部署设置路径|用于 web 应用程序中，相对于根目录文件夹的 repo.pubxml 文件的路径。 对于云服务忽略。|
  	|Sharepoint 部署环境|服务名称相同。|
  	|Azure 的部署环境|Web 应用程序或云服务名称。|

1. 如果您正在使用多个服务配置 （.cscfg 文件），您可以在**生成，高级，MSBuild 参数**设置中指定所需的服务配置。 例如，若要使用 ServiceConfiguration.Test.cscfg，设置 MSBuild 参数行选项`/p:TargetProfile=Test`。

    ![][38]

    这个时候，应成功完成您的生成。

    ![][28]

1. 如果您双击生成名称，Visual Studio 将显示**生成摘要**，包括关联的单元测试项目中的所有测试结果。

    ![][29]

1. 在[Azure 的传统门户网站](http://go.microsoft.com/fwlink/?LinkID=213885)中，您可以在**部署**选项卡上查看关联的部署当选临时环境。

    ![][30]

1.  浏览到您的站点的 URL。 对于 web 应用程序，只需单击命令栏上的**浏览**按钮。 对于云服务，**仪表板**显示的页，云服务的临时环境的**快速概览**部分中选择 URL。 默认情况下，从云服务的持续集成的部署发布到临时环境。 您可以更改此**备用云服务环境**属性设置到**生产环境**。 此屏幕快照所示，其中该网站的 URL 位于云服务的仪表板页面。

    ![][31]

    一个新的浏览器选项卡会打开以显示您正在运行的站点。

    ![][32]

    对于云服务，如果您对您的项目中进行其他更改，则触发器更生成，而且将累计多个部署。 最新标记为活动。

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5︰ 重新部署较早的版本

此步骤适用于云服务，并且是可选的。 Azure 的传统门户网站，选择一个更早的部署，然后选择**重新部署**按钮退到早期签入您的网站。  注意，这将触发新生成在 TFS，您部署的历史记录中创建新条目。

![][34]

## <a name="6-change-the-production-deployment"></a>6︰ 更改生产部署

此步骤仅适用于云服务而不是 web 应用程序。 准备就绪后，可以通过在 Azure 的传统门户网站中选择**替换**按钮来提升暂存环境与生产环境。 新部署的临时环境被提升为生产，和以前的生产环境中，如果有的话，将成为临时环境。 活动部署可能是不同的生产环境和临时环境，但最新版本的部署历史记录是相同的无论环境如何。

![][35]

## <a name="7-run-unit-tests"></a>7︰ 运行单元测试

此步骤仅适用于 web 应用程序，请不云服务。 若要使您的部署的质量要求，可以运行单元测试，如果他们失败，则可以停止部署。

1.  在 Visual Studio 中，将添加一个单元测试项目。

    ![][39]

1.  添加您想要测试的项目的项目引用。

    ![][40]

1.  添加一些单元测试。 若要开始，请尝试将始终通过虚拟测试。

        ```
        using System;
        using Microsoft.VisualStudio.TestTools.UnitTesting;

        namespace UnitTestProject1
        {
            [TestClass]
            public class UnitTest1
            {
                [TestMethod]
                [ExpectedException(typeof(NotImplementedException))]
                public void TestMethod1()
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1.  编辑生成定义，选择**进程**选项卡，并展开**测试**节点。

1.  设置为 True**使未生成测试失败**。 这意味着部署将不会发生，除非通过了测试。

    ![][41]

1.  新生成进行排队。

    ![][42]

    ![][43]

1. 而继续生成，请检查其进度。

    ![][44]

    ![][45]

1. 完成生成后，请检查测试结果。

    ![][46]

    ![][47]

1.  请尝试创建一个将失败的测试。 通过复制第一个添加新的测试，其重命名，注释掉 NotImplementedException 是预期的异常的代码行。

        ```
        [TestMethod]
        //[ExpectedException(typeof(NotImplementedException))]
        public void TestMethod2()
        {
            throw new NotImplementedException();
        }
        ```

1. 签入更改为新生成进行排队。

    ![][48]

1. 查看有关失败的详细信息，请参阅测试结果。

    ![][49]

    ![][50]

## <a name="next-steps"></a>下一步行动
关于单元测试在 Visual Studio 的团队服务的详细信息，请参阅[运行在您生成的单元测试](http://go.microsoft.com/fwlink/p/?LinkId=510474)。 如果您正在使用 Git，请参阅[共享在 Git 代码](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx)和[连续部署到 Azure 应用程序服务](../app-service-web/app-service-continuous-deployment.md)。  有关 Visual Studio 团队服务的详细信息，请参阅[Visual Studio 的团队服务](http://go.microsoft.com/fwlink/?LinkId=253861)。

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
