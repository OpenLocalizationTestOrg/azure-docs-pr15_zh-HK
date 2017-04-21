<properties 
    pageTitle="生成在 Visual Studio 中的逻辑应用程序 |Microsoft Azure" 
    description="在 Visual Studio 创建和部署您的逻辑应用程序创建项目。" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/> 
    
# <a name="build-and-deploy-logic-apps-in-visual-studio"></a>生成和部署在 Visual Studio 中的逻辑应用程序

虽然[Azure 门户网站](https://portal.azure.com/)为您提供设计和管理您的逻辑应用程序的好方法，还可能想要设计和部署从 Visual Studio 应用程序逻辑，而是。  逻辑应用程序方面具有丰富 Visual Studio 工具集使您可以构建使用设计器的逻辑应用程序，配置所有部署和自动化的模板，并将其部署到任何环境。  

## <a name="installation-steps"></a>安装步骤

以下是安装和配置 Visual Studio 工具，用于逻辑的应用程序的步骤。

### <a name="prerequisites"></a>系统必备组件

- [Visual Studio 2015 年](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- [最新的 Azure SDK](https://azure.microsoft.com/downloads/)（2.9.1 或更高版本）
- [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
- 访问 web 嵌入式设计器时

### <a name="install-visual-studio-tools-for-logic-apps"></a>逻辑应用程序安装 Visual Studio 工具

一旦您有安装系统必备组件 

1. 到**工具**菜单打开 Visual Studio 2015年，选择**扩展和更新**
1. 选择**联机**类别联机搜索
1. 搜索**逻辑应用程序**以显示**Visual Studio 的 Azure 逻辑应用程序工具**
1. 单击**下载**按钮以下载并安装扩展
1. 安装后重新启动 Visual Studio

> [AZURE.NOTE] 您还可以直接从[该链接](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9)下载扩展

安装完成后，您将能够使用逻辑应用程序设计器中使用 Azure 资源组项目。

## <a name="create-a-project"></a>创建项目

1. 转到**文件**菜单并选择**新建** >  **项目**（或者，您可以转到**添加**，然后选择**新的项目**以将其添加到现有解决方案）︰ ![文件菜单](./media/app-service-logic-deploy-from-vs/filemenu.png)

1. 在对话框中，找到**云**，然后选择**Azure 的资源组**。 键入一个**名称**，然后单击**确定**。
    ![添加新项目](./media/app-service-logic-deploy-from-vs/addnewproject.png)

1. 选择**逻辑应用程序**模板。 这将首先创建一个空的逻辑应用程序部署模板。
    ![选择碧空模板](./media/app-service-logic-deploy-from-vs/selectazuretemplate.png)

1. 一旦选择了您的**模板**，请单击**确定**。

    现在逻辑应用程序项目添加到解决方案中。 您应该看到在解决方案资源管理器中的部署文件︰  

    ![部署](./media/app-service-logic-deploy-from-vs/deployment.png)

## <a name="using-the-logic-app-designer"></a>使用逻辑应用程序设计器

Azure 资源组项目包含的逻辑应用程序后，您可以打开设计器在 Visual Studio 中，以帮助您创建工作流。  设计器需要连接到 internet 才能查询可用的属性和数据的连接器 （例如，如果使用 Dynamics CRM Online 的连接器，设计器将查询您的 CRM 实例，列出可用的自定义和默认属性）。

1. 用鼠标右键单击`<template>.json`文件，然后选择**与逻辑应用程序设计器中打开**(或`Ctrl+L`)
1. 选择订阅、 资源组和部署模板的位置
    - 请务必注意，设计逻辑应用程序将创建**API 连接**资源属性的查询在设计过程中。  所选的资源组将用来在设计时创建这些连接的资源组。  您可以查看或修改任何 API 连接通过转到 Azure 门户并浏览的**API 连接**。
    ![预订选择器](./media/app-service-logic-deploy-from-vs/designer_picker.png)
1. 设计器应呈现基于中定义`<template>.json`文件。
1. 您现在可以创建并设计您的逻辑的应用程序，并将部署模板中更新这些更改。
    ![在 Visual Studio 设计器](./media/app-service-logic-deploy-from-vs/designer_in_vs.png)

您还将看到`Microsoft.Web/connections`资源添加到资源文件中函数的逻辑应用程序所需的所有连接。  这些连接属性可设置在部署时，和管理**API 连接**在 Azure 门户中部署之后。

### <a name="switching-to-the-json-code-view"></a>切换到 JSON 代码视图

您可以选择**代码视图**选项卡底部的设计器切换到应用程序逻辑的 JSON 表示。  要切换到 JSON 的全部资源，请右键单击`<template>.json`文件并选择**打开**。

### <a name="saving-the-logic-app"></a>保存逻辑应用程序

您可以保存逻辑应用程序在任何时候通过**保存**按钮或`Ctrl+S`。  如果在保存时没有逻辑应用程序的任何错误，则它们将显示在 Visual Studio 的**输出**窗口中。

## <a name="deploying-your-logic-app"></a>部署您的应用程序逻辑

最后，在配置您的应用程序之后，您可以直接从 Visual Studio 中只有两个步骤的部署。 

1. 右击解决方案资源管理器中的项目，然后转到**部署** > **新部署...**
    ![新部署](./media/app-service-logic-deploy-from-vs/newdeployment.png)

2. 提示您登录到您的 Azure 订购。 

3. 现在您需要选择要部署到的逻辑应用程序的资源组的详细信息。 
    ![将部署到资源组](./media/app-service-logic-deploy-from-vs/deploytoresourcegroup.png)

     > [AZURE.NOTE]    请务必选择正确的模板和参数文件为资源组 （例如如果要部署到生产环境中可能需要选择生产参数文件）。 
4. 选择部署按钮
 
    
6. 部署的状态显示在**输出**窗口 （您可能需要选择**Azure 资源调配**。 
    ![输出](./media/app-service-logic-deploy-from-vs/output.png)

在将来，可以修改您在源代码管理中的逻辑应用程序并使用 Visual Studio 部署新版本。 

> [AZURE.NOTE] 如果直接修改 Azure 门户中的定义，在您从 Visual Studio 将这些更改部署的下一次将被覆盖。

## <a name="next-steps"></a>下一步行动

- 入门的逻辑应用程序，请按照[创建逻辑应用程序](app-service-logic-create-a-logic-app.md)的教程。  
- [查看常见示例和应用场景](app-service-logic-examples-and-scenarios.md)
- [可以自动执行业务流程逻辑的应用程序](http://channel9.msdn.com/Events/Build/2016/T694) 
- [了解如何将您的系统集成与应用程序逻辑](http://channel9.msdn.com/Events/Build/2016/P462)