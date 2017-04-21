<properties
  pageTitle="与其他 Azure 服务使用 Azure DNS |Microsoft Azure"
  description="了解如何使用 Azure DNS 来解析为其他 Azure 服务名称"
  services="dns"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure dns"
/>
<tags
  ms.service="dns"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="09/21/2016"
  ms.author="sewhee"
/>

# <a name="using-azure-dns-with-other-azure-services"></a>与其他 Azure 服务使用 Azure DNS

Azure DNS 是承载的 DNS 管理和名称解析服务。 这使您可以在 Azure 创建的其他应用程序和已部署的服务的公共 DNS 名称。 您自定义的域中创建一个名称为 Azure 服务被添加为您的服务的类型正确的记录一样简单。

* 对于动态分配的 IP 地址，您必须创建映射到 Azure 服务创建的 DNS 名称的 DNS CNAME 记录。 DNS 标准防止区域 apex 使用 CNAME 记录。
* 对于静态分配的 IP 地址，您可以创建使用任何名称，包括在区域 apex_裸_域名的 DNS A 记录。

下表列出了可用于各种 Azure 服务的受支持的记录类型。 从下表可以看出，随着 Azure DNS 仅面向 Internet 的网络资源支持 DNS 记录。 Azure DNS 不能用于内部、 专用地址的名称解析。

| Azure 服务 | 网络接口 | 说明 |
|---------------|-------------------|-------------|
| 应用程序网关 | 前端的公用 IP | 您可以创建一个 DNS A 或 CNAME 记录。 |
| 负载平衡器 | 前端的公用 IP | 您可以创建一个 DNS A 或 CNAME 记录。 负载平衡器可以有动态分配 IPv6 公用 IP 地址。 因此，您必须创建 IPv6 地址的 CNAME 记录。 |
| 通信管理器 | 公用名称 | 您只能创建 CNAME 映射到 trafficmanager.net 名称分配给您的流量管理器配置文件。 有关详细信息，请参阅[如何流量管理器的工作方式](../traffic-manager/traffic-manager-how-traffic-manager-works.md#traffic-manager-example)。 |
| 云服务 | 公用 IP | 对于静态分配的 IP 地址，您可以创建一个 DNS A 记录。 对于动态分配的 IP 地址，必须创建映射到_cloudapp.net_名的 CNAME 记录。 此规则适用于传统门户中创建，因为它们作为云服务进行部署的虚拟机。 有关详细信息，请参阅[配置自定义域名在云服务中](../cloud-services/cloud-services-custom-domain-name-portal.md)。 |
| 应用程序服务 | 外部 IP | 对于外部 IP 地址，您可以创建一个 DNS A 记录。 否则，您必须创建映射到 azurewebsites.net 名的 CNAME 记录。 详细信息，请参阅[自定义域名到 Azure 应用程序映射](../app-service-web/web-sites-custom-domain-name.md) |
| 资源管理器的虚拟机 | 公用 IP | 资源管理器的虚拟机可以具有公用 IP 地址。 负载平衡器后面，也可能是一个公用 IP 地址的虚拟机。 您可以创建的公用地址的 DNS A 或 CNAME 记录。 此自定义名称可用于绕过在负载平衡器的 VIP。 |
| 经典的虚拟机 | 公用 IP | 经典的虚拟机创建使用 PowerShell 或 CLI 可以用动态或静态 （保留） 的虚拟地址。 可以分别创建一个 DNS CNAME 记录或记录。 |
