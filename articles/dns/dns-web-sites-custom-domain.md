<properties
   pageTitle="创建自定义的 DNS 记录的 web 应用程序 |Microsoft Azure  "
   description="如何创建 web 应用程序使用 Azure DNS 的 DNS 记录的自定义的域。"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>为自定义的域中创建一个 web 应用程序的 DNS 记录

您可以使用 Azure DNS 承载的 web 应用程序自定义的域。 例如，您正在创建一个 Azure 的 web 应用程序，您希望用户可通过以下任一方式使用 FQDN contoso.com 或 www.contoso.com。

若要执行此操作，必须创建两个记录︰

- 一个指向 contoso.com 的根"A"记录
- 指向一个记录 www 名为"CNAME"记录

请记住，如果您在 Azure 创建 web 应用程序的 A 记录，A 记录必须手动更新如果底层 IP 地址的 web 应用程序更改。

## <a name="before-you-begin"></a>在开始之前

在开始之前，必须首先在 Azure DNS 创建 DNS 区域并在注册到 Azure DNS 委派区域。

1. 若要创建 DNS 区域，请按照[创建 DNS 区域](dns-getstarted-create-dnszone.md)。
2. 若要委派到 Azure DNS DNS，请按照[DNS 域委派](dns-domain-delegation.md)。

在创建区域并将其委派到 Azure DNS 之后, 您可以再创建记录为您自定义的域。


## <a name="1-create-an-a-record-for-your-custom-domain"></a>1.创建 A 记录为您自定义的域

A 记录用于将一个名称映射到其 IP 地址。 下面的示例中，我们将分配@为 IPv4 地址的 A 记录︰

### <a name="step-1"></a>第 1 步

创建 A 记录和分配给变量 $rs

    $rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600

### <a name="step-2"></a>第 2 步

将 IPv4 值添加到以前创建的记录集"@"使用 $rs 变量分配。 分配的 IPv4 值将是您的 web 应用程序的 IP 地址。

若要查找 web 应用程序的 IP 地址，请按照[配置自定义域名在 Azure 应用程序服务](../web-sites-custom-domain-name.md#Find-the-virtual-IP-address)。

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address <your web app IP address>

### <a name="step-3"></a>第 3 步

将更改提交到记录集。 使用`Set-AzureRMDnsRecordSet`的更改上载到记录到 Azure DNS 设置︰

    Set-AzureRMDnsRecordSet -RecordSet $rs

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2.创建 CNAME 记录为您自定义的域

如果您的域已由 Azure DNS (请参阅[DNS 域委派](dns-domain-delegation.md)，您可以使用以下示例来创建 contoso.azurewebsites.net 的 CNAME 记录。

### <a name="step-1"></a>第 1 步

打开 PowerShell 和创建新的 CNAME 记录集指定给变量 $rs。 本示例将创建具有"生存时间"的记录集类型 CNAME 600 秒在名为"contoso.com"DNS 区域中。

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>第 2 步

创建 CNAME 记录集后，您需要创建一个别名值将指向 web 应用程序。

使用"$rs"以前分配的变量可用于以下 PowerShell 命令创建 web 应用程序 contoso.azurewebsites.net 的别名。

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>第 3 步

使用提交更改`Set-AzureRMDnsRecordSet`cmdlet:

    Set-AzureRMDnsRecordSet -RecordSet $rs

您可以验证该记录已被正确创建通过查询使用 nslookup，"www.contoso.com"，如下所示︰

    PS C:\> nslookup
    Default Server:  Default
    Address:  192.168.0.1

    > www.contoso.com
    Server:  default server
    Address:  192.168.0.1

    Non-authoritative answer:
    Name:    <instance of web app service>.cloudapp.net
    Address:  <ip of web app service>
    Aliases:  www.contoso.com
    contoso.azurewebsites.net
    <instance of web app service>.vip.azurewebsites.windows.net

## <a name="create-an-awverify-record-for-web-apps"></a>创建 web 应用程序的"awverify"记录


如果您决定为您的 web 应用程序中使用 A 记录，必须经过验证流程，以确保您自己的自定义的域。 此验证步骤是通过创建名为"awverify"的特殊 CNAME 记录。 本节适用于仅记录。


### <a name="step-1"></a>第 1 步

创建的"awverify"记录。 在下面的示例中，我们将创建 contoso.com 验证的自定义的域所有权的"aweverify"记录。

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "awverify" -RecordType "CNAME" -Ttl 600

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>第 2 步

创建记录集"awverify"后，将分配别名设置 CNAME 记录。 在下面的示例中，我们将分配别名设置为 awverify.contoso.azurewebsites.net 的 CNAMe 记录。

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>第 3 步

使用提交更改`Set-AzureRMDnsRecordSet cmdlet`，以下命令中所示。

    Set-AzureRMDnsRecordSet -RecordSet $rs



## <a name="next-steps"></a>下一步行动

按照[配置应用程序服务的自定义域名称](../app-service-web/web-sites-custom-domain-name.md)来配置您的 web 应用程序使用自定义的域中的步骤。








