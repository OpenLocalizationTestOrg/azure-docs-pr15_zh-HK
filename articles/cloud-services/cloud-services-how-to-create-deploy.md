<properties
    pageTitle="如何创建和部署云服务 |Microsoft Azure"
    description="了解如何创建和部署云服务使用 Azure 中的快速创建方法。"
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
    ms.date="09/06/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>如何创建和部署云服务

> [AZURE.SELECTOR]
- [Azure 门户](cloud-services-how-to-create-deploy-portal.md)
- [Azure 的传统门户网站](cloud-services-how-to-create-deploy.md)

Azure 的传统门户网站提供了以下两种方式来创建和部署云服务︰**快速创建**和**自定义创建**。

本主题说明如何使用快速创建方法来创建一个新的云服务，然后使用**上载**来上载和部署中 Azure 的云服务包。 使用此方法时，Azure 的经典门户系统使得可用方便链接，当您完成所有要求。 如果您已经准备好部署云服务在创建它时，可以不要同时使用**自定义创建**的同一时间。

> [AZURE.NOTE] 如果您计划发布云服务从 Visual Studio 团队服务 (VSTS)，使用快速创建，并再从**快速启动**或仪表板设置 VSTS 发布。 有关详细信息，请参阅[连续传递到 Azure 使用 Visual Studio 团队服务][TFSTutorialForCloudService]，或请参阅**快速启动**页的帮助。

## <a name="concepts"></a>概念
为了部署在 Azure 的云服务应用程序需要三个组件︰

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

- 如果您要部署云服务为数据加密，ssl[配置应用程序](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files)使用安全套接字层 (SSL)。

- 如果您想要配置远程桌面连接到远程桌面[配置角色](cloud-services-role-enable-remote-desktop.md)的角色实例。

- 如果您想要配置详细的云服务监视，云服务启用 Azure 诊断。 *最少的监控*（默认的监视级别） 使用性能计数器收集从主机操作系统的角色实例 （虚拟机）。 "详细监视 * 收集其他基于角色实例，以便更仔细地分析应用程序的处理过程中出现的问题中的性能数据。 若要了解如何启用 Azure 诊断程序，请参阅[启用 Azure 中的诊断程序](cloud-services-dotnet-diagnostics.md)。

要创建部署 web 角色或辅助角色的云服务，则必须[创建的服务包](cloud-services-model-and-package.md#servicepackagecspkg)。

## <a name="before-you-begin"></a>在开始之前

- 如果您还没有安装 Azure SDK，单击打开[Azure 下载页](https://azure.microsoft.com/downloads/)上，**安装 Azure SDK** ，然后下载 SDK 以查找您想开发代码的语言。 （您必须有机会这样做以后。）

- 如果任何角色实例需要证书，请创建证书。 云服务的专用密钥需要一个.pfx 文件。 可以[上载到 Azure 的证书](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate)为您创建和部署云服务。

- 如果您打算部署到关系组的云服务，创建关联组。 可以使用关系组将云服务和其他 Azure 服务部署到一个区域中的同一位置。 在 Azure 传统门户网站，在**关系组**页上的**网络**区域中，您可以创建关联组。


## <a name="how-to-create-a-cloud-service-using-quick-create"></a>如何︰ 创建使用快速创建一个云服务

1. 在[Azure 的传统门户网站](http://manage.windowsazure.com/)中，单击**新建**>**计算**>**云服务**>**快速创建**。

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)

2. 在**URL**中，输入用于访问云服务在生产部署中使用公用 URL 中。 生产部署的 URL 格式是︰ http://*myURL*。 cloudapp.net。

3. 在**区域或相关性组**中，选择关系组部署到云服务的地理区域。 如果您想要将您的云服务部署到与区域内其他 Azure 服务相同的位置，请选择关系组。

4. 单击**创建云服务**。

    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)

    您可以监视窗口底部的消息区域中的进程的状态。

    **云服务**区域中将打开，其中显示新的云服务。 当状态更改为创建时，创建云服务已成功完成。

    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)


## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>如何︰ 上载云服务的证书

1. 在[Azure 的传统门户网站](http://manage.windowsazure.com/)中，单击**云服务**，单击云服务的名称，然后单击**证书**。

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)


2. 单击**上载证书**或**上载**。

3. 在**文件**中，使用**浏览**，选择证书 （.pfx 文件）。

4. 在**密码**中，输入证书的专用密钥。

5. 单击**确定**（复选标记）。

    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)

    您可以监视进度的上载消息区域中，如下所示。 上载完成后，该证书被添加到表中。 在消息区域中，单击确定关闭该消息。

    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>如何︰ 部署云服务

1. 在[Azure 的传统门户网站](http://manage.windowsazure.com/)中，单击**云服务**，单击云服务的名称，然后单击**仪表板**。

2. 单击**上载新的生产部署**或**上载**。

3. 在**部署标签**，输入新的部署-例如，MyCloudServicev4 的名称。

3. 在**包**中，使用**浏览**，选择服务包 (.cspkg) 要使用的文件。

4. 在**配置**使用**浏览**，选择使用服务配置文件 (.cscfg)。

5. 如果云服务将包括与只有一个实例的任何角色，则选择**部署，即使一个或多个角色中包含的单个实例**复选框以启用该部署，以继续。

    Azure 只能保证 99.95%访问云服务维护和服务更新期间如果每个角色都有至少两个实例。 如果需要您可以添加其他角色实例**比例**页面后部署在云服务。 有关详细信息，请参阅[服务级别协议要求](https://azure.microsoft.com/support/legal/sla/)。

6. 单击**确定**（复选标记） 开始部署云服务。

    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)

    您可以监视消息区域中部署的状态。 单击确定以隐藏消息。

    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>验证已成功完成部署

1. 单击**仪表板**。

    状态应显示该服务正在**运行**。

2. 在**快速浏览**下，单击网站 URL 以在 web 浏览器中打开您的云服务。

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


[TFSTutorialForCloudService]: cloud-services-continuous-delivery-use-vso.md
 
## <a name="next-steps"></a>下一步行动

* [云服务的常规配置](cloud-services-how-to-configure.md)。
* 配置[自定义的域的名称](cloud-services-custom-domain-name.md)。
* [管理您的云服务](cloud-services-how-to-manage.md)。
* 配置[ssl 证书](cloud-services-configure-ssl-certificate.md)。