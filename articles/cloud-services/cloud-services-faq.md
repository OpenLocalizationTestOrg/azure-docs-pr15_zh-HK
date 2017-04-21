<properties
    pageTitle="云服务常见问题 |Microsoft Azure"
    description="云服务有关的常见问题。"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="adegeo"/>

# <a name="cloud-services-faq"></a>云服务常见问题解答
本文解答一些有关 Microsoft Azure 云服务的常见问题。 您也可以访问[Azure 支持常见问题](http://go.microsoft.com/fwlink/?LinkID=185083)Azure 的一般定价和支持信息。 大小信息，您还可以参考[云服务虚拟机大小的页面](cloud-services-sizes-specs.md)。

## <a name="certificates"></a>证书

### <a name="where-should-i-install-my-certificate"></a>其中应安装我的证书？

- **我**  
应用程序使用私钥的证书 (\*.pfx， \*.p12)。

- **CA**  
在此存储区中 （策略和子 Ca） 转所有中间证书。

- **根**  
根 CA 存储，因此您的主根 CA 证书应在此处。

### <a name="i-cant-remove-expired-certificate"></a>不能删除已过期的证书

Azure 防止在使用时移除证书。 您需要删除使用该证书，部署或更新该部署与不同或已续订的证书。

### <a name="delete-an-expired-certificate"></a>删除过期的证书

只要证书不在使用中，您可以使用[删除 AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell cmdlet 移除证书。

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>我已过期的证书扩展命名 Windows Azure 服务管理

只要到云服务，例如远程桌面扩展添加了扩展名，则会创建这些证书。 这些证书仅用于加密和解密私有扩展的配置。 如果这些证书过期并不重要。 不检查到期日期。

### <a name="certificates-i-have-deleted-keep-reappearing"></a>我已删除的证书重复出现

这些保留最有可能出现由于您正在使用的例如 Visual Studio 工具。 每当您重新连接所使用的证书的一种工具，它将再次被上载到 Azure。

### <a name="my-certificates-keep-disappearing"></a>证书将消失

当循环使用的虚拟机实例时，本地的所有更改都都将丢失。 使用[启动任务](cloud-services-startup-tasks.md)每次启动时该角色到虚拟机中安装证书。

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>在门户网站中找不到我管理证书

[管理证书](..\azure-api-management-certs.md)才 Azure 经典门户中可用。 当前的 Azure 门户网站不使用管理证书。 

### <a name="how-can-i-disable-a-management-certificate"></a>如何禁用管理证书？

[管理证书](..\azure-api-management-certs.md)不能被禁用。 您删除它们通过 Azure 经典门户时您不希望它们不再使用。

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>如何创建 SSL 证书对于特定的 IP 地址？

按照[创建证书教程](cloud-services-certs-create.md)中的说明进行操作。 使用作为 DNS 名称的 IP 地址。

## <a name="security"></a>安全

### <a name="disable-ssl-30"></a>禁用 SSL 3.0

要禁用 SSL 3.0 并使用 TLS 安全性，创建记录启动任务在此博客文章︰ https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

## <a name="scale-a-cloud-service"></a>云服务的比例

### <a name="i-cannot-scale-beyond-x-instances"></a>我不能扩展到 X 实例

Azure 订阅的内核可以使用的数量上的限制。 如果您已使用所有可用的内核，缩放将无法工作。 例如，如果您有 100 内核的限制，这意味着造成 100 大的 A1 虚拟机实例的云服务，或 50 A2 调整虚拟机实例。

## <a name="troubleshooting"></a>故障排除

### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>我不能预留 IP 多 VIP 云服务中

首先，请确保您要保留的 IP 虚拟机实例已打开。 第二，请确保您正在使用保留 IPs 烦躁的临时和生产部署。 升级部署的同时，**不能**更改的设置。

