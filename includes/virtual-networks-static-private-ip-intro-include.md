您的 IaaS 虚拟机 (Vm) 和虚拟网络中的 PaaS 角色实例自动接收一个专用的 IP 地址从您指定的用户连接到子网的范围。 该地址由 Vm 和角色实例保留，直到他们被分离。 通过禁止它 PowerShell，Azure CLI 或 Azure 门户中退役的 VM 或角色实例。 在这些情况下，一旦该虚拟机或角色实例会再次启动，它将接收可用的 IP 地址从 Azure 的基础结构，这可能不是它以前相同。 如果您关闭从来宾操作系统的虚拟机或角色实例，它将保留它的 IP 地址。  

在某些情况下，您希望具有静态 IP 地址，例如，如果您的虚拟机将运行 DNS 或将域控制器虚拟机或角色实例。 您可以通过设置专用的静态 IP 地址来这样做。