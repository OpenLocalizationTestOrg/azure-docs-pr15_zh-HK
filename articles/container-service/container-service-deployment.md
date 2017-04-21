<properties
   pageTitle="部署群集 Azure 容器服务 |Microsoft Azure"
   description="使用 Azure 的门户，Azure CLI 或 PowerShell 部署群集 Azure 容器服务。"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker，容器，微服务 Mesos，Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="deploy-an-azure-container-service-cluster"></a>部署群集 Azure 容器服务

Azure 容器服务提供的流行的开源容器快速部署群集和业务流程的解决方案。 通过使用 Azure 容器服务，您可以部署 DC/OS 和 Azure 资源管理器模板 Docker 群簇或 Azure 的门户。 您部署使用 Azure 虚拟机的扩展集，这些群集和群集利用 Azure 的网络和存储产品。 若要访问 Azure 容器服务，您需要订阅了 Azure。 如果您没有一个，然后您可以注册[免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935)。

本文档将指导您完成使用[Azure 门户](#creating-a-service-using-the-azure-portal)、 [Azure 的命令行界面 (CLI)](#creating-a-service-using-the-azure-cli)和[Azure PowerShell 模块](#creating-a-service-using-powershell)部署群集 Azure 容器服务。  

## <a name="create-a-service-by-using-the-azure-portal"></a>通过使用 Azure 门户创建服务

登录到 Azure 的门户网站，选择**新建**和**Azure 容器服务**搜索 Azure 市场。

![创建部署 1](media/acs-portal1.png)  <br />

选择**Azure 容器服务**，然后单击**创建**。

![创建部署 2](media/acs-portal2.png)  <br />

输入以下信息︰

- **用户名**︰ 这是将用于每个虚拟机上的一个帐户的用户名和 Azure 容器服务群集中的虚拟机比例设置。
- **订阅**︰ 选择 Azure 的订阅。
- **资源组**︰ 选择现有资源组，或创建一个新。
- **位置**︰ 选择 Azure Azure 容器服务部署的地区。
- **SSH 公钥**︰ 添加公钥将用于对 Azure 容器服务虚拟机的身份验证。 此项包含没有换行符，并包含 ssh 的 rsa 前缀非常重要和'username@domain'后缀。 它应该看起来如下所示︰ **...ssh rsa AAAAB3Nz <>......UcyupgH azureuser@linuxvm **。 创建安全外壳协议 (SSH) 键的指导，请参阅[Linux]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/)和[Windows]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/)的文章。

当你准备好继续时，请单击**确定**。

![创建部署 3](media/acs-portal3.png)  <br />

选择业务流程类型。 包含以下选项︰

- **DC/OS**︰ 部署一个 DC/OS 群集。
- **群**︰ 部署 Docker 群群集。

当你准备好继续时，请单击**确定**。

![创建部署 4](media/acs-portal4.png)  <br />

输入以下信息︰

- **主数据统计**︰ 在群集中的主机数。
- **代理程序计数**︰ 对于 Docker 群，这将是初始的代理比例集中的代理数。 针对 DC/OS，这将是初始的代理程序集中的私有比例数。 此外，创建公共扩展集，其中包含预定的代理的数量。 通过多少母版创建的群集--对于一个母版，一个公用代理和三个或五个主服务器的两个公用代理确定此公用比例集中的代理数。
- **代理的虚拟机大小**︰ 代理的虚拟机的大小。
- **DNS 前缀**︰ 世界上唯一的名称，将使用前缀的完全限定的域名服务的关键部分。

当你准备好继续时，请单击**确定**。

![创建部署 5](media/acs-portal5.png)  <br />

服务验证完毕后，请单击**确定**。

![创建部署 6](media/acs-portal6.png)  <br />

单击**创建**来启动部署过程。

![创建部署 7](media/acs-portal7.png)  <br />

如果您已经选择固定到 Azure 门户部署，您可以看到的部署状态。

![创建部署 8](media/acs-portal8.png)  <br />

完成部署后，Azure 容器服务群集可供使用。

## <a name="create-a-service-by-using-the-azure-cli"></a>通过使用 Azure CLI 创建服务

