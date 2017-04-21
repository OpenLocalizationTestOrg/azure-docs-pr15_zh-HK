<properties
   pageTitle="什么是 Azure 数据目录？ |Microsoft Azure"
   description="这篇文章概括介绍了 Microsoft Azure 数据目录，包括它的功能和它所针对的问题。 数据目录提供功能，使任何用户 – 数据科学家向开发人员 – 若要注册，向分析师发现、 了解和使用的数据源。"
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="what-is-azure-data-catalog"></a>什么是 Azure 数据目录？

Azure 数据目录是完全托管的云服务，使用户查找他们所需的数据源并了解他们发现，同时帮助企业从现有投资中获取更大价值的数据源。 数据目录提供功能，使任何用户 – 数据科学家向开发人员 – 来发现，与分析师理解和使用的数据源。 数据目录包含的 crowdsourcing 模型的元数据和批注，并允许所有用户参与他们的知识来构建一个社区和区域性的数据。

## <a name="discovery-challenges-for-data-consumers"></a>数据使用者的发现挑战

传统上，发现企业数据源已基于经验知识的一个有机过程。 这对于想要从他们的信息资产中获得最大价值的公司面临几个难题。

-   用户将不知道数据源存在，除非他们接触它属于另一个进程;没有注册的数据源了中心位置。
-   除非用户知道数据源的位置，他将不能连接到数据使用客户端应用程序;数据消耗量经验要求用户知道连接字符串或路径。
-   除非用户知道的数据源文档的位置，他无法理解用途的数据;数据源和文档在不同的地方居住，消耗通过不同的经验。
-   如果用户有疑问的信息资产，他必须找到专家或工作组负责数据并请这些专家离线;没有显式连接之间数据和专家的视角对其使用的那些。
-  除非用户了解请求访问数据源的过程，发现在数据源和它的文档仍不允许他访问他所要求的数据。

## <a name="discovery-challenges-for-data-producers"></a>发现数据生产者面临挑战

数据使用者面对这些挑战，同时负责制订和维护信息资产的用户面临着他们自己的挑战。

-   注释包含描述性元数据的数据源通常是丢失的努力;客户端应用程序通常会忽略存储在数据源的说明。
-   创建数据源的文档通常是丢失的努力;保持文档与数据源同步是日常的职责范围，并且用户缺乏信任文档中的，如通常被视为是过时。
- 限制访问的数据源，并确保数据使用者知道如何请求访问是一个经常性的难题。

创建和维护有关数据源的文档非常复杂且耗时。 使该文档随时可供使用的数据源的每个人的挑战通常是更加便宜。

结合使用时，这些挑战会对于那些想要鼓励和促进使用和理解的企业数据的公司提供一个巨大的障碍。

## <a name="azure-data-catalog-can-help"></a>Azure 数据目录可帮助

数据目录旨在解决这些问题，使企业能够以从其现有的信息资产中获得最大价值。 通过由他们管理的数据需要的用户可轻松查找和理解使数据源数据目录可帮助。

数据目录提供基于云的服务源注册到的数据。 数据将保留在现有位置，但元数据的副本中添加到数据目录和数据源位置的引用。 此元数据也编入索引以使每个数据源很容易地发现通过搜索，并发现它的用户所理解。

一旦已注册的数据源，其元数据可以然后可丰富了，执行注册、 用户或企业中的其他用户。 任何用户都可以通过提供说明、 标签或其他元数据，例如文档和流程的请求数据源访问注释数据源。 这种描述性元数据补充了结构元数据 （例如，列名和数据类型） 数据源的注册。

发现和理解数据源和它们的使用是注册源的主要目的。 当企业用户需要为他们的工作 （这可能是商业智能，应用程序开发、 数据科学或任何其他任务是必需的适当的数据） 的数据可用于数据目录搜索体验快速找到符合他们需要的数据，了解评估目的，其适用性和使用该数据通过他们选择的工具中打开数据源的数据。 同时，数据目录允许用户通过标记、 记录和批注数据源已被注册，以及注册新的数据源可以进行发现、 了解和使用的目录用户群参与到编录。

![数据目录功能](./media/data-catalog-what-is-data-catalog/data-catalog-capabilities.png)

## <a name="get-started-with-data-catalog"></a>开始使用数据目录

若要立即开始数据目录，请访问[www.azuredatacatalog.com](https://www.azuredatacatalog.com)。

获取启动的指南位于[此处](data-catalog-get-started.md)。

## <a name="learn-more-about-data-catalog"></a>了解更多关于数据目录

若要了解有关数据目录的功能的详细信息，请参阅︰

* [如何注册数据源](data-catalog-how-to-register.md)
* [如何发现数据源](data-catalog-how-to-discover.md)
* [如何对数据源进行批注](data-catalog-how-to-annotate.md)
* [如何记录的数据源](data-catalog-how-to-documentation.md)
* [如何连接到数据源](data-catalog-how-to-connect.md)
* [如何处理大数据](data-catalog-how-to-big-data.md)
* [如何管理数据资产](data-catalog-how-to-manage.md)
* [如何设置业务术语表](data-catalog-how-to-business-glossary.md)
* [常见问题](data-catalog-frequently-asked-questions.md)
