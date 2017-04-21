<properties
    pageTitle="诊断文件压缩设置 Azure CDN |Microsoft Azure"
    description="解决 Azure CDN 文件压缩的问题。"
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-file-compression"></a>故障排除 CDN 文件压缩

这篇文章可以帮助您解决[CDN 文件压缩](cdn-improve-performance.md)的问题。

如果您需要更多的帮助，在这篇文章中的任何一点，您可以联系的 Azure [MSDN Azure 和堆栈溢出论坛](https://azure.microsoft.com/support/forums/)上专家。 或者，您也可以文件 Azure 支持事件。 转到[Azure 支持站点](https://azure.microsoft.com/support/options/)并单击**获得支持**。

## <a name="symptom"></a>故障现象

终结点为压缩已启用，但返回的文件进行解压缩。

>[AZURE.TIP] 若要检查是否您返回的文件被压缩，您需要使用一个工具， [Fiddler](http://www.telerik.com/fiddler)或浏览器的[开发人员工具](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)等。  检查 HTTP 响应标头返回，且您的缓存 CDN 内容。  如果没有标题名为`Content-Encoding`值的**gzip**， **bzip2**，或**地道**时，压缩您的内容。
>
>![内容编码标头](./media/cdn-troubleshoot-compression/cdn-content-header.png)

## <a name="cause"></a>原因

有几个可能的原因，包括︰

- 所请求的内容不适用于压缩。
- 请求的文件类型未启用压缩。
- 在 HTTP 请求中未包括请求有效的压缩类型的标头。

## <a name="troubleshooting-steps"></a>故障排除步骤。

> [AZURE.TIP] 与部署新的终结点，CDN 配置更改需要一些时间，以便在网络间传播。  通常情况下，在 90 分钟内会应用更改。  如果这是首次设置了压缩 CDN 端点，您应考虑等待 1-2 个小时，以确保设置传播到 Pop 的压缩。 

### <a name="verify-the-request"></a>核实该请求

首先，我们应该做在请求快速性能检查。  可以使用浏览器的[开发人员工具](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)来查看正在进行的请求。

- 验证是否将请求发送到您的端点 URL， `<endpointname>.azureedge.net`，并不是您的起点。
- 验证请求包含**接受编码**标头，该标头的值包含**gzip**、**地道**，或**bzip2**。

> [AZURE.NOTE] **Akamai 从 azure CDN**配置文件仅支持**gzip**编码。

![CDN 请求标头](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>验证 （标准 CDN 配置文件） 的压缩设置

> [AZURE.NOTE] 如果 CDN 配置的**标准从 Verizon 的 CDN Azure**或**标准从 Akamai 的 CDN Azure**配置文件，此步骤才适用。 

导航到[Azure 门户网站](https://portal.azure.com)终结点，然后单击**配置**按钮。

- 验证启用了压缩。
- 验证的压缩格式的列表中包含要压缩内容的 MIME 类型。

![CDN 压缩设置](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>验证压缩设置 （高级 CDN 配置文件）

> [AZURE.NOTE] 如果您 CDN 的配置文件是**Verizon 从 Azure CDN 高级**配置文件，此步骤才适用。

导航到[Azure 门户网站](https://portal.azure.com)终结点，然后单击**管理**按钮。  将会打开附加的门户。  悬停在**HTTP 大**选项卡，然后悬停在**缓存设置**飞出。  单击**压缩**。 

- 验证启用了压缩。
- 验证**文件类型**列表包含以逗号分隔的列表 （不计空格） 的 MIME 类型。
- 验证的压缩格式的列表中包含要压缩内容的 MIME 类型。

![CDN 高级压缩设置](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a>验证缓存内容

> [AZURE.NOTE] 如果您 CDN 的配置文件是**Azure CDN Verizon 从**配置文件 （常用或特优），此步骤才适用。

使用浏览器的开发人员工具，检查以确保文件缓存中请求在该地区的响应标头。

- 检查**服务器**响应标头。  标题应具有格式**平台 (POP/服务器 ID)**，如下面的示例所示。
- 检查**X 缓存**响应标头。  标题应显示为**命中**。  

![CDN 响应标头](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a>验证该文件满足大小要求

> [AZURE.NOTE] 如果您 CDN 的配置文件是**Azure CDN Verizon 从**配置文件 （常用或特优），此步骤才适用。

将有资格获得压缩，文件必须符合以下尺寸要求︰

- 大于 128 个字节。
- 小于 1 MB。

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>检查源服务器上**通过**头处的请求

**通过**HTTP 标头指示 web 服务器正在将该请求传递由代理服务器。  默认情况下 Microsoft IIS web 服务器时该请求中包含**通过**头不压缩响应。  若要重写此行为，请执行以下操作︰

- **IIS 6**:[设置 HcNoCompressionForProxies ="错"在 IIS 元数据库属性](https://msdn.microsoft.com/library/ms525390.aspx)
- **IIS 7 及更高版本**: [ **noCompressionForHttp10**和**noCompressionForProxies**中的服务器配置设置](http://www.iis.net/configreference/system.webserver/httpcompression)

