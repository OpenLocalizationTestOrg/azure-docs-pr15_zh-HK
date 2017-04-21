<properties
    pageTitle="如何设置在 Azure API 管理 VPN 连接"
    description="学习如何设置在 Azure API 管理和访问 web 服务，它通过 VPN 连接。"
    services="api-management"
    documentationCenter=""
    authors="antonba"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="antonba"/>

# <a name="how-to-setup-vpn-connections-in-azure-api-management"></a>如何设置在 Azure API 管理 VPN 连接

API 管理对 VPN 的支持允许您连接 API 管理通往 Azure 虚拟网络 （经典）。 这使 API 管理客户安全地连接到相应的后端 web 服务内部或在其他方面不能访问公用互联网。

>[AZURE.NOTE] 与经典的 VNETs，azure API 管理协同工作。 有关创建经典 VNET 的信息，请参阅[创建虚拟网络使用 Azure 门户 （经典）](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)。 将经典的 VNETs 连接到 ARM VNETS 的信息，请参阅[连接到新的 VNets 的经典 VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md)。

## <a name="enable-vpn"></a>启用 VPN 连接

>VPN 连接才可用的**高级**人员和**开发人员**的层中。 若要切换到它， [Azure 传统门户网站][]中打开您的 API 管理服务，然后打开**缩放比例**选项卡。 在**常规**部分下选择特优层，并单击保存。

若要启用 VPN 连接，请在[Azure 经典门户][]打开 API 管理服务并切换到**配置**选项卡。 

在 VPN 部分中，切换到**上**的**VPN 连接**。

![配置 API 管理实例的选项卡][api-management-setup-vpn-configure]

现在，您将看到配置 API 管理服务时的所有地区的列表。

选择 VPN 和每个区域的子网。 Vpn 的列表填充基于 Azure 订购可在您正在配置的区域中设置的虚拟网络。

![选择 VPN][api-management-setup-vpn-select]

单击**保存**在屏幕的底部。 您将不能从 Azure 经典门户中执行其他操作的 API 管理服务，它更新时。 服务网关将保持可用，应不会影响运行时调用。

请注意的 VIP 地址的网关将更改每次启用或禁用 VPN。

## <a name="connect-vpn"></a>连接到 web 服务背后 VPN

API 管理服务连接到 VPN 访问虚拟网络中的 web 服务后，访问公共服务相比没有什么不同。 只需键入本地地址或主机名 （如果 DNS 服务器被配置为 Azure 虚拟网络） 的 web 服务**的 Web 服务 URL**字段中创建一个新的 API 时或编辑一个现有。

![从 VPN 添加 API][api-management-setup-vpn-add-api]

## <a name="required-ports-for-api-management-vpn-support"></a>API 管理 VPN 支持所需的端口

当在 VNET 中承载 API 管理服务实例时，使用下表中的端口。 如果这些端口被阻挡，则该服务可能不能正常工作。 VNET 中使用 API 管理时遇到一个或多个阻止这些端口是最常见的错误配置问题。

| 输入端口                      | 方向        | 传输协议 | 目的                                                          | 源 / 目标              |
|------------------------------|------------------|--------------------|------------------------------------------------------------------|-----------------------------------|
| 80、 443                      | 入站          | TCP                | 与 API 管理的客户端通信                           | 互联网 / VIRTUAL_NETWORK        |
| 80443                       | 出站         | TCP                | 在 Azure 存储和 Azure 服务总线上的 API 管理依赖项 | VIRTUAL_NETWORK / 互联网        |
| 1433                         | 出站         | TCP                | API 管理依赖于 SQL                               | VIRTUAL_NETWORK / 互联网        |
| 9350、 9351，9352，9353、 9354 | 出站         | TCP                | 服务总线上的 API 管理依赖项                       | VIRTUAL_NETWORK / 互联网        |
| 5671                         | 出站         | AMQP               | 日志事件中心策略的 API 管理依赖项            | VIRTUAL_NETWORK / 互联网        |
| 6381、 6382 6383             | 入站/出站 | UDP                | 在 Redis 缓存 API 管理依赖项                       | VIRTUAL_NETWORK / VIRTUAL_NETWORK |
| 445                          | 出站         | TCP                | GIT 的 Azure 的文件共享上的 API 管理依赖项            | VIRTUAL_NETWORK / 互联网        |

## <a name="custom-dns"></a>自定义 DNS 服务器安装程序

API 管理取决于大量的 Azure 服务。 当使用自定义的 DNS 服务器的位置 VNET 中承载 API 管理服务实例时，它需要能够解决那些 Azure 服务的主机名。 请自定义的 DNS 设置，按照[本](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server)指南。  

## <a name="related-content"></a>相关内容


* [使用 Azure 经典门户站点到站点 VPN 连接时创建虚拟网络][]
* [在 Azure API 管理中如何使用 API 跟踪检查呼叫][]

[api-management-setup-vpn-configure]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png
[api-management-setup-vpn-select]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[Azure 的传统门户网站]: https://manage.windowsazure.com/

[使用 Azure 经典门户站点到站点 VPN 连接时创建虚拟网络]: ../vpn-gateway/vpn-gateway-site-to-site-create.md
[在 Azure API 管理中如何使用 API 跟踪检查呼叫]: api-management-howto-api-inspector.md
