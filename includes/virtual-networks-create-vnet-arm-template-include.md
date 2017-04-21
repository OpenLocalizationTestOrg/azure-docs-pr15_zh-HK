## <a name="download-and-understand-the-arm-template"></a>下载并理解 ARM 模板

您可以下载从 github 创建 VNet 和两个子网的现有 ARM 模板，进行任何更改，您可能希望，并重用它。 为此，请按照下面的步骤。

1. 定位到[该示例模板页](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)。
2. 单击**azuredeploy.json**，然后再单击**原始**。
3. 将文件保存到您的计算机上的本地文件夹。
4. 如果您熟悉 ARM 的模板，则跳到步骤 7。
5. 打开刚才保存的文件并查看其内容在第 5 行中的**参数**时。 ARM 模板参数提供的值可以在部署过程中填写的占位符。

    | 参数 | 说明 |
    |---|---|
    | **位置** | 将在其中创建 VNet azure 区域 |
    | **vnetName** | 新的 VNet 的名称 |
    | **addressPrefix** | VNet，CIDR 格式的地址空间 |
    | **subnet1Name** | 第一个 VNet 的名称 |
    | **subnet1Prefix** | CIDR 块的第一个子网。 |
    | **subnet2Name** | 第二个 VNet 的名称 |
    | **subnet2Prefix** | 第二个网的 CIDR 块 |

    >[AZURE.IMPORTANT] 在 github 中维护的 ARM 模板可随时间变化。 请确保在使用前检查模板中。
    
6. 检查**资源**下的内容，并请注意以下︰

    - **类型**。 由模板创建的资源类型。 在这种情况下， **Microsoft.Network/virtualNetworks**，它代表 VNet。
    - **名称**。 资源的名称。 注意到**[parameters('vnetName')]**，表示该名称将作为输入提供由用户或参数文件在部署期间使用。
    - **属性**。 为该资源的属性的列表。 此模板使用 VNet 创建的过程的地址空间和子网属性。

7. 向后定位到[该示例模板页](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)。
8. **Azuredeploy-paremeters.json**，请单击，然后单击**原始**。
9. 将文件保存到您的计算机上的本地文件夹。
10. 打开刚才保存的文件并编辑这些参数的值。 使用下列值来部署我们的方案中所述 VNet。

        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }

11. 保存该文件。
  