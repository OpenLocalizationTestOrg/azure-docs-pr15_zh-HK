## <a name="traffic-manager-profile"></a>通信管理器配置文件

通信管理器和子终结点资源启用 DNS 路由到终结点在 Azure 中和在 Azure 之外。 这种通信分发受路由策略方法。 通信管理器还允许终结点运行状况监视，并运行状况的终结点基于流量不适当转移。 

| 属性 | 说明 |
|---|---|
|**trafficRoutingMethod**| 可能的值为*性能*、*加权*和*优先级* | 
| **dnsConfig** | FQDN 配置文件 | 
| **协议** | *HTTP*和*HTTPS*监视协议，可能的值为|
| **端口** | 监视端口 |  
| **路径** | 监视的路径 |
| **终结点** |  终结点资源的容器 | 

### <a name="endpoint"></a>终结点 

终结点是一个流量管理器配置文件的子资源。 它表示一个服务或 web 端点通信分配到哪个用户根据流量管理器配置文件资源中配置的策略。 

| 属性 | 说明 | 
|---|---| 
| **类型** |  该类型的终结点，可能的值是*Azure 结束点*、*外部端点*和*嵌套的终结点* | 
| **targetResourceId** |  公共服务或网站终结点的 IP 地址。 这可能是 Azure 或外部端点。 | 
| **重量** | 用于通信量管理的端点权重。 | 
| **优先级** | 终结点、 用于定义故障转移操作的优先级 |

以 Json 格式的示例的流量管理器︰ 


        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }

 
## <a name="additional-resources"></a>其他资源

阅读更多信息[REST API 文档为通讯管理器](https://msdn.microsoft.com/library/azure/mt163664.aspx)。
