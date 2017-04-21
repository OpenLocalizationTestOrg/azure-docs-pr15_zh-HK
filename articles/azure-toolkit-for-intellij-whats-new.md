<properties
    pageTitle="新增的 IntelliJ Azure Toolkit |Microsoft Azure"
    description="了解最新的功能在 Azure Toolkit IntelliJ。"
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>什么是 Azure Toolkit 的 IntelliJ 中的新增功能

## <a name="azure-toolkit-for-intellij-releases"></a>对于 IntelliJ 版本的 azure Toolkit

本文包含有关各种版本和最新的更新到 Azure Toolkit IntelliJ。

> [AZURE.NOTE] 此外，还有 Azure Toolkit Eclipse ide。 有关详细信息，请参阅[Eclipse 的 Azure Toolkit]。

### <a name="august-26-2016"></a>2016 年 8 月 26日，

为 IntelliJ-8 月 2016年发布 Azure Toolkit 包括了以下增强功能︰

* **自定义的 JDK 版本**。 对于 IntelliJ Azure Toolkit 现在支持指定并将任意的 JDK 版本部署到 Azure WebApp 容器︰
  - 除了提供 Azure 的 Jdk，您还可以从各种可供选择的祖鲁 OpenJDK 版本供在 Azure 上的蓝色系统的选择。
  - 如果为一个 ZIP 文件上载到您的存储帐户，还可以指定您自己的 JDK 分发。
* **Azure 浏览器视图的增强功能**︰
  - 支持虚拟机管理使用 Azure 的新资源管理器模式︰ 可以列出、 创建和删除资源管理器基于虚拟机，而无需离开 IDE。
  - 支持的存储帐户 blob 管理使用 Azure 的资源管理器，它是用于管理"经典"的存储帐户的现有功能的补充。
* **对于 SQL Server 6.0 的 Microsoft JDBC 驱动程序**。 此更新包括新的 JDBC 驱动程序对 Microsoft SQL Server (v6.0)，这是现在作为您可以方便地添加到 Java 项目的库，从而替换较旧的版本。

### <a name="june-29-2016"></a>2016 年 6 月 29日，

为 IntelliJ-6 月的 2016年发布 Azure Toolkit 包括了以下增强功能︰

* **Java 8 的要求**。 对于 IntelliJ Azure Toolkit 现在需要 Java 8，虽然这一要求是仅对该工具包-您的应用程序可以继续使用所有的 Java 版本所支持的 Azure。
* **最新的 Java Jdk 的支持**。 对于 IntelliJ 中，Azure Toolkit 现在都支持最新版本的 Java Jdk。
* **对 Azure SDK v2.9.1 的支持**。 最新版本的 Azure SDK 现为 IntelliJ Azure Toolkit 最低的先决条件。
* **集成的样本**。 对于 IntelliJ Azure Toolkit 现在特别推出几个示例应用程序以帮助入门的开发人员。
* **HDInsight 工具集成**。 Azure 的 HDInsight 工具现在捆绑在一起的 Azure Toolkit 为 IntelliJ。 有关详细信息，请参阅[IntelliJ HDInsight 工具插件]。
* **远程调试 Java Web 应用程序**。 对于 IntelliJ Azure Toolkit 现在支持 Java web 应用程序在 Azure 应用程序服务的远程调试。

### <a name="april-12-2016"></a>2016 年 4 月 12日，

为 IntelliJ-4 月 2016年发布 Azure Toolkit 包括了以下增强功能︰

* **对 Azure SDK v2.9.0 的支持**。 最新版本的 Azure SDK 现为 IntelliJ Azure Toolkit 最低的先决条件。
* **其他可用性、 响应能力和性能改进与 Azure Web 应用程序的支持**。 大量的性能优化 Toolkit Azure 的结果与通信的方式更快地响应用户界面中。
* **若要删除现有的 Web 应用程序容器内 IntelliJ 从 Azure 中的能力**。 对于 IntelliJ Azure Toolkit 现在允许您无需离开 IntelliJ 中删除现有的 Azure Web 容器。

## <a name="see-also"></a>请参见 ##

针对 Java Ide Azure 工具包的详细信息，请参阅以下链接︰

- [日蚀式的 azure Toolkit]
  - [日蚀式安装 Azure Toolkit]
  - [Azure 在 Eclipse 中创建一个照会世界 Web 应用程序]
  - [什么是 Azure Toolkit 的 Eclipse 中的新功能]
- [对于 IntelliJ azure Toolkit]
  - [安装 IntelliJ Azure Toolkit]
  - [在 IntelliJ azure 创建呼叫世界 Web 应用程序]
  - *什么是 Azure Toolkit 的 IntelliJ （这篇文章） 中的新功能*

有关使用 Java 的 Azure 的详细信息，请参阅[Azure Java 开发人员中心]。

<!-- URL List -->

[日蚀式的 azure Toolkit]: ./azure-toolkit-for-eclipse.md
[对于 IntelliJ azure Toolkit]: ./azure-toolkit-for-intellij.md
[Azure 在 Eclipse 中创建一个照会世界 Web 应用程序]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[在 IntelliJ azure 创建呼叫世界 Web 应用程序]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[日蚀式安装 Azure Toolkit]: ./azure-toolkit-for-eclipse-installation.md
[安装 IntelliJ Azure Toolkit]: ./azure-toolkit-for-intellij-installation.md
[什么是 Azure Toolkit 的 Eclipse 中的新功能]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java 开发人员中心]: http://go.microsoft.com/fwlink/?LinkID=699547

[用于 IntelliJ HDInsight 工具插件。]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md
