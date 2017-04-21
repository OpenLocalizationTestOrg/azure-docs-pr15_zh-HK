## <a name="peering-across-subscriptions"></a>通过订阅对等

在此方案中，您将创建两个 VNets 属于不同订阅之间对等。

![跨子方案](./media/virtual-networks-create-vnetpeering-scenario-crosssub-include/figure01.PNG)

对 VNet 等依赖于基于角色的访问控制 (RBAC) 进行授权。 对于跨订阅方案，首先需要将创建对等链接的用户权限授予足够的权限︰

> [AZURE.NOTE] 如果同一用户具有权限，通过两个预订，则可以跳过下面的步骤 1 4。
