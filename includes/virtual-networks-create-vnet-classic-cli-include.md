## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>如何创建使用 Azure CLI 经典 VNet

Azure CLI 可用于在命令提示符下，运行 Windows、 Linux、 或 OSX 的任何计算机中管理 Azure 资源。 若要使用 Azure CLI 创建 VNet，请按照下面的步骤。

1. 如果您以前从未使用 Azure CLI，请参见[安装和配置 Azure CLI](../articles/xplat-cli-install.md)并按照直至您选择您的 Azure 帐户和订阅的说明进行操作。
2. 运行创建 VNet 和一个子网， **azure 网络 vnet 创建**命令，如下所示。 显示后输出说明中使用的参数的列表。

            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    
    预期的输出︰

            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    - **-vnet**。 要创建 VNet 的名称。 对于我们的情况， *TestVNet*
    - **-e （或--地址空间）**。 VNet 的地址空间。 对于我们的情况， *192.168.0.0*
    - **-i （或-cidr）**。 CIDR 格式的网络掩码。 对于我们的情况， *16*。
    - **-n (或--子网名称**)。 第一个子网的名称。 对于我们的情况，*前端*。
    - **-p （或--开始-ip 子网）**。 子网或子网地址空间的起始 IP 地址。 对于我们的情况， *192.168.1.0*。
    - **-r （或--子网 cidr）**。 CIDR 格式的子网的网络掩码。 对于我们的情况， *24*。
    - **-l （或--位置）**。 将在其中创建 VNet azure 的地区。 对于我们的情况，*美国中部*。

3. 运行**azure vnet 子网创建**的命令如下所示创建子网。 显示后输出说明中使用的参数的列表。

            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    
    以下是上述命令的预期的输出︰

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    - **-t (或名称-vnet-**。 VNet 将在其中创建子网的名称。 对于我们的情况， *TestVNet*。
    - **-n （或--名称）**。 新的子网的名称。 对于我们的情况*后, 端*。
    - **-a （或--地址前缀）**。 子网 CIDR 块。 四个我们的方案、 *192.168.2.0/24*。

4. 运行**azure 网络 vnet show**命令以查看新的 vnet，属性，如下所示。

            azure network vnet show

    以下是上述命令的预期的输出︰

            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK
