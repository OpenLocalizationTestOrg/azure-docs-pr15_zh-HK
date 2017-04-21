<properties 
   pageTitle="用于.NET 2.9 azure SDK 发行说明" 
   description="用于.NET 2.9 azure SDK 发行说明" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-29-release-notes"></a>用于.NET 2.9 azure SDK 发行说明

##<a name="overview"></a>概述

此文档包含.NET 2.9 版本的 Azure sdk 发行说明。 

有关此版本中的更新的详细信息，请参阅[发布 Azure SDK 2.9 公告](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)。

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Visual Studio 2015年更新 2 和 Visual Studio"15"的 azure SDK 2.9 预览
 
此更新包括以下的 bug 修补程序︰

- 其中"未知类型"的字符串将显示为代码生成文件夹的名称和/或命名空间的名称相关的其余部分中的 API 客户端生成的问题放到生成的代码。
- 与计划 WebJobs 身份验证信息失败传递给计划程序的资源调配过程相关的问题。

此更新包括以下新功能︰

- 对应用程序服务设置对话框中的"服务"选项卡中的辅助应用程序服务的支持。 

##<a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Azure 数据湖工具 Visual Studio 2015年更新 2
 
此更新包括以下内容︰

- Visual Studio 的**azure 数据湖工具**现在合并到 Azure SDK 的.NET 版本。 Azure SDK 安装时，将自动安装该工具。 

    该工具需要频繁更新，转[此处](http://aka.ms/datalaketool)以获取更新。

- **服务器资源管理器**现在可以查看所有并创建某些 U SQL 元数据实体。 有关详细信息，请参见[以下](https://azure.microsoft.com/documentation/services/data-lake-analytics/)博客。


##<a name="hdinsight-tools"></a>HDInsight 工具 

**HDInsight 工具**Visual Studio 现在支持 HDInsight 版本 3.3，包括显示 Tez 关系图和其他语言的修正。


##<a name="azure-resource-manager"></a>Azure 的资源管理器 

此版本添加了[KeyVault](../resource-manager-keyvault-parameter.md)支持 ARM 模板。

##<a name="see-also"></a>请参见

[Azure SDK 2.9 发布公告](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)
