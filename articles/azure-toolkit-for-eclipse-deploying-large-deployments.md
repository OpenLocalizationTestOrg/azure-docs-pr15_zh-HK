<properties
    pageTitle="部署大型部署"
    description="了解如何部署使用 Eclipse Azure Toolkit 的大规模部署。"
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->

# <a name="deploying-large-deployments"></a>部署大型部署 #

如果您的部署是太大，无法包含在默认的 approot 文件夹中，您可以用作本地存储资源部署根文件夹您的 JDK 和应用程序服务器。

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>对于大型部署用作部署根文件夹的本地存储资源 ##

1. 创建新的本地存储资源。 资源的名称并不重要。 在角色级别定义的存储资源。 访问本地存储的配置对话框，从中您可以创建新的本地存储资源，最快捷的方法是通过使用以下步骤︰ 用鼠标右键单击**项目资源管理器**视图中的角色 （展开 Azure 项目节点如果您看不到角色）， **Azure**，请单击，然后单击**本地存储区**。 在**本地存储**对话框中，单击**添加**以创建新的本地存储资源。
1. 将所需的大小设置为至少 2048 MB （任何东西不太可能会导致同一文件大小问题一样会遇到 approot）。
1. 确保选中**清除时的角色实例将被回收的内容**;这将有助于防止部署的启动逻辑到冲突与现有资源中的文件时运行角色实例将被回收。
1. 确保**存储部署后资源的目录路径环境变量**的值设置为字符串**DEPLOYROOT**。 您的本地存储资源对话框看起来将类似于以下。
    ![][ic667943]

或者，如果您使用**DEPLOYROOT**作为所需的本地资源的*名称*，您不要更改自动生成的环境变量名称 （这将设置为**DEPLOYROOT_PATH**在这种情况下），可用于您的应用程序。

有关创建本地存储资源的附加信息可以位于[本地存储属性][]。

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
[本地存储属性]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png
