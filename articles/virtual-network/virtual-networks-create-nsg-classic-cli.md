<properties
   pageTitle="如何在经典模式下使用 Azure CLI 创建 NSGs |Microsoft Azure"
   description="了解如何创建和部署 NSGs 在经典模式下使用 Azure CLI"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>如何创建在 Azure CLI 中的 NSGs （传统）

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]本文介绍了经典的部署模型。 您还可以[创建资源管理器部署模型中的 NSGs](virtual-networks-create-nsg-arm-cli.md)。

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

下面的示例 Azure CLI 命令期望已根据上述方案创建一个简单的环境。 如果您想要运行的命令，在本文档中所显示，第一次[创建 VNet](virtual-networks-create-vnet-classic-cli.md)构建的测试环境。

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>如何创建前端网的 NSG
若要创建名为基于上述方案命名的**NSG 前端**NSG，请按照下面的步骤。

1. 如果您以前从未使用 Azure CLI，请参见[安装和配置 Azure CLI](../xplat-cli-install.md)并按照直至您选择您的 Azure 帐户和订阅的说明进行操作。

2. 运行**`azure config mode`**命令来切换到经典模式，如下所示。

        azure config mode asm

    预期的输出︰

        info:    New mode is asm

3. 运行**`azure network nsg create`**命令创建 NSG。

        azure network nsg create -l uswest -n NSG-FrontEnd

    预期的输出︰

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    参数︰

    - **-l （或--位置）**。 将在其中创建新的 NSG azure 的地区。 对于我们的情况， *westus*。
    - **-n （或--名称）**。 新的 NSG 的名称。 对于我们的情况， *NSG 前端*。

4. 运行**`azure network nsg rule create`**命令来创建一个规则来允许从 Internet 访问端口 3389 (RDP)。

        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    预期的输出︰

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    参数︰

    - **-a （或-nsg 名称）**。 NSG 将在其中创建规则的名称。 对于我们的情况， *NSG 前端*。
    - **-n （或--名称）**。 新规则的名称。 对于我们的情况， *rdp 规则*。
    - **-c （或--操作）**。 规则 （拒绝或允许） 的访问级别。
    - **-p （或--协议）**。 协议 (Tcp、 Udp 或 *) 的规则。
    - **-r （或--类型）**。 连接 （入站或出站） 的方向。
    - **-y （或--优先级）**。 该规则的优先级。
    - **-f （或--源地址前缀）**。 在 CIDR 或使用默认的标记的源地址前缀。
    - **-o （或--源端口范围）**。 源端口或端口范围。
    - **-e （或--目标地址前缀）**。 CIDR 或使用默认的标记中的目标地址前缀。
    - **-u （或--目标端口范围）**。 目标端口或端口范围。

5. 运行**`azure network nsg rule create`**命令来创建一个规则来允许从 Internet 访问端口 80 (HTTP)。

        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    预期的 putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. 运行**`azure network nsg subnet add`**命令链接到前端的子网的 NSG。

        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd

    预期的输出︰

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>如何创建后端网的 NSG
若要创建 NSG 命名命名的*NSG 后端*根据上述方案，请按照下面的步骤。

3. 运行**`azure network nsg create`**命令创建 NSG。

        azure network nsg create -l uswest -n NSG-BackEnd

    预期的输出︰

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    参数︰

    - **-l （或--位置）**。 将在其中创建新的 NSG azure 的地区。 对于我们的情况， *westus*。
    - **-n （或--名称）**。 新的 NSG 的名称。 对于我们的情况， *NSG 前端*。

4. 运行**`azure network nsg rule create`**命令来创建一个规则来允许从前端子网访问端口 1433 (SQL)。

        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    预期的输出︰

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK


5. 运行**`azure network nsg rule create`**命令来创建一个规则来拒绝对 Internet 的访问。

        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80

    预期的 putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. 运行**`azure network nsg subnet add`**命令链接到后端的子网的 NSG。

        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd

    预期的输出︰

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK
