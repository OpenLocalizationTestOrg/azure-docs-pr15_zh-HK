## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>通过使用 Azure CLI 部署 ARM 模板

若要部署使用 Azure CLI 的 ARM 模板，请按照下面的步骤。

1. 如果您以前从未使用 Azure CLI，请参见[安装和配置 Azure CLI](../articles/xplat-cli-install.md)并按照直至您选择您的 Azure 帐户和订阅的说明进行操作。
2. 运行**`azure config mode`**命令来切换到资源管理器模式，如下所示。

        azure config mode arm

    以下是上述命令的预期的输出︰

        info:    New mode is arm

3. 如果有必要，请运行**`azure group create`**来创建新的资源组，如下所示。 请注意命令的输出。 显示后输出说明中使用的参数的列表。 有关资源组的详细信息，请访问[Azure 资源管理器的概述](../articles/resource-group-overview.md)。

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

4. 运行**`azure group deployment create`**cmdlet 使用模板和参数来部署新的 VNet 文件下载和修改上面。 显示后输出说明中使用的参数的列表。

        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

    以下是上述命令的预期的输出︰

        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK

    - **-g （或--资源组）**。 将在中创建新的 VNet 的资源组的名称。
    - **-f （或--模板文件）**。 ARM 模板文件的路径。
    - **-e （或--参数文件）**。 对 ARM 参数文件的路径。

5. 运行**`azure network vnet show`**命令来查看属性的新的 vnet，如下所示。

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
