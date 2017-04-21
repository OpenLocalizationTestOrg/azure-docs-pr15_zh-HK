<properties
    pageTitle="日蚀式安装 Azure Toolkit |Microsoft Azure"
    description="了解如何安装 Eclipse 的 Azure Toolkit。"
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
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

# <a name="installing-the-azure-toolkit-for-eclipse"></a>日蚀式安装 Azure Toolkit

日蚀式的 Azure Toolkit 提供模板和功能，使您可以轻松地创建、 开发、 测试和部署使用 Eclipse 开发环境的 Azure 应用程序。 日蚀式的 Azure Toolkit 是一个开源项目，其源代码可从以下 URL 在 GitHub 上的项目的站点的 MIT 许可证︰

<https://github.com/microsoft/azure-tools-for-java>

以下步骤显示了如何安装 Eclipse 的 Azure Toolkit。

[AZURE.INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>若要安装 Eclipse 的 Azure Toolkit

1. 启动 Eclipse。

1. Eclipse 打开时，单击**帮助**菜单，如下图所示，然后单击**安装新软件**。

    ![日蚀式安装 Azure Toolkit][01]

1. 在**可用的软件**对话框中**处理**文本框中，键入**http://dl.microsoft.com/eclipse** ，然后按**Enter**键。

1. 在**名称**窗格中，检查**Eclipse 的 Azure Toolkit**，并取消选中**所有更新期间网站的联系人安装来查找所需的软件**。 屏幕应该类似于以下内容︰

    ![日蚀式安装 Azure Toolkit][02]

1. 如果您扩展**Eclipse 的 Azure Toolkit**，您将看到以下各项︰

    * **对于 Java 应用程序的见解插件**︰ 此组件允许您使用 Azure 的遥测记录和分析服务为您的应用程序和服务器实例。
    * **Azure 访问控制服务筛选器**︰ 该组件提供与 ACS Azure 应用程序用户进行身份验证，启用单点登录方案和外置标识逻辑与应用程序的支持。
    * **Azure 公共插件**︰ 此组件提供了所需的其它工具包组件的通用功能。
    * **Azure 的 Eclipse 的资源管理器**︰ 该组件提供所需的其它工具包组件的通用功能。
    * **Azure 使用 Java 的 Eclipse 插件**︰ 该组件开发帮助您构建、 测试和部署 Microsoft Azure 云在 Eclipse 中，然后通过命令行的 Java 应用程序的项目提供支持。
    * **Azure 使用 Java 的 Web 应用程序插件**︰ 该组件部署到 Microsoft Azure Web 应用程序容器的 Java web 应用程序提供支持。
    * **Microsoft 为 SQL Server 的 JDBC 驱动程序 4.2**︰ 该组件提供 JDBC API 用于 SQL Server 和 Microsoft Azure SQL 数据库的 Java 平台企业版 8。
    * **对于 JMS 的 Apache Qpid 客户端库的包**︰ 此组件提供了从 Apache Qpid 项目，以使您的应用程序中使用 AMQP 在 Microsoft Azure 消息的 JMS 客户端组件。
    * **Microsoft Azure 库，用于 Java 的包**︰ 该组件提供的 Api 访问 Microsoft Azure 服务，如存储、 服务总线、 服务运行时等。

1. 单击**下一步**。 （如果安装该工具包时出现不寻常的延迟，确保**所有更新期间网站的联系人安装来查找所需的软件**处于未选中状态。）

1. 在**安装的详细信息**对话框中，单击**下一步**。

    ![查看安装的详细信息][03]

1. 在**查看许可证**对话框中，查看许可协议中的条款。 如果您接受许可协议中的条款，单击**我接受许可协议中的条款**，然后单击**完成**。 （其余的步骤假定您接受许可协议中的条款。 如果您不接受许可协议中的条款，退出安装过程。）

    ![查看许可证][04]

    日蚀式将下载并安装必需的文件包。

    ![安装进度][05]

1. 如果系统提示您重新启动 Eclipse 才能完成安装，请单击**是**。

    ![重新启动提示][06]

## <a name="see-also"></a>请参见

针对 Java Ide Azure 工具包的详细信息，请参阅以下链接︰

- [日蚀式的 azure Toolkit]
  - *Azure Toolkit 安装 Eclipse （这篇文章）*
  - [Azure 在 Eclipse 中创建一个照会世界 Web 应用程序]
  - [什么是 Azure Toolkit 的 Eclipse 中的新功能]
- [对于 IntelliJ azure Toolkit]
  - [安装 IntelliJ Azure Toolkit]
  - [在 IntelliJ azure 创建呼叫世界 Web 应用程序]
  - [什么是 Azure Toolkit 的 IntelliJ 中的新增功能]

有关使用 Java 的 Azure 的详细信息，请参阅[Azure Java 开发人员中心]。

<!-- URL List -->

[日蚀式的 azure Toolkit]: ./azure-toolkit-for-eclipse.md
[对于 IntelliJ azure Toolkit]: ./azure-toolkit-for-intellij.md
[Azure 在 Eclipse 中创建一个照会世界 Web 应用程序]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[在 IntelliJ azure 创建呼叫世界 Web 应用程序]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[安装 IntelliJ Azure Toolkit]: ./azure-toolkit-for-intellij-installation.md
[什么是 Azure Toolkit 的 Eclipse 中的新功能]: ./azure-toolkit-for-eclipse-whats-new.md
[什么是 Azure Toolkit 的 IntelliJ 中的新增功能]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java 开发人员中心]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

