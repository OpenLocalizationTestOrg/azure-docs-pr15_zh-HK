<properties
   pageTitle="Amazon Web 服务使用配置的身份验证 |Microsoft Azure"
   description="本文介绍如何创建和验证运行手册在 Azure 自动化管理 AWS 资源 AWS 凭据。"
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="aws 的身份验证配置 aws"/>
<tags
   ms.service="automation"
   ms.workload="tbd"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.date="09/12/2016"
   ms.author="magoedte"/>

# <a name="authenticate-runbooks-with-amazon-web-services"></a>身份验证与 Amazon Web 服务运行手册
可以使用自动化运行手册在 Azure 中实现自动化常见任务的资源在 Amazon Web 服务 (AWS)。  您可以自动执行许多任务在 AWS 使用自动化运行手册一样可以在 Azure 中的资源。  只需两个事项︰

* AWS 订阅和一套凭据。  特别是您 AWS 访问键和机密密钥。  有关详细信息，请查看文章[使用 AWS 凭据](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html)。
* Azure 订阅和自动化帐户。  Azure 自动帐户设置的详细信息，请查看文章[配置 Azure 运行方式帐户](../automation/automation-sec-configure-azure-runas-account.md)。  

要使用 AWS 进行身份验证，必须指定一套 AWS 凭据进行身份验证运行手册从 Azure 自动化运行。 如果您已经创建的自动化帐户并且想要使用的身份验证使用 AWS，您可以按照下一节中的步骤。  如果想要专门运行手册定向 AWS 资源帐户，应首先创建一个新的[自动化运行方式帐户](../automation/automation-sec-configure-azure-runas-account.md)（跳过创建服务主体的选项），然后按照下面的步骤。

## <a name="configure-automation-account"></a>配置自动化帐户
Azure 自动化与 AWS 进行通信，您需要检索 AWS 凭据并将它们存储为 Azure 自动化中的资产。  执行以下步骤记录在 AWS 文档[管理 AWS 帐户的访问键](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html)来创建一个访问键，**访问键 ID**和**密钥访问**复制 （还可以选择下载您的密钥文件存储安全的地方）。

已经创建并复制 AWS 安全密钥后，您需要安全地将它们存储和运行手册用引用它们的 Azure 自动化帐户创建凭据资产。  按照一节中**创建新的凭据**[凭据资产 Azure 自动化在](../automation/automation-certificates.md/###To create a new credential with the Azure portal)文章中的步骤并输入以下信息︰

1. 在**名称**框中，输入**AWScred**或按照您的命名标准为适当的值。  
2. 在**用户名**框中键入您的**访问 ID**和您的**访问密钥****密码**和**确认密码**框中。   

## <a name="next-steps"></a>下一步行动

- 审阅标有解决方案文章[自动化部署的虚拟机在 Amazon Web 服务中](../automation/automation-scenario-aws-deployment.md)，若要了解如何创建运行手册在 AWS 自动执行任务。
