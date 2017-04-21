<properties
   pageTitle="如何连接到数据源 |Microsoft Azure"
   description="突出显示如何连接到数据源发现 Azure 数据目录的操作方法的文章。"
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>


# <a name="how-to-connect-to-data-sources"></a>如何连接到数据源

## <a name="introduction"></a>介绍
**Microsoft Azure 数据目录**是查询的一种完全托管的云服务，作为注册的系统和企业数据源的系统。 换句话说， **Azure 数据目录**是帮助人了解、 理解和使用数据源，并帮助企业从他们现有的数据中获取更大的价值。 这种情况的一个重要方面使用数据，一旦用户发现数据源，并了解它的用途下, 一步是连接到数据源，以利用其数据。

## <a name="data-source-locations"></a>数据源位置
数据源在注册期间， **Azure 数据目录**收到有关数据源的元数据。 此元数据包括在数据源的位置的详细信息。 详细信息的位置会因数据源到数据源，但它将始终包含连接所需的信息。 例如，SQL Server 表的位置包括服务器名称、 数据库名称、 架构名称和表名称，而 SQL Server 报表服务报表的位置包括服务器名称和报表的路径。 其他数据源类型会有反映的结构和功能的源系统的位置。

## <a name="integrated-client-tools"></a>集成的客户端工具
连接到数据源的最简单方法是使用"中打开。.." **Azure 数据目录**入口中的菜单。 此菜单显示用于连接到所选的数据资产的选项列表。
在使用默认的平铺视图时，此菜单出现在每个图块上。

 ![从数据资产平铺在 Excel 中打开 SQL Server 表](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

时使用列表视图，菜单上有门户窗口顶部的搜索栏。

 ![在搜索栏中打开 SQL Server 报表服务报表在报表管理器](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>支持的客户端应用程序
当使用"中打开。.." 在客户端计算机上必须安装在 Azure 数据目录门户中，正确的客户端应用程序的数据源的菜单。

| 在应用程序中打开 | 文件扩展名 / 协议 | 支持的应用程序版本 |
| --- | --- | --- |
| Excel | .odc | Excel 2010 或更高版本 |
| Excel （顶部 1000年） | .odc | Excel 2010 或更高版本 |
| 电源查询 | .xlsx | 安装了 Excel 2016 或 Excel 2010 或 Excel 2013 与 Excel 外接电源查询
| 电源双桌面 | .pbix | 电源双桌面 7 月 2016年或更高版本 |
| SQL Server 数据工具 | vsweb: / / | 4 或更高版本使用 SQL Server 工具安装的 Visual Studio 2013年更新 |
| 报表管理器 | http:// | 请参阅[SQL Server 报表服务的浏览器要求](https://technet.microsoft.com/en-us/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>您的数据，您的工具
菜单中可用的选项取决于当前所选的数据资产的类型。 当然，并不是所有可能的工具将包括在"中打开。.." 菜单上，但它是仍可以轻松连接到使用任一客户端工具的数据源。 **Azure 数据目录**入口中选择数据资产时，在属性窗格中显示的完整位置。

 ![SQL Server 表的连接信息](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

连接信息的详细信息将与数据源类型，不同数据源类型，但包括在门户网站中的信息将为您提供一切您需要连接到数据源中所有客户端工具。 用户可以复制他们发现使用**Azure 数据目录**，从而使他们能够处理他们所选择的工具中的数据的数据源的连接详细信息。

## <a name="connecting-and-data-source-permissions"></a>连接和数据源的权限
尽管**Azure 数据目录**可以使数据源能够发现，访问数据本身仍然在数据源的所有者或管理员的控制之下。 发现在**Azure 数据目录**中的数据源不允许用户访问数据源本身的任何权限。

使它更便于发现数据源，但不是具有其数据的访问权的用户，用户可以对数据源进行批注时提供请求访问属性中的信息。 旁边的数据源位置信息在门户中显示 — 包括链接到进程或用于访问数据源的联系点--此处提供的信息。

 ![请求访问说明提供的连接信息](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

##<a name="summary"></a>摘要
**Azure 数据目录**中注册的数据源使该数据可发现通过将结构和描述性元数据从数据源复制到的目录服务。 数据源已注册，并发现，一旦用户可连接到数据源从**Azure 数据目录**入口"中打开。.."" 菜单或使用他们的数据的工具选择。

## <a name="see-also"></a>请参见
- 有关如何连接到数据源的逐步骤详细信息的[Azure 数据目录入门](data-catalog-get-started.md)教程。
