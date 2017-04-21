<properties
    pageTitle="Azure 应用程序服务，并对现有的 Azure 服务的影响"
    description="说明了新 Azure 应用程序服务和它的功能对 Azure 中的现有服务的影响。"
    services="app-service"
    documentationCenter=""
    authors="yochay"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="yochayk"/>


# <a name="azure-app-service-and-existing-azure-services"></a>Azure 应用程序服务和现有的 Azure 服务

本文概述现有的 Azure 服务变为作为更改到[Azure 应用程序服务](https://azure.microsoft.com/services/app-service/)，新的集成式解决方案将组合在一起的几个 Azure 服务的一部分。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>概述

[Azure 应用程序服务](https://azure.microsoft.com/services/app-service/)是一种新的和独特的云服务，使开发人员能够创建 web 和移动应用程序以及任何平台的任何设备。 应用程序服务是一个集成的解决方案，旨在简化重复编码函数、 与企业和 SaaS 系统集成和自动化业务流程，同时满足您对安全性、 可靠性和可扩展性需求。

应用程序服务将下列现有 Azure 服务的[网站](https://azure.microsoft.com/services/websites/)、[移动服务](https://azure.microsoft.com/services/mobile-services/)和[Biztalk 服务](https://azure.microsoft.com/services/biztalk-services/)整合到一个组合服务，添加强大的新功能时。  应用程序服务可以承载以下应用程序类型︰

-   Web 应用程序
-   移动应用程序
-   API 的应用程序
-   逻辑应用程序

下表说明如何现有 Azure 服务映射到应用程序服务，并在其中可用的应用程序类型。

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">现有的 Azure 服务</th>
<th align="left", style="width:10%">Azure 应用程序服务</th>
<th align="left", style="width:80%">更改的内容</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure 网站</td>
<td align="left">Web 应用程序</td>
<td align="left"><li>Azure 网站为应用程序服务是局限于网站的名称更改为 Web 应用程序。
<p><li>所有的网站的现有实例现在是应用程序服务中的 Web 应用程序。</p>
<p><li>您可以访问您现有的网站通过<a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure 门户</a>，您可以在其中找到<em>Web 应用程序</em>在所有现有网站。</p>
<p><li><em>Web 托管计划</em>现在是<em>应用程序服务计划</em>。 <em>应用程序服务计划</em>可以承载任何应用程序类型的应用程序服务，如 Web、 移动、 逻辑或 API 的应用程序。</p>
<p><li>Azure 应用程序服务 Web 应用程序通常是可用性。</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">了解有关 Web 应用程序</a>。</p></td>
</tr>
<tr class="even">
<td align="left">Azure 的移动服务</td>
<td align="left">移动应用程序</td>
<td align="left"><p><li>移动服务继续作为独立的服务提供和保持完全受支持。</p>
<p><li>移动应用程序是功能的在应用程序服务集成的移动服务和更多的所有应用程序类型。</p>
<p><li>很容易<a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">迁移到移动应用程序的移动服务</a>。</p>
<p><li>作为应用程序服务的一部分，移动应用程序获取移动服务以外的新功能，比如与内部和 SaaS 系统集成临时插槽、 WebJobs、 更好地缩放选项和更多。</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">了解有关移动应用程序</a>。</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API 的应用程序</td>
<td align="left">
<p><li>API 的应用程序是一个新的应用程序类型，可以轻松地构建和使用 Api 在云中的应用程序服务中。</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">了解更多关于 API 的应用程序</a>。</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">逻辑应用程序</td>
<td align="left">
<p><li>逻辑应用程序是应用程序服务，您可以轻松地自动业务流程中的新应用程序类型。</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">了解更多关于逻辑应用程序</a>。</p></td>
</tr>
<tr class="odd">
<td align="left">Azure BizTalk 服务</td>
<td align="left">BizTalk API 的应用程序</td>
<td align="left">
<li><p>BizTalk 服务继续作为独立的服务提供和保持完全受支持。</p>
<li><p>BizTalk 服务所有的功能集成到应用程序服务作为 API 的应用程序使用户能够执行企业应用程序集成和 B2B 集成方案与任何应用程序类型在应用程序服务</p>
<li><p>与逻辑应用程序现在可以自动使用可视化设计体验来创建工作流的业务流程</p></td>
</tr>
</tbody>
</table>

若要了解详细信息，请访问[应用程序服务文档](https://azure.microsoft.com/documentation/services/app-service/)。
