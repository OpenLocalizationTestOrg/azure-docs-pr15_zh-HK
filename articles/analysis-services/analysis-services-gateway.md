<properties
   pageTitle="内部数据网关 |Microsoft Azure"
   description="内部部署网关是在 Azure Analysis Services 服务器将连接到内部数据源的情况下有必要的。"
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="on-premises-data-gateway"></a>内部数据网关

内部数据网关充当一座桥梁，提供内部数据源和 Azure Analysis Services 云中的服务器之间的安全数据传输。

在您的网络中的计算机上安装了一个网关。 在 Azure 订阅每个 Azure Analysis Services 服务器必须安装一个网关。 例如，如果您有 Azure 订购的两台服务器连接到内部数据源，网关必须安装在网络中的两个不同的计算机上。

## <a name="requirements"></a>要求

**最低要求︰**

- .NET 4.5 框架
- 64 位版本的 Windows 7 / Windows Server 2008 R2 （或更高版本）

**建议︰**

- 8 核心 CPU
- 8 GB 内存
- 64 位版本的 Windows 2012 R2 （或更高版本）

**重要注意事项︰**

- 无法在域控制器上安装网关。

- 只有一个网关可以在一台计算机上安装。

- 在一台计算机上保持，不进入睡眠上安装网关。 如果计算机不在 Azure Analysis Services 服务器无法连接到您的内部数据源刷新数据。

- 并以无线方式连接到您的网络的计算机上安装网关。 可以降低性能。

- 若要更改已配置的网关的服务器名，您需要重新安装并配置一个新的网关。

- 在某些情况下，连接到使用本机的提供程序，例如 SQL Server 本机客户端 (SQLNCLI11) 的数据源的表格模型可能会返回一个错误。 若要了解详细信息，请参阅[数据源的连接](analysis-services-datasource.md)。

## <a name="supported-on-premises-data-sources"></a>受支持的内部部署数据源
预览，网关支持 Azure Analysis Services 服务器和下列内部数据源之间的连接︰

- SQL Server
- SQL 数据仓库
- 接入点
- Oracle
- Teradata


## <a name="download"></a>下载
 [下载该网关](https://aka.ms/azureasgateway)


## <a name="install-and-configure"></a>安装和配置

1. 运行安装程序。

2. 选择安装位置并接受许可条款。

3. 登录到 Azure。

4. 指定您 Azure 分析服务器的名称。 您只能指定一台服务器，每个网关。 单击**配置**，而你就可以进行练习。

    ![登录到 azure](./media\analysis-services-gateway\aas-gateway-configure-server.png)


## <a name="how-it-works"></a>它的工作原理
网关作为 Windows 服务运行，**内部数据网关**，您组织的网络中的计算机上。 安装使用 Azure Analysis Services 的网关基于相同的网关用于其他服务，如电源 BI，但其配置方式存在一些差异。

![它的工作原理](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

查询和数据流量工作如下︰

1.  通过加密凭据与云服务的内部数据源创建查询。 然后，它被发送到网关来处理队列。

2.  网关云服务分析查询并将该请求推送到[Azure 服务总线](https://azure.microsoft.com/documentation/services/service-bus/)。

3.  内部数据网关进行轮询的 Azure 服务总线挂起的请求。

4.  网关获取查询、 解密凭据，并连接到数据源中使用这些凭据。

5.  网关发送到数据源执行的查询。

6.  返回到网关，然后到云服务，结果被发送数据源。

## <a name="windows-service-account"></a>Windows 服务帐户

内部数据网关配置为使用*NT SERVICE\PBIEgwService*的 Windows 服务的登录凭据。 默认情况下，它都有右侧登录作为一种服务;在环境中的计算机上安装网关的。 此凭据不是用来连接到内部数据源的同一帐户或您的 Azure 帐户。  

如果由于身份验证代理服务器遇到问题，您可能需要将 Windows 服务帐户更改为域用户或托管服务帐户。

## <a name="ports"></a>端口

网关创建到 Azure 服务总线出站连接。 出站端口上进行通信︰ TCP 443 （默认），5671，5672，通过 9354 9350。  网关不需要入站的端口。

它已建议白名单的 IP 地址为您在您的防火墙的数据区域。 您可以下载[的 Microsoft Azure 数据中心 IP 列表](https://www.microsoft.com/download/details.aspx?id=41653)。 每周会更新此列表。

> [AZURE.NOTE]  在 CIDR 表示法是 Azure 数据中心 IP 列表中列出的 IP 地址。 例如，10.0.0.0/24 并不意味着 10.0.0.0 通过 10.0.0.24。 了解有关[CIDR 表示法](http://whatismyipaddress.com/cidr)。

以下是由网关使用完全限定的域名。

|域名称|出站端口|说明|
|---|---|---|
|*。 powerbi.com|80|下载安装程序使用 HTTP。|
|*。 powerbi.com|443|HTTPS|
|*。 analysis.windows.net|443|HTTPS|
|*。 login.windows.net|443|HTTPS|
|*。 servicebus.windows.net|5671 5672|高级的消息队列协议 (AMQP)|
|*。 servicebus.windows.net|443，9350 9354|侦听器上通过 TCP （访问控制标记购置需要 443） 服务总线中继|
|*。 frontend.clouddatahub.net|443|HTTPS|
|*。 core.windows.net|443|HTTPS|
|login.microsoftonline.com|443|HTTPS|
|*。 msftncsi.com|443|用于测试互联网连接，如果网关电源 BI 服务不可访问。|
|*.microsoftonline p.com|443|用于根据配置的身份验证。|


### <a name="forcing-https-communication-with-azure-service-bus"></a>强制使用 Azure 服务总线的 HTTPS 通信

您可以强制使用 Azure 服务总线，而不直接 TCP; 使用 HTTPS 通信网关但是，这会大大降低性能。 您需要修改*Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config*文件。 将值从`AutoDetect`到`Https`。 默认情况下，在*C:\Program Files\On 的内部部署的数据网关*，找到该文件。

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```


## <a name="troubleshooting"></a>故障排除
决窍，用来连接到您的本地数据源的 Azure Analysis Services 内部数据网关是用电源 BI 相同的网关。

如果您遇到问题，安装和配置网关时，请务必参阅[疑难解答电源 BI 网关](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem-tshoot/)。 如果您认为有一个问题与您的防火墙，请参阅防火墙或代理服务器部分。

如果您认为您遇到代理问题与网关，看到[电源 BI 网关配置代理服务器设置](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy.md)。

## <a name="next-steps"></a>下一步行动
- [管理 Analysis Services](analysis-services-manage.md)
- [从 Azure Analysis Services 中获取数据](analysis-services-connect.md)
