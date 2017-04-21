<properties
     pageTitle="使用 Azure CDN |Microsoft Azure"
     description="本主题演示如何启用 Azure 内容传递网络 (CDN)。 本教程介绍创建新的 CDN 配置文件和终结点。"
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="07/28/2016" 
     ms.author="casoper"/>

# <a name="using-azure-cdn"></a>使用 Azure CDN  

本主题介绍使 Azure CDN 通过创建一个新的 CDN 的配置文件和终结点。

>[AZURE.IMPORTANT] 有关的简介 CDN 的工作原理，以及列表的功能，请参阅[CDN 概述](./cdn-overview.md)。

## <a name="create-a-new-cdn-profile"></a>创建新的 CDN 配置文件

CDN 配置文件是一套 CDN 的终结点。  每个配置文件包含一个或多个 CDN 终结点。  您可能希望使用多个配置文件来组织您的 internet 域，web 应用程序或一些其他条件的 CDN 终结点。

> [AZURE.NOTE] 默认情况下，一个 Azure 订阅仅限于八 CDN 配置文件。 每个 CDN 配置文件仅限于 10 个 CDN 的终结点。
>
> 在 CDN 的配置文件级别应用 CDN 定价。 如果您想要的 Azure CDN 定价层混合使用，需要多个 CDN 配置文件。

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>创建新的 CDN 终结点

**若要创建新的 CDN 终结点**

1. 在[Azure 门户网站](https://portal.azure.com)中，导航到您 CDN 的配置文件。  您可能已经固定其到上一步中的仪表板。  如果您没有，您可以发现它通过单击**浏览**，然后**CDN 的配置文件**，然后单击要添加到配置文件。

    CDN 配置文件刀片式服务器出现。

    ![CDN 的配置文件][cdn-profile-settings]

2. 单击**添加终结点**按钮。

    ![添加终结点按钮][cdn-new-endpoint-button]

    此时将显示**添加终结点**刀片式服务器。

    ![添加终结点刀片式服务器][cdn-add-endpoint]

3. 输入此 CDN 终结点的**名称**。  此名称将用于访问您在域的缓存的资源`<endpointname>.azureedge.net`。

4. 在**源类型**下拉列表中选择源类型。  选择 Azure 存储帐户**存储**、**云服务**的 Azure 云服务、 **Web 应用程序**（承载 Azure 中或其他地方） 任何其他可公开访问的 web 服务器来源的 Azure Web 应用程序中，或**自定义原点**。

    ![CDN 来源类型](./media/cdn-create-new-endpoint/cdn-origin-type.png)
        
5. 在**来源主机名**下拉列表中选择或键入原始域。  下拉列表中将列出您在步骤 4 中指定的类型的所有可用的来源。  如果您选择作为**原始键入**您的*自定义源*，将在您的自定义源的域中进行键入。

6. 在**源路径**文本框中，输入要缓存，或保留为空以允许缓存在步骤 5 中指定的域的任何资源的资源的路径。

7. 在**原始主机标头**中，输入所需的 CDN 随每个请求，发送主机标头或保留默认名称。

    > [AZURE.WARNING] 某些类型的来源，如 Azure 存储和 Web 应用程序需要以匹配原始域的主机标头。 除非您有需要不同于其域的主机标头一个原点，应保留默认值。

8. 对于**协议**和**源端口**，指定协议和端口用来访问您的资源在原点。  必须选择至少一个协议 （HTTP 或 HTTPS）。
    
    > [AZURE.NOTE] **源端口**只会影响哪些端口用于检索信息从原点的终结点。  本身的终结点将只可用于将客户端上的默认 HTTP 和 HTTPS 端口 （80 和 443） 任何**源端口**。  
    >
    > **Akamai 从 azure CDN**终结点不允许来源的完整 TCP 端口范围。  不允许的源端口的列表，请参阅[从 Akamai 允许的源端口的 Azure CDN](https://msdn.microsoft.com/library/mt757337.aspx)。  
    >
    > 内容使用 HTTPS 访问 CDN 具有以下限制︰
    > 
    > - 您必须使用提供的 CDN 的 SSL 证书。 不支持第三方证书。
    > - 您必须使用 CDN 提供域 (`<endpointname>.azureedge.net`) HTTPS 访问内容。 由于 CDN 此时不支持自定义证书 HTTPS 支持不适用于自定义域名 (Cname)。

9. 单击**添加**按钮来创建新的终结点。

10. 创建终结点后，它的终结点配置文件列表中显示。 列表视图显示用于访问缓存的内容，以及原始域的 URL。

    ![CDN 终结点][cdn-endpoint-success]

    > [AZURE.IMPORTANT] 终结点未立即将可供使用，因为要花时间传播通过 CDN 的登记。  对于<b>Azure CDN Akamai 从</b>配置文件中，传播通常在一分钟内完成。  <b>Azure CDN Verizon 从</b>配置文件中，传播通常会在 90 分钟内完成，但在某些情况下可能需要更长的时间。
    >    
    > 用户尝试使用 CDN 的域名称的终结点配置已传播到 Pop 之前将收到 HTTP 404 响应代码。  如果它已被几个小时，因为创建终结点，并且您仍然还收到 404 响应，请参阅[疑难解答 CDN 终结点返回 404 状态](cdn-troubleshoot-endpoint.md)。


##<a name="see-also"></a>请参见
- [控制缓存行为与查询字符串的请求](cdn-query-string.md)
- [如何 CDN 的内容映射到自定义的域](cdn-map-content-to-custom-domain.md)
- [预加载 Azure CDN 终结点上的资产](cdn-preload-endpoint.md)
- [清除的 Azure CDN 终结点](cdn-purge-endpoint.md)
- [故障排除 CDN 终结点返回 404 状态](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
