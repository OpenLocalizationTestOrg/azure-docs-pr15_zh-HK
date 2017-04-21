## <a name="how-to-create-a-vnet-using-the-azure-cli"></a>如何创建使用 Azure CLI VNet

Azure CLI 可用于在命令提示符下，运行 Windows、 Linux、 或 OSX 的任何计算机中管理 Azure 资源。 若要使用 Azure CLI 创建 VNet，请按照下面的步骤。

1. 如果您以前从未使用 Azure CLI，请参见[安装和配置 Azure CLI](../articles/xplat-cli-install.md)并按照直至您选择您的 Azure 帐户和订阅的说明进行操作。
2. 运行**azure 配置模式**命令切换到资源管理器模式，如下所示。

        azure config mode arm

    以下是上述命令的预期的输出︰

        info:    New mode is arm

3. 如果有必要，请运行**azure 组创建**来创建新的资源组，如下所示。 请注意命令的输出。 显示后输出说明中使用的参数的列表。 有关资源组的详细信息，请访问[Azure 资源管理器的概述](../articles/virtual-network/resource-group-overview.md#resource-groups)。

        azure group create -n TestRG -l centralus

    以下是上述命令的预期的输出︰

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **-n （或--名称）**。 新的资源组的名称。 对于我们的情况， *TestRG*。
    - **-l （或--位置）**。 将在其中创建新的资源组的 azure 地区。 对于我们的情况， *centralus*。

4. 运行创建 VNet 和一个子网， **azure 网络 vnet 创建**命令，如下所示。 

        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus

    以下是上述命令的预期的输出︰

        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK

    - **-g （或--资源组）**。 VNet 将创建的资源组的名称。 对于我们的情况， *TestRG*。
    - **-n （或--名称）**。 要创建 VNet 的名称。 对于我们的情况， *TestVNet*
    - **-a （或--地址前缀）**。 使用 VNet 地址空间的 CIDR 块的列表。 对于我们的情况， *192.168.0.0/16*
    - **-l （或--位置）**。 将在其中创建 VNet azure 的地区。 对于我们的情况， *centralus*。

5. 运行**azure vnet 子网创建**的命令如下所示创建子网。 请注意命令的输出。 显示后输出说明中使用的参数的列表。

        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24

    以下是上述命令的预期的输出︰

        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK

    - **-e (或名称-vnet-**。 VNet 将在其中创建子网的名称。 对于我们的情况， *TestVNet*。
    - **-n （或--名称）**。 新的子网的名称。 对于我们的情况，*前端*。
    - **-a （或--地址前缀）**。 子网 CIDR 块。 四个我们的方案、 *192.168.1.0/24*。

6. 如有必要，请重复步骤 5 上面创建其他子网。 对于我们的情况，运行以下命令创建的*后端*的子网。

        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24

4. 运行**azure 网络 vnet show**命令以查看新的 vnet，属性，如下所示。

        azure network vnet show -g TestRG -n TestVNet

    以下是上述命令的预期的输出︰

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
