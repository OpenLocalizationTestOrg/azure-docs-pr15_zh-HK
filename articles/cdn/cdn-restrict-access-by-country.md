<properties
    pageTitle="按国家/地区的 Azure CDN 内容限制访问 |Microsoft Azure"
    description="了解如何限制对 Azure CDN 内容使用地区筛选功能的访问。"
    services="cdn"
    documentationCenter=""
    authors="camsoper, rli"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="casoper"/>

#<a name="restrict-access-to-your-content-by-country---verizon"></a>限制对您的国家/地区-Verizon 的内容的访问

> [AZURE.SELECTOR]
- [Verizon](cdn-restrict-access-by-country.md)
- [Akamai 标准](cdn-restrict-access-by-country-akamai.md)

##<a name="overview"></a>概述

当用户请求内容，默认情况下时，无论用户在其中做出这样的请求被提供内容。 在某些情况下，您可能需要限制对您的内容按国家/地区的访问。 本主题说明如何使用**地区筛选**功能才能配置该服务以允许或阻止访问按国家/地区。

> [AZURE.IMPORTANT] Verizon 和 Akamai 产品提供相同的 geo 筛选功能，但用户界面不同。 本文档描述的接口**标准从 Verizon 的 CDN Azure**和**Verizon 从 Azure CDN 津贴**。 与**标准从 Akamai 的 CDN Azure**geo 筛选，请参阅[限制各国的 Akamai 的内容的访问](cdn-restrict-access-by-country-akamai.md)。

有关注意事项适用于配置这种类型的限制的信息，请参阅本主题的末尾[注意事项](cdn-restrict-access-by-country.md#considerations)一节。  

>[AZURE.NOTE] 一旦配置的设置，将应用于**从 Verizon 的 Azure CDN** Azure CDN 配置文件中的所有终结点。

![国家筛选](./media/cdn-filtering/cdn-country-filtering.png)

##<a name="step-1-define-the-directory-path"></a>步骤 1︰ 定义的目录路径

在配置的国家/地区筛选器时，必须指定的允许或拒绝访问的用户的位置的相对路径。 您可以应用您的所有文件的筛选国家"/"或通过指定目录路径中选择文件夹。

示例目录路径筛选器︰

    /                                 
    /Photos/
    /Photos/Strasbourg

##<a name="step-2-define-the-action-block-or-allow"></a>步骤 2︰ 定义的操作︰ 阻止或允许

**块︰**从指定的国家/地区的用户将被拒绝访问资产要求从该递归路径。 如果没有其他国家筛选选项已配置为该位置了，那么所有其他用户将被允许访问。

**允许︰**仅从指定的国家/地区的用户将允许对资产要求从该递归路径的访问。

##<a name="step-3-define-the-countries"></a>第 3 步︰ 定义国家/地区

选择希望阻止或允许的路径的国家/地区。 有关详细信息，请参阅[从 Verizon 国家/地区代码的 Azure CDN](https://msdn.microsoft.com/library/mt761717.aspx)。

例如，阻止 /Photos/Strasbourg/规则将筛选的文件包括︰

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


##<a name="country-codes"></a>国家/地区代码

**地区筛选**功能使用国家/地区代码来定义的国家的允许或阻止对受保护的目录请求。 在[Azure CDN 从 Verizon 国家/地区代码](https://msdn.microsoft.com/library/mt761717.aspx)，您将找到国家/地区代码。 如果您指定"欧盟"（欧洲） 或"接入点"（亚太），则将阻止或允许来自该区域任何国家/地区的 IP 地址的子集。


##<a id="considerations"></a>注意事项

- 可能需要最多 90 分钟后，更改到您的国家/地区筛选配置以使其生效。
- 此功能不支持通配符字符 (例如，*)。
- 筛选与相对路径相关联的配置的国家/地区将递归地应用于该路径。
- 一个规则可以应用于相同的相对路径 （不能创建指向相同的相对路径的多个国家筛选器。 但是，文件夹可能有多个国家筛选器。 这是由于国家筛选器的递归性质。 换句话说，以前配置文件夹的子文件夹，可以分配不同国家筛选器。
