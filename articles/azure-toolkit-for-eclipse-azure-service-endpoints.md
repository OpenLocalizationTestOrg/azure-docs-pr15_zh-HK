<properties
    pageTitle="Azure 服务终结点"
    description="描述在 Eclipse 的 Azure Toolkit 的 Azure 服务终结点设置。"
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

# <a name="azure-service-endpoints"></a>Azure 服务终结点 #

Azure 服务终结点确定是否您的应用程序的部署和管理全局 Azure 平台，Azure 由中国或专用的 21Vianet Azure 平台。 **服务终结点**对话框允许您指定要使用哪个服务终结点。 若要打开**服务终结点**对话框中的在 Eclipse 中，单击**窗口**、**首选项**扩展**Azure**，和，然后单击**服务终结点**。 **活动设置**字段设置确定哪些 Azure 服务终结点将用于当前的工作区中的 Azure 项目。

下图显示对话框中**服务终结点**。

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>若要设置服务终结点 ##

在**服务终结点**对话框中，执行下列操作之一︰

* 如果您想要使用的全局 Azure 平台，从**当前设置**下拉列表中，选择**windowsazure.com** ，然后单击**确定**。
* 如果您想要使用 Azure 由 21Vianet 在中国，从**当前设置**下拉列表中，选择**windowsazure.cn （中国）** ，然后单击**确定**。
* 如果您想要使用专用 Azure 平台︰
    1. 单击**编辑**。
    2. 将打开一个对话框，通知您的**服务终结点**对话框将关闭，并且将打开首选项设置文件。 单击**确定**。
    3. 在 preferencesets.xml 文件中，创建一个新`preferenceset`元素。 此新的元素，创建`name`， `blob`， `management`， `portalURL` ，`publishsettings`特性，并将它们对应的值添加到专用 Azure 平台。 您可以使用所提供的现有值`preferenceset`作为模板的元素。 **注意**︰ 使用的值`blob`属性必须包含在 URL 中的"blob"的文本。
    4. 保存并关闭 preferencesets.xml。
    5. 重新打开对话框中**服务终结点**。
    6. 从**当前设置**下拉列表中，选择您创建的活动集并单击**确定**。
    7. 一旦您已经创建了您的专用 Azure 平台`preferenceset`元素，您可以更改单击**服务终结点**对话框中的**编辑**按钮为其分配默认值。 您还可以创建多个专用 Azure 平台`preferenceset`元素，如果需要。

## <a name="see-also"></a>请参见 ##

[日蚀式的 azure Toolkit][]

[日蚀式安装 Azure Toolkit][] 

[Azure 在 Eclipse 中创建一个 Hello World 应用程序][]

有关使用 Java 的 Azure 的详细信息，请参阅[Azure Java 开发人员中心][]。

<!-- URL List -->

[Azure Java 开发人员中心]: http://go.microsoft.com/fwlink/?LinkID=699547
[日蚀式的 azure Toolkit]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure 在 Eclipse 中创建一个 Hello World 应用程序]: http://go.microsoft.com/fwlink/?LinkID=699533
[日蚀式安装 Azure Toolkit]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png
