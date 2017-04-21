## <a name="scenario"></a>方案

本文档将指导在特定情形中在虚拟机中使用多个 Nic 的部署。 在这种情况下，必须寄宿在 Azure 的双层 IaaS 工作负荷。 每一层是自己在虚拟网络 (VNet) 的子网中部署。 前端层由多个 web 服务器，在负载平衡的高可用性设置组合在一起。 由多个数据库服务器的后端层。 这些数据库服务器将使用两个 Nic，另一个用于数据库访问，另一个用于管理部署。 该方案还包括网络安全组 (NSGs) 控制到每个子网，允许哪种通信和 NIC 部署中。 下图显示了此方案中的基本体系结构。  

![MultiNIC 方案](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

