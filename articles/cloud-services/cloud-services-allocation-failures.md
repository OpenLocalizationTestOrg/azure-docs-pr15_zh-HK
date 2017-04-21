<properties
    pageTitle="云服务分配失败疑难解答 |Microsoft Azure"
    description="当您部署在 Azure 的云服务疑难解答分配失败"
    services="azure-service-management, cloud-services"
    documentationCenter=""
    authors="simonxjx"
    manager="felixwu"
    editor=""
    tags="top-support-issue"/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-six"/>



# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>当您部署在 Azure 的云服务疑难解答分配失败

## <a name="summary"></a>摘要
当您部署到云服务的实例，或添加新的 web 或辅助角色实例时，Microsoft Azure 将分配计算资源。 有时，甚至达到 Azure 订阅限制之前执行这些操作时，可能会收到错误。 本文介绍了一些常见的分配失败的原因，并建议可能的补救措施。 当您规划您的服务的配置信息可能也会很有用。

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>背景-分配的工作原理
在 Azure 数据中心中的服务器进行分区到群集。 在多个群集中尝试新的云服务分配请求。 向云服务 （在临时或生产） 时，云服务部署的第一个实例时获取固定到一个群集。 云服务的任何进一步部署将发生在同一个群集。 在本文中，我们会称这为"固定到群集"。 下面的图 1 显示了其尝试在多个群集; 普通分配的情况图 2 说明了分配的情况，因为这是现有的云服务 CS_1 位于具有固定到群集 2。

![分配关系图](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>为什么会发生分配失败
分配请求被固定到一个群集，时更好地发现免费的资源，因为可用的资源池仅限于群集失败。 此外，如果分配请求被固定到群集，但是该群集不支持所请求的资源的类型，您的请求将失败，即使群集有免费的资源。 下面的图 3 说明位置固定的分配将失败，因为唯一候选群集没有释放资源的情况。 图 4 所示位置固定的分配将失败，因为唯一候选群集不支持所请求的 VM 大小，即使群集具有可用资源的情况。

![固定的分配失败](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>云服务的分配失败疑难解答
### <a name="error-message"></a>错误消息
您可能会看到以下错误消息︰

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>常见的问题
以下是导致分配请求要被固定到一个群集的常见的分配情况。

- 部署到临时插槽-如果云服务已经部署在任何一个插槽中，然后在整个云服务已固定到特定群集。  这意味着，如果已部署存在生产插槽中，然后新的临时部署可以只能分配到同一群集作为生产插槽中。 如果群集已接近容量，请求可能失败。

- 缩放-将新实例添加到现有的云服务必须在同一个群集中分配。  通常可分配小扩展请求，但不是总是。 如果群集已接近容量，请求可能失败。

- 相似性组-新部署到空云服务可以通过分配结构在任何群集中在该地区，除非云服务被固定到一个关联组。 在同一个群集会尝试部署到相同的关联组。 如果群集已接近容量，请求可能失败。

- 相似性组 vNet-旧的虚拟网络绑定到关联组而不是区域，并在这些虚拟的网络中的云服务将固定到相似性组群集。 将固定在群集上尝试部署这种类型的虚拟网络。 如果群集已接近容量，请求可能失败。

## <a name="solutions"></a>解决方案

1. 重新部署到新的云服务 — 此解决方案将可能是最成功的因为它允许从该区域中的所有群集中选择的平台。

    - 部署到新的云服务的工作负荷  

    - 更新 CNAME 记录使其指向新的云服务通信

    - 一旦零流量进入旧的网站，就可以删除旧的云服务。 此解决方案应该会导致停机时间为零。

2. 删除生产和暂存槽 — 此解决方案将保留现有的 DNS 名称，但是将导致您的应用程序的停机时间。

    - 删除生产和转移现有的云服务的插槽以便云服务是空的然后再

    - 现有的云服务中创建新的部署。 这将重新尝试为在该区域中的所有群集上的分配。 确保云服务没有绑定到关联组。

3. 保留的 IP-该解决方案将保留您现有的 IP 地址，但会导致您的应用程序的停机时间。  

    - 创建现有部署使用 Powershell ReservedIP

    ```
    New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
    ```

    - 按照之外，确保服务的 CSCFG 中指定新的 ReservedIP #2。

4. 删除关系组为新部署的好友小组不能建议使用。 按照上述部署新的云服务的 #1 的步骤。 确保云服务未在关联组。

5. 将转换为区域的虚拟网络-请参阅[如何迁移从好友小组与区域虚拟网 (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md)。
