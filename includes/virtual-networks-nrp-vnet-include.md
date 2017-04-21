## <a name="virtual-network"></a>虚拟网络
虚拟网络 (VNET) 和子网资源可帮助定义 Azure 中正在运行的工作负荷的安全边界。 VNet 被表征的地址空间，定义为 CIDR 块的集合。 

>[AZURE.NOTE] 网络管理员必须熟悉使用 CIDR 表示法。 如果您不熟悉 CIDR，[更了解它](http://whatismyipaddress.com/cidr)。

![有多个子网 VNet](./media/resource-groups-networking/Figure4.png)

VNets 包含以下属性。

|属性|说明|示例值|
|---|---|---|
|**addressSpace**|地址前缀 CIDR 表示法在 VNet 构成的集合|192.168.0.0/16|
|**子网**|子网构成的 VNet 集合|请参阅[子网](#Subnets)下面。|
|**ip 地址**|IP 地址分配给对象。 这是只读属性。|104.42.233.77|

### <a name="subnets"></a>子网
子网是 VNet 的子资源，可帮助定义段内的 CIDR 块，使用 IP 地址前缀的地址空间。 可以添加到子网，并连接到虚拟机，针对各种工作负载提供了连接 Nic。

子网包含以下属性。 

|属性|说明|示例值|
|---|---|---|
|**addressPrefix**|单个构成 CIDR 表示法中的子网的地址前缀|192.168.1.0/24|
|**networkSecurityGroup**|NSG 应用到子网|请参见[NSGs](#Network-Security-Group)|
|**routeTable**|应用到子网的路由表|请参阅[UDR](#Route-table)|
|**ipConfigurations**|使用 Nic 连接到子网的 IP configruation 对象的集合|请参阅[UDR](#Route-table)|


以 JSON 格式的示例 VNet:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>其他资源

- 获取有关[VNet](../articles/virtual-network/virtual-networks-overview.md)的详细信息。
- 阅读 VNets [REST API 参考文档](https://msdn.microsoft.com/library/azure/mt163650.aspx)。
- 阅读为子网的[REST API 参考文档](https://msdn.microsoft.com/library/azure/mt163618.aspx)。