## <a name="scenario"></a>方案

为了更好地说明如何创建 NSGs，该文档将使用下面的场景。

![VNet 方案](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

在这种情况下您将创建在**TestVNet**虚拟网络中，每个子网的 NSG 如下所述︰ 

- **NSG 前端**。 NSG 的前端将应用于*前端*的子网，且包含两个规则︰  
    - **rdp 规则**。 此规则将允许 RDP 通信到*前端*的子网。
    - **网站规则**。 此规则将允许 HTTP 通信量到*前端*的子网。
- **NSG 的后端**。 NSG 的后端到*后端*的子网，并包含两种规则︰ 
    - **sql 规则**。 此规则允许仅从*前端*的子网的 SQL 通信。
    - **网站规则**。 此规则将拒绝所有 internet 都绑定从*后端*的子网通信。

这些规则的组合创建类似于 DMZ 的情况下，后端的子网位置只能接收传入通信的 SQL 从前端的子网，并已不能访问 Internet，而前端网可以与 Internet 通信并接收传入 HTTP 请求。
 
