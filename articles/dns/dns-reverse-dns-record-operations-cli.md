<properties
   pageTitle="管理 Azure 服务使用 Azure CLI 的反向 DNS 记录 |Microsoft Azure"
   description="如何管理反向 DNS 记录或资源管理器中使用 Azure CLI 的 Azure 服务的 PTR 记录"
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-using-the-azure-cli"></a>如何管理 Azure 服务使用 Azure CLI 的反向 DNS 记录

[AZURE.INCLUDE [DNS-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][经典的部署模型](dns-reverse-dns-record-operations-classic-ps.md)。

## <a name="validation-of-reverse-dns-records"></a>验证的反向 DNS 记录
为确保第三方无法创建反向映射到您的 DNS 域的 DNS 记录，Azure 只允许创建反向 DNS 记录以下任一项为真︰

- "ReverseFqdn"等同为"Fqdn"公用 IP 地址资源，它已指定或在相同的订阅任何公共 IP 地址的"Fqdn"例如，"ReverseFqdn""contosoapp1.northus.cloudapp.azure.com"。。

- "ReverseFqdn"转发解析为名称或为其指定了，或任何公共 IP 地址"Fqdn"或 IP 的公共 IP 地址的 IP 在相同的订阅中如"ReverseFqdn"是"app1.contoso.com"。 这是一个 CName 别名为"contosoapp1.northus.cloudapp.azure.com"。

在一个公共 IP 地址的反向 DNS 属性设置或修改时只执行验证检查。 不执行定期重新验证。

## <a name="add-reverse-dns-to-existing-public-ip-addresses"></a>添加到现有的公用 IP 地址的反向 DNS
您可以向现有公用 IP 地址使用 azure 网络公共 ip 组添加反向 DNS:

    azure network public-ip set -n PublicIp -g NRP-DemoRG-PS -f contosoapp1.westus.cloudapp.azure.com.

如果您想要添加到不已经有一个 DNS 名称现有的公用 IP 地址的反向 DNS，您还必须指定一个 DNS 名称。 您可以添加使用 azure 网络公共 ip 组为此︰

    azure network public-ip set -n PublicIp -g NRP-DemoRG-PS -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.

## <a name="create-a-public-ip-address-with-reverse-dns"></a>使用反向 DNS 创建一个公用 IP 地址
您可以创建新的公用 IP 地址指定使用 azure 的网络公共 ip 地址的反向 DNS 属性︰

    azure network public-ip create -n PublicIp3 -g NRP-DemoRG-PS -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.

## <a name="view-reverse-dns-for-existing-public-ip-addresses"></a>查看现有的公用 IP 地址的反向 DNS
您可以查看使用 azure 网络公共 ip 显示现有公用 IP 地址配置的值︰

    azure network public-ip show -n PublicIp3 -g NRP-DemoRG-PS

## <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>删除现有的公用 IP 地址的反向 DNS
您可以删除使用 azure 网络公共 ip 组现有公用 IP 地址的反向 DNS 属性。 这是通过将 ReverseFqdn 属性值设置为空︰

    azure network public-ip set -n PublicIp3 -g NRP-DemoRG-PS –f “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-arm-include.md)]
