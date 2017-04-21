<properties
    pageTitle="将与 Azure 堆栈中的 Visual Studio 模板部署 |Microsoft Azure"
    description="了解如何部署使用 Azure 堆栈中的 Visual Studio 模板。"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>部署在 Azure 栈使用 Visual Studio 中的模板

使用 Visual Studio 将 Azure 资源管理器模板部署到 Azure 堆栈 POC。

资源管理器模板部署和调配单、 协调操作中应用程序的所有资源。

1.  打开 Visual Studio 2015年更新 1。

2.  单击**文件**，单击**新建**，在**新建项目**对话框中单击**Azure 资源组**。

3.  为新项目输入的**名称**，然后单击**确定**。

4.  在**选择 Azure 模板**对话框中，单击**Windows 虚拟机**，，然后单击**确定**。

  在新项目中，您可以通过展开**模板**节点在**解决方案资源管理器**窗格中看到可用模板的列表。

5.  在**解决方案资源管理器**窗格中，右键单击您的项目的名称，单击**部署**，然后单击**新部署**。

6.  在**部署到资源组**对话框中的**订阅**下拉列表中，选择您的 Microsoft Azure 堆栈订购。

7.  在**资源组**列表中，选择现有资源组或新建一个。

8.  在**资源组位置**列表中，选择位置，然后单击**部署**。

9.  在**编辑参数**对话框中，输入的值的参数 （它不同的模板），，然后单击**保存**。

## <a name="next-steps"></a>下一步行动

[部署模板使用命令行](azure-stack-deploy-template-command-line.md)
