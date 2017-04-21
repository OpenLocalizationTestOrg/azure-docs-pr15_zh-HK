<properties
   pageTitle="开始创建面向云服务的典型部署模型使用负载平衡器 Internet |Microsoft Azure"
   description="了解如何建立一个互联朝云服务的典型部署模型中的负载平衡器"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>开始创建 Internet 面临的云服务的负载平衡器

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]本文介绍了经典的部署模型。 您还可以[了解如何建立一个互联对称负载平衡器使用 Azure 资源管理器](load-balancer-get-started-internet-arm-cli.md)。

云服务会自动获配一个负载平衡器，可以通过服务模型进行自定义。

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>创建负载平衡器使用服务定义文件

您可以利用 Azure SDK 的.NET 2.5 来更新您的云服务。 在[服务定义](https://msdn.microsoft.com/library/azure/gg557553.aspx).csdef 文件进行云服务的终结点设置。

下面的示例演示如何配置云部署的 servicedefinition.csdef 文件︰

检查 snipet 的云部署生成的.csdef 文件，您可以看到外部终结点配置为在端口 10000、 10001 和 10002 使用 HTTP 端口。


    <ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
    <Endpoints>
        <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
        <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
        <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

        <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

        <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
           <AllocatePublicPortFrom>
              <FixedPortRange min=“10110” max=“10120“  />
           </AllocatePublicPortFrom>
        </InstanceInputEndpoint>
        <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
    </Endpoints>
    </WorkerRole>
    </ServiceDefinition>




## <a name="check-load-balancer-health-status-for-cloud-services"></a>负载平衡器的云服务运行状况检查


以下是健康探测器的一个示例︰

        <LoadBalancerProbes>
        <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
        </LoadBalancerProbes>

负载平衡器将该终结点的信息和探测器可用于查询服务的运行状况的 VM}:80/Probe.aspx 的 http://{DIP 的形式创建一个 URL 的信息。

服务检测到相同的 IP 地址从定期探测。 这是健康探测请求来自主机的节点运行虚拟机的位置。
该服务必须响应以 HTTP 200 状态代码负载平衡器假定该服务是运行状况良好。 任何其他 HTTP 状态代码 (例如 503) 直接采用轮换虚拟机。

该探测器定义还控制探测器的频率。 在我们上面的情况下，负载平衡器探测终结点每隔 5 秒钟之内。 如果没有肯定的答案收到 10 秒 （两个探测器区间），探测器假定下，和虚拟机采取轮换。 同样，如果服务是轮换和接收到的肯定应答，服务会还原为旋转向右走。 如果该服务正常和不正常之间起伏波动，负载平衡器可以决定推迟回旋转服务重新引入，直到已为大量的探测器状态良好。

检查服务定义架构[健全探测器](https://msdn.microsoft.com/library/azure/jj151530.aspx)的详细信息。

## <a name="next-steps"></a>下一步行动

[开始配置内部负载平衡器](load-balancer-get-started-ilb-arm-ps.md)

[配置负载平衡器分布模式](load-balancer-distribution-mode.md)

[配置负载平衡器的空闲 TCP 超时设置](load-balancer-tcp-idle-timeout.md)

