<properties
   pageTitle="使用资源管理器中的 Azure CLI 应用程序网关创建 |Microsoft Azure"
   description="了解如何使用 Azure CLI 在资源管理器中创建应用程序网关"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>通过使用 Azure CLI 创建应用程序网关

> [AZURE.SELECTOR]
- [Azure 门户](application-gateway-create-gateway-portal.md)
- [资源管理器的 azure PowerShell](application-gateway-create-gateway-arm.md)
- [Azure 的经典 PowerShell](application-gateway-create-gateway.md)
- [Azure 的资源管理器模板](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure 应用程序网关是一个 7 层负载平衡器。 无论是对云的内部，它提供了故障切换时，不同的服务器之间的路由性能的 HTTP 请求。 应用程序网关具有以下应用程序交付功能︰ HTTP 负载平衡、 基于 cookie 的会话关联和安全套接字层 (SSL) 卸载自定义运行状况探测器和多站点的支持。

## <a name="prerequisite-install-the-azure-cli"></a>前提条件︰ 安装 Azure CLI

若要执行本文中的步骤，您需要[安装 Mac、 Linux、 和 (Azure CLI) Windows Azure 命令行界面](../xplat-cli-install.md)，您需要[登录到 Azure](../xplat-cli-connect.md)。 

> [AZURE.NOTE] 如果您没有 Azure 帐户，您需要一个。 [这里免费试用版](../active-directory/sign-up-organization.md)转号。

## <a name="scenario"></a>方案

在此方案中，您将学习如何创建使用 Azure 的门户应用程序网关。

该方案将︰

- 创建两个实例中应用程序网关。
- 创建名为 AdatumAppGatewayVNET 与 10.0.0.0/16 保留 CIDR 块的虚拟网络。
- 创建名为 Appgatewaysubnet，使用 10.0.0.0/28 作为其 CIDR 块的子网。
- 配置 SSL 卸载证书。

![方案示例][scenario]

>[AZURE.NOTE] 应用程序网关，包括自定义运行状况的其他配置探测，配置应用程序网关后，并不是在初始部署过程配置后端池地址和其他规则。

## <a name="before-you-begin"></a>在开始之前

Azure 应用程序网关需要各自的子网上。 当创建虚拟网络时，请确保保留足够具有多个子网的地址空间。 一旦部署到的子网使用应用程序网关，仅附加应用程序网关都能够添加到子网。

## <a name="log-in-to-azure"></a>登录到 Azure 中

打开**Microsoft Azure 命令提示符**，然后登录。 

    azure login

一旦您键入前面的示例中，提供了代码。 导航到浏览器以继续登录过程中的 https://aka.ms/devicelogin。

![cmd 显示设备登录][1]

在浏览器中，输入您收到的代码。 您将被重定向到登录页。

![浏览器输入代码][2]

一旦输入代码签名，关闭浏览器以继续该方案。

![成功登录][3]

## <a name="switch-to-resource-manager-mode"></a>切换到资源管理器模式

    azure config mode arm

## <a name="create-the-resource-group"></a>创建资源组

在创建应用程序网关之前, 创建资源组包含应用程序网关。 下图显示该命令。

    azure group create -n AdatumAppGatewayRG -l eastus

## <a name="create-a-virtual-network"></a>创建虚拟网络

创建资源组后，应用程序网关创建虚拟网络。  在下面的示例中，地址空间为 10.0.0.0/16 时，前面的方案说明中所规定。

    azure network vnet create -n AdatumAppGatewayVNET -a 10.0.0.0/16 -g AdatumAppGatewayRG -l eastus

## <a name="create-a-subnet"></a>创建子网

在创建虚拟网络之后，应用程序网关添加子网。  如果您计划使用应用程序网关所在的虚拟网络中承载 web 应用程序使用应用程序网关，请确保保留足够的空间为另一个子网。

    azure network vnet subnet create -g AdatumAppGatewayRG -n Appgatewaysubnet -v AdatumAppGatewayVNET -a 10.0.0.0/28 

## <a name="create-the-application-gateway"></a>创建应用程序网关

一旦创建了虚拟的网络和子网，已完成应用程序网关的必备。 另外先前导出的.pfx 文件证书，该证书的密码所需的以下步骤︰ 用于后端的 IP 地址是您的后端服务器的 IP 地址。 这些值可以是虚拟网络中的专用 Ip、 公用 ip，也为后端服务器的完全限定的域名。

    azure network application-gateway create -n AdatumAppGateway -l eastus -g AdatumAppGatewayRG -e AdatumAppGatewayVNET -m Appgatewaysubnet -r 134.170.185.46,134.170.188.221,134.170.185.50 -y c:\AdatumAppGateway\adatumcert.pfx -x P@ssw0rd -z 2 -a Standard_Medium -w Basic -j 443 -f Enabled -o 80 -i http -b https -u Standard

> [AZURE.NOTE] 可以在运行以下命令创建过程中提供的参数的列表︰ **azure 的网络应用程序网关创建--帮助**。

本示例创建一个基本的应用程序网关使用侦听器、 后端库、 后端的 http 设置和规则的默认设置。 它还配置了 SSL 卸载。 您可以修改这些设置，以适合您的部署设置后成功。
如果您已经有 web 应用程序定义的后端池前面的步骤中，创建后，负载平衡开始。

## <a name="next-steps"></a>下一步行动

了解如何通过[创建自定义运行状况探测器](application-gateway-create-probe-portal.md)访问创建自定义运行状况探测器

了解如何配置 SSL 卸载并采取代价高昂的 SSL 解密，关闭您的 web 服务器访问[配置 SSL 卸载](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png