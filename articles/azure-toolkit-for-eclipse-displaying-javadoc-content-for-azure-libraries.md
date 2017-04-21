<properties
    pageTitle="对于 Java 的 Azure 库包在 Eclipse 中显示 Javadoc 内容"
    description="如何在 Eclipse 中 Azure 库显示 Javadoc 内容。"
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>对于 Java 的 Azure 库包在 Eclipse 中显示 Javadoc 内容 #

Azure 的 Java 库的 Javadoc 内容可以查看在 Eclipse 环境中，通过将 Javadoc 内容对 Azure 的 Java 库相关联。 以下步骤演示如何使用此功能在 Eclipse 中。

此过程假定您已添加到生成路径用于 Java 的 Azure 库。

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Azure 的 Java 库在 Eclipse 中显示 Javadoc 内容 ##

* 在 Eclipse 的项目资源管理器中，您的项目的**引用库**中打开上下文菜单 Azure 存储库 Java JAR。 例如， **microsoft windowsazure api 0.1.0.jar** （版本号可能是不同的取决于已安装的版本）。
* 单击**属性**。
* 在**属性**对话框的左侧窗格中，单击**Javadoc 位置**。 将显示**Javadoc 位置**对话框。
* 您可以指定**Javadoc URL**或**Javadoc 在存档文件中**。
    * 如果您选择指定的**Javadoc URL**，使用的 Url，如**http://dl.windowsazure.com/javadoc**或**http://dl.windowsazure.com/storage/javadoc**。
    * 如果您选择使用**归档中的 Javadoc**，您可以指定一个外部文件或工作区文件。
    做出选择，浏览/验证的需要。 下面的示例将名为**c:\MyAzureJARs**的文件夹到本地下载相应 Javadoc JAR 与 Java Azure 库。
    ![][ic553487]
* *可选*︰ 单击**验证**。 无法在此处显示 Javadoc JAR 的潜在问题。
* 单击**确定**。

一旦与库关联，Javadoc 内容应显示在 Eclipse IDE 中。 例如，如果`blob`的类型定义`CloudBlockBlob`在代码中，下面是一种在您键入时显示 Javadoc 内容`blob.acquireLease`代码中︰

![][ic553488]

## <a name="see-also"></a>请参见 ##

[日蚀式的 azure Toolkit][]

[Azure 在 Eclipse 中创建一个 Hello World 应用程序][]

[日蚀式安装 Azure Toolkit][] 

有关使用 Java 的 Azure 的详细信息，请参阅[Azure Java 开发人员中心][]。

<!-- URL List -->

[Azure Java 开发人员中心]: http://go.microsoft.com/fwlink/?LinkID=699547
[日蚀式的 azure Toolkit]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure 在 Eclipse 中创建一个 Hello World 应用程序]: http://go.microsoft.com/fwlink/?LinkID=699533
[日蚀式安装 Azure Toolkit]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png