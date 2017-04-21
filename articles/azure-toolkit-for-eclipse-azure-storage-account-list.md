<properties
    pageTitle="Azure 存储帐户列表"
    description="管理您的 Eclipse 使用 Azure Toolkit 的存储帐户设置"
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->

# <a name="azure-storage-account-list"></a>Azure 存储帐户列表 #

Azure 存储帐户启用要使用您的 JDK，应用程序服务器和任意组件，以及存储状态时使用缓存的下载位置。 日蚀式维护已知的存储帐户可供您在您的 Eclipse 工作区中的项目的列表。 若要打开**存储帐户**对话框，用来管理该列表，在 Eclipse 中，单击**窗口****首选项**展开**Azure**，和，然后单击**存储帐户**。

下图显示对话框中**存储帐户**。

![][ic719496]

也可以从一个**帐户**链接上使用存储帐户，如下所示的对话框打开此对话框︰

* **服务器配置**对话框的**JDK**选项卡。
* **服务器配置**对话框的**服务器**选项卡。
* **添加组件**对话框中。
* **缓存**属性对话框中。

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>若要导入您的存储帐户使用发布设置文件 ##

1. 在**存储帐户**对话框中，单击**从发布设置文件导入**。
2. （跳过这一步如果您已经发布设置文件保存到本地计算机。在**导入订阅信息**对话框中，单击**下载发布设置文件**。 如果您还没有登录到 Azure 帐户，系统将提示您登录。 然后系统将提示您保存 Azure 发布设置文件。 （可以忽略登录网页-网页上显示的结果说明他们提供的 Azure 的门户和适用于 Visual Studio 用户。）将其保存到本地计算机。
3. 仍在**导入订阅信息**对话框中，单击**浏览**按钮，选择发布设置文件中保存在本地以前，，然后单击**打开**。
4. 单击**确定**以关闭**导入订阅信息**对话框。

## <a name="to-create-a-new-storage-account"></a>若要创建新的存储帐户。 ##

1. 在**存储帐户**对话框中，单击**添加**。
2. 在**添加存储帐户**对话框中，单击**新建**。
3. 在**新的存储帐户**对话框中，指定以下值︰
    * 存储的帐户名。
    * 存储帐户的位置。
    * 存储帐户的说明。
    * 订阅存储帐户属于哪个。
4. 单击**确定**以关闭**新的存储帐户**对话框中。

它可能需要几分钟，让您的存储帐户创建。 它被创建后，单击**确定**以关闭**添加存储帐户**对话框，您新的存储帐户将被添加到列表中的可用存储帐户。

## <a name="to-add-an-existing-storage-account-to-the-list"></a>若要向列表中添加现有的存储帐户 ##

1. 如果您没有一个 Azure 存储帐户，创建一个在**创建新的存储帐户部分**上面列出的步骤。 （或者，您可以创建新的存储帐户在[Azure 管理门户][]。）
2. 在**存储帐户**对话框中，单击**添加**。
3. 在**添加存储帐户**对话框中输入的值**名称**和**访问键**。 必须为现有 Azure 存储帐户的帐户名称和访问键。 使用[Azure 管理门户][]的**存储**部分以查看您的存储帐户名称和键。 **添加存储帐户**对话框看起来将类似于以下。

    ![][ic719497]

4. 单击**确定**以关闭**添加存储帐户**对话框中。

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>要修改存储帐户使用一个新的访问键 ##

1. 中**存储帐户**对话框中，单击您想要编辑，然后单击**编辑**存储帐户。
2. 在**编辑存储帐户访问权限项**对话框中，修改的**访问键**的值。
3. 单击**确定**以关闭**编辑存储帐户访问密钥**对话框。

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>若要从在 Eclipse 中维护的列表中删除存储帐户 ##

1. 中**存储帐户**对话框中，单击您想要编辑，然后单击**删除**的存储帐户。
2. 单击**确定**时提示您要删除的存储帐户。

>[AZURE.NOTE] 删除到对话框中**存储帐户**存储帐户只有它从列表中删除在 Eclipse 中可查看的存储帐户。 它不会从 Azure 订阅删除存储帐户。 此外，存储帐户可能再次出现在列表中后 Eclipse 重新加载您的订阅的详细信息。

## <a name="see-also"></a>请参见 ##

[日蚀式的 azure Toolkit][]

[日蚀式安装 Azure Toolkit][] 

[Azure 在 Eclipse 中创建一个 Hello World 应用程序][]

有关使用 Java 的 Azure 的详细信息，请参阅[Azure Java 开发人员中心][]。

<!-- URL List -->

[Azure Java 开发人员中心]: http://go.microsoft.com/fwlink/?LinkID=699547
[日蚀式的 azure Toolkit]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure 的管理门户]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure 在 Eclipse 中创建一个 Hello World 应用程序]: http://go.microsoft.com/fwlink/?LinkID=699533
[日蚀式安装 Azure Toolkit]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png