若要使用命令行创建 Azure 容器服务的实例，您需要订阅了 Azure。 如果您没有一个，然后您可以注册[免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935)。 您还需要[安装](../xplat-cli-install.md)和[配置](../xplat-cli-connect.md)Azure CLI。

若要部署一个 DC/OS 或 Docker 群群集，选择从 GitHub 下列模板之一。 请注意这两个模板都是相同的但默认控制器所选内容。

* [DC/OS 模板](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [蜂群模板](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

下一步，确保 Azure CLI 确保已连接到 Azure 的订阅。 您可以使用下面的命令来执行此操作︰

```bash
azure account show
```
如果未返回的 Azure 帐户，可以使用下面的命令来使 CLI 登录到 Azure。

```bash
azure login -u user@domain.com
```

接下来，配置了 Azure CLI 工具来使用 Azure 资源管理器。

```bash
azure config mode arm
```

使用以下命令，创建 Azure 的资源组，群集服务容器的位置︰

- **RESOURCE_GROUP**是您想要使用这项服务的资源组的名称。
- **位置**是 Azure 将在其中创建的资源组和 Azure 容器服务部署的地区。
- **TEMPLATE_URI**是部署文件的位置。 请注意，这必须是原始文件，不是指针 GitHub UI。 若要查找此 URL，在 GitHub，选择 azuredeploy.json 文件并单击**原始**按钮。

> [AZURE.NOTE] 运行此命令时，shell 将提示您为部署参数值。

```bash
azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>提供模板参数

此版本的命令需要您以交互方式定义参数。 如果您想要提供的参数，比如一个 JSON 格式的字符串，则可以这样使用`-p`开关。 例如︰

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

或者，您可以通过提供一个 JSON 格式的参数文件`-e`切换︰

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

若要查看一个示例参数文件，名为`azuredeploy.parameters.json`，使用 Azure 容器服务模板在 GitHub 中寻找。

## <a name="create-a-service-by-using-powershell"></a>通过使用 PowerShell 创建服务

您还可以部署群集使用 PowerShell Azure 容器服务。 本文基于 1.0 版本[Azure PowerShell 模块](https://azure.microsoft.com/blog/azps-1-0/)。

要部署 DC/OS 或 Docker 群群集，请选择下列模板之一。 请注意这两个模板都是相同的但默认控制器所选内容。

* [DC/OS 模板](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [蜂群模板](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

在创建之前在 Azure 的订阅中，群集验证，PowerShell 会话具有已登录到 Azure。 你可以用`Get-AzureRMSubscription`命令︰

```powershell
Get-AzureRmSubscription
```

如果您需要登录到 Azure，使用`Login-AzureRMAccount`命令︰

```powershell
Login-AzureRmAccount
```

如果您正在部署到新的资源组，您必须首先创建的资源组。 若要创建新的资源组，请使用`New-AzureRmResourceGroup`命令，并指定资源组名称和目标区域︰

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

创建资源组后，您可以使用以下命令创建群集。 将为指定的 URI 所需模板的`-TemplateUri`参数。 当您运行此命令时，PowerShell 将提示您为部署参数值。

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>提供模板参数

如果您熟悉 PowerShell，您知道，您可以通过键入一个减号 （-），然后按 TAB 键来循环使用 cmdlet 的可用参数。 与您在您的模板中定义的参数也适用同样的性能。 只要键入模板名称，cmdlet 提取模板、 分析参数，并将模板参数动态地添加到该命令。 这使得它很容易地指定模板参数值。 并且，如果您忘记了所需的参数值，PowerShell 的提示值。

下面是完整的命令，与包括参数。 资源的名称，您可以提供您自己的值。

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>下一步行动

现在，您已经运行的群集中，这些文档连接和管理的详细信息，请参阅︰

- [连接到 Azure 容器服务群集](container-service-connect.md)
- [使用 Azure 容器服务和 DC/OS](container-service-mesos-marathon-rest.md)
- [使用 Azure 容器服务和 Docker 蜂群](container-service-docker-swarm.md)
