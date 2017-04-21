<properties
    pageTitle="故障排除 Azure CDN 终结点返回 404 状态 |Microsoft Azure"
    description="诊断故障与 Azure CDN 的终结点 404 响应代码。"
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
    ms.date="07/28/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>故障排除 CDN 终结点返回 404 状态

这篇文章可以帮助您解决[CDN 终结点](cdn-create-new-endpoint.md)返回 404 错误的问题。

如果您需要更多的帮助，在这篇文章中的任何一点，您可以联系的 Azure [MSDN Azure 和堆栈溢出论坛](https://azure.microsoft.com/support/forums/)上专家。 或者，您也可以文件 Azure 支持事件。 转到[Azure 支持站点](https://azure.microsoft.com/support/options/)，然后单击**获得支持**。

## <a name="symptom"></a>故障现象

您已经创建了 CDN 配置文件和终结点，但您的内容似乎在 CDN 上可用。  尝试访问您的内容通过 CDN URL 的用户收到 HTTP 404 状态码。 

## <a name="cause"></a>原因

有几个可能的原因，包括︰

- 该文件的来源不是可见的 cdn
- 终结点配置错误，导致在错误的位置中查找的 CDN
- 主机将拒绝从 CDN 的主机标头
- 该终结点还没有来得及 CDN 在整个传播

## <a name="troubleshooting-steps"></a>故障排除步骤。

> [AZURE.IMPORTANT] 创建一个 CDN 的终结点之后, 它不立即将可供使用，因为要花时间传播通过 CDN 的登记。  对于<b>Azure CDN Akamai 从</b>配置文件中，传播通常在一分钟内就完成。  <b>Azure CDN Verizon 从</b>配置文件中，传播通常会在 90 分钟内完成，但在某些情况下可能需要更长的时间。  如果您完成本文档中的步骤操作，您仍然会得到 404 响应，考虑等待几个小时来打开支持票前再次检查。

### <a name="check-the-origin-file"></a>检查原始文件

首先，我们应确保我们想要缓存的文件上提供我们的来源并且被公开访问。  做到这一点的最快方法是在私人或 Incognito 会话中打开浏览器并直接浏览到该文件。  只需键入或将该 URL 粘贴到地址框中，是否这会导致您期望的文件，请参阅。  对于此示例，我将使用 Azure 存储帐户，可以访问的已安装的文件`https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`。  正如您所看到的它成功地通过了测试。

![成功 ！](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [AZURE.WARNING] 虽然这是最快和最简单的方法来验证您的文件是公开的在您的组织中某些网络配置可为您提供此文件是公开的当它是事实，仅对用户可见的网络 （即使它位于 Azure 中） 中的视觉效果。  如果有外部浏览器从中可以测试，如移动设备未连接到组织的网络或虚拟机在 Azure，这将是最佳。

### <a name="check-the-origin-settings"></a>检查原始设置

现在，我们已经确认该文件是在 internet 上公开，我们应确认我们原点的设置。  在[Azure 门户](https://portal.azure.com)中，浏览到您 CDN 的个人资料和单击您正在进行故障诊断的终结点。  在生成的**端点**刀片式服务器，单击来源。  

![终结点刀片式服务器带有突出显示的来源](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

此时将显示**原点**刀片式服务器。 

![原点刀片式服务器](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>原始类型和主机名

验证**原始类型**正确，并验证**来源主机名**。  在我的示例中， `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`，URL 的主机名部分是`cdndocdemo.blob.core.windows.net`。  如屏幕快照所示，这是正确的。  Azure 存储、 Web 应用程序和云服务起源，**来源主机**名字段是下拉列表中，所以我们不需要担心如何拼写它正确无误。  但是，如果您使用自定义的原点，是*绝对关键*您的主机名拼写正确 ！

#### <a name="http-and-https-ports"></a>HTTP 和 HTTPS 端口

请单击此处的另外一点是您的**HTTP**和**HTTPS 端口**。  在大多数情况下，80 和 443 是正确的并将需要做任何更改。  但是，如果原始服务器正在侦听不同的端口，将需要此处出现。  如果您不确定，只需查看原始文件的 URL。  HTTP 和 HTTPS 规范指定为默认值的端口 80 和 443。 在我的 URL， `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`，未指定端口，因此假定默认值为 443 和我的设置是否正确。  

但是，说 URL，您前面测试的原始文件为`http://www.contoso.com:8080/file.txt`。  注意`:8080`主机名段的末尾。  将告诉浏览器使用的端口`8080`连接到 web 服务器在`www.contoso.com`，因此您需要在**HTTP 端口**字段中输入 8080。  请务必注意，这些端口设置仅影响该终结点使用检索信息从原点哪些端口。

> [AZURE.NOTE] **Akamai 从 azure CDN**终结点不允许来源的完整 TCP 端口范围。  不允许的源端口的列表，请参阅[从 Akamai 允许的源端口的 Azure CDN](https://msdn.microsoft.com/library/mt757337.aspx)。  
  
### <a name="check-the-endpoint-settings"></a>检查终结点设置

在**终结点**刀片式服务器，请单击**配置**按钮。

![终结点刀片与突出显示配置按钮](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

终结点的**配置**刀片出现。

![将刀片式服务器配置](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>协议

**协议**，请验证选择了由客户端所使用的协议。  客户端使用相同的协议将是一种用于访问源，因此务必要有正确配置一节中的源端口。  终结点仅监听默认 HTTP 和 HTTPS 端口 （80 和 443)，无论来源端口。

让我们回到我们证实与`http://www.contoso.com:8080/file.txt`。  如您能够记起，Contoso 指定`8080`为其 HTTP 端口，但我们还假定它们指定`44300`作为他们 HTTPS 端口。  如果它们创建终结点命名为`contoso`，将其 CDN 端点主机名`contoso.azureedge.net`。  请求的`http://contoso.azureedge.net/file.txt`是 HTTP 请求，因此终结点将使用端口 8080 上 HTTP 来源检索出来。  安全请求通过 HTTPS， `https://contoso.azureedge.net/file.txt`，将导致终结点为 44300 端口上使用 HTTPS 时 retriving 文件的来源。

#### <a name="origin-host-header"></a>原始主机标头

**原始主机标头**是主机标头值发送到每个请求的来源。  在大多数情况下，这应该是我们前面验证**来源主机名**相同。  在此字段中不正确的值通常不会造成 404 状态，但可能会导致其他 4xx 状态，这取决于原点的期望。

#### <a name="origin-path"></a>原始路径

最后，我们应确认我们的**原始路径**。  默认情况下为空。  如果您想要您想要在 CDN 上提供的原点承载的资源的范围缩小，只应使用此字段。  

例如，在我终结点，我希望所有资源上存储帐户都可用，所以我保留**原始路径**为空。  这意味着，对请求`https://cdndocdemo.azureedge.net/publicblob/lorem.txt`从我终结点的连接会导致`cdndocdemo.core.windows.net`请求`/publicblob/lorem.txt`。  同样，对于一个请求`https://cdndocdemo.azureedge.net/donotcache/status.png`终结点请求会导致`/donotcache/status.png`从原点。

但是，如果我不想在我的原点上的每个路径使用 CDN 呢？  说我只想公开`publicblob`路径。  如果我在我**原始路径**字段中输入*/publicblob* ，这将导致插入*/publicblob*之前对源所做的每个请求的端点。  这意味着对于请求`https://cdndocdemo.azureedge.net/publicblob/lorem.txt`现在实际取得的 URL 的请求部分`/publicblob/lorem.txt`，并附加`/publicblob`开头。 这将导致请求`/publicblob/publicblob/lorem.txt`从原点。  如果该路径不能解决实际的文件，原点会返回 404 状态。  实际上是正确的 URL 来检索在此示例中的 lorem.txt `https://cdndocdemo.azureedge.net/lorem.txt`。  请注意，我们不要包含*/publicblob*路径，因为 URL 请求部分是`/lorem.txt`，并终结点添加`/publicblob`，导致`/publicblob/lorem.txt`正在请求传递到原点。
