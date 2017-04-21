<properties
    pageTitle="如何创建和部署云服务 |Microsoft Azure"
    description="了解如何创建和部署云服务使用 Azure 的门户。"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>如何创建和部署云服务

> [AZURE.SELECTOR]
- [Azure 门户](cloud-services-how-to-create-deploy-portal.md)
- [Azure 的传统门户网站](cloud-services-how-to-create-deploy.md)

Azure 的门户提供了两种方法来创建和部署云服务︰*快速创建*和*自定义创建*。

本文介绍如何使用快速创建方法来创建一个新的云服务，然后使用**上载**来上载和部署中 Azure 的云服务包。 使用此方法时，Azure 门户系统使得可用方便链接，当您完成所有要求。 如果您已经准备好部署云服务在创建它时，可以不要同时使用自定义创建的同一时间。

> [AZURE.NOTE] 如果您计划发布云服务从 Visual Studio 团队服务 (VSTS)，使用快速创建，并再从 Azure 快速入门或仪表板设置 VSTS 发布。 有关详细信息，请参阅[连续传递到 Azure 使用 Visual Studio 团队服务][TFSTutorialForCloudService]，或请参阅**快速启动**页的帮助。

## <a name="concepts"></a>概念
三个组件需要部署为在 Azure 的云服务应用程序︰

- **服务定义**  
  云服务定义文件 (.csdef) 定义的服务模型，包括角色的数目。

- **服务配置**  
  云服务配置文件 (.cscfg) 提供了配置设置的云服务和个人的角色，包括角色实例数。

- **服务包**  
  服务包 (.cspkg) 中包含的应用程序代码和配置的服务定义文件。

您可以了解更多有关这些以及如何创建包[下面](cloud-services-model-and-package.md)。

## <a name="prepare-your-app"></a>准备您的应用程序
您可以部署云服务之前，必须从应用程序代码和云服务配置文件 (.cscfg) 创建的云服务包 (.cspkg)。 Azure SDK 提供了准备这些所需的部署文件的工具。 从[Azure 下载](https://azure.microsoft.com/downloads/)页中的在语言中喜欢用来开发应用程序代码中，可以安装 SDK。

三种云服务功能要求特殊配置导出服务包之前︰

- 如果您要部署云服务为数据加密，ssl[配置应用程序](cloud-services-configure-ssl-certificate-portal.md#modify)使用安全套接字层 (SSL)。

- 如果您想要配置远程桌面连接到远程桌面[配置角色](cloud-services-role-enable-remote-desktop.md)的角色实例。 只是这种经典的门户。

- 如果您想要配置详细的云服务监视，云服务启用 Azure 诊断。 *最少的监控*（默认的监视级别） 使用性能计数器收集从主机操作系统的角色实例 （虚拟机）。 *详细监视*收集基于角色实例，以便更仔细地分析应用程序的处理过程中出现的问题中的性能数据的其他指标。 若要了解如何启用 Azure 诊断程序，请参阅[启用诊断 Azure 中](cloud-services-dotnet-diagnostics.md)。

要创建部署 web 角色或辅助角色的云服务，则必须[创建的服务包](cloud-services-model-and-package.md#servicepackagecspkg)。

## <a name="before-you-begin"></a>在开始之前

- 如果您还没有安装 Azure SDK，单击打开[Azure 下载页](https://azure.microsoft.com/downloads/)上，**安装 Azure SDK** ，然后下载 SDK 以查找您想开发代码的语言。 （您必须有机会这样做以后。）

- 如果任何角色实例需要证书，请创建证书。 云服务的专用密钥需要一个.pfx 文件。 您[可以上载到 Azure 证书]()创建并部署云服务。

- 如果您打算部署到关系组的云服务，创建关联组。 可以使用关系组将云服务和其他 Azure 服务部署到一个区域中的同一位置。 在 Azure 传统门户网站，在**关系组**页上的**网络**区域中，您可以创建关联组。


## <a name="create-and-deploy"></a>创建和部署

1. 登录到[Azure 的门户](https://portal.azure.com/)。
2. 单击**新建 > 虚拟机**，然后向下滚动并单击**云服务**。

    ![发布您的云服务](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)

3. 显示的信息页面的底部，单击**创建**。 
4. 在新的**云服务**刀片，输入**DNS 名称**的值。
5. 创建新的**资源组**，或选择一个现有。
6. 选择一个**位置**。
7. 单击**包**。 这将打开**上载程序包**刀片式服务器。 填写必填字段。  

     如果您的角色的任何包含的单个实例，确保**部署，即使一个或多个角色中包含的单个实例**处于选中状态。

8. 请确保已选中**开始部署**。
9. 单击**确定**将关闭**上载程序包**刀片式服务器。
10. 如果您没有添加任何证书，请单击**创建**。

    ![发布您的云服务](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>将上载证书

如果您部署程序包[配置为使用证书](cloud-services-configure-ssl-certificate-portal.md#modify)，您可以立即上载证书。

1. 选择**证书**和**添加证书**刀片式服务器，选择 SSL 证书.pfx 文件，然后提供该证书的**密码**
2. 单击**附加证书**，然后单击**确定****添加证书**刀片式服务器。
3. **云服务**刀片式服务器，请单击**创建**。 如果部署达到**就绪**状态，就可以继续下面的步骤。

    ![发布您的云服务](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)


## <a name="verify-your-deployment-completed-successfully"></a>验证已成功完成部署

1. 单击云服务实例。

    状态应显示该服务正在**运行**。

2. 下**精要**要在 web 浏览器中打开您的云服务的**网站 URL** 。

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)


[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>下一步行动

* [云服务的常规配置](cloud-services-how-to-configure-portal.md)。
* 配置[自定义的域的名称](cloud-services-custom-domain-name-portal.md)。
* [管理您的云服务](cloud-services-how-to-manage-portal.md)。
* 配置[ssl 证书](cloud-services-configure-ssl-certificate-portal.md)。