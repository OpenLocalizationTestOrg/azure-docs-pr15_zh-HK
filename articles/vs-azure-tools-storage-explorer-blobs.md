<properties
    pageTitle="使用存储浏览器 （预览） 的 Azure Blob 存储资源管理 |Microsoft Azure"
    description="管理 Azure Blob 容器，并使用存储浏览器 （预览） 的 Blob"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>使用存储浏览器 （预览） 的 Azure Blob 存储资源管理

## <a name="overview"></a>概述

[Azure Blob 存储](./storage/storage-dotnet-how-to-use-blobs.md)是一种服务来存储大量的非结构化数据，如文本或二进制数据，可以从任何位置访问在世界中通过 HTTP 或 HTTPS。
您可以使用 Blob 存储，将数据暴露给全世界，公开或私下存储应用程序数据。 在本文中，您将学习如何使用存储浏览器 （预览） 使用 blob 容器和 blob。

## <a name="prerequisites"></a>系统必备组件

若要完成本文中的步骤操作，您需要︰

- [下载并安装存储浏览器 （预览）](http://www.storageexplorer.com)
- [连接到 Azure 存储帐户或服务](./vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>创建 blob 容器

所有 blob 必须都驻留在 blob 容器，即只是 blob 的逻辑分组。 一个帐户可以包含任意的数量的容器，并且每个容器可以存储任意的数量的 blob。

以下步骤说明了如何创建 blob 容器中存储浏览器 （预览）。

1.  打开存储资源管理器 （预览）。
1.  在左窗格中，展开要在其中创建 blob 容器的存储帐户。
1.  **Blob 容器**，用鼠标右键单击-并从上下文菜单中的选择**创建 Blob 容器**。

    ![创建 blob 容器上下文菜单][0]

1.  在文本框中将出现下面的**Blob 容器**文件夹。 输入为 blob 容器的名称。 请参见[容器命名规则](./storage/storage-dotnet-how-to-use-blobs.md#create-a-container)列表的规则和限制命名 blob 容器。

    ![创建 Blob 容器的文本框][1]

1.  按**enter 键**完成创建 blob 容器或**esc 键**可取消。 一旦已成功创建 blob 容器，它将显示所选的存储帐户的**Blob 容器**文件夹下。

    ![创建 blob 容器][2]

## <a name="view-a-blob-containers-contents"></a>查看 blob 容器的内容

Blob 容器包含 blob 和文件夹 （它也可以包含 blob）。

下面的步骤说明了如何查看存储浏览器 （预览） 中的 blob 容器的内容︰

1.  打开存储资源管理器 （预览）。
1.  在左窗格中，展开要查看包含 blob 容器的存储帐户。
1.  展开存储帐户的**Blob 容器**。
1.  用鼠标右键单击想要查看，blob 容器-并从上下文菜单中的选择**打开 Blob 容器编辑器**。
也可以双击想要查看的 blob 容器。

    ![打开 blob 容器编辑器上下文菜单][19]

1.  主窗格中将显示该 blob 容器的内容。

    ![Blob 容器编辑器][3]

## <a name="delete-a-blob-container"></a>删除一个 blob 容器

Blob 容器可以方便地创建和删除根据需要。 （若要了解如何删除单个 blob，请参阅的部分，[管理 blob 中的 blob 容器](./#managing-blobs-in-a-blob-container)。）

下面的步骤说明如何删除存储浏览器 （预览） 中的 blob 容器︰

1.  打开存储资源管理器 （预览）。
1.  在左窗格中，展开要查看包含 blob 容器的存储帐户。
1.  展开存储帐户的**Blob 容器**。
1.  右键单击您想要删除的 blob 容器-并从上下文菜单中的选择**删除**。
您也可以按**删除**可删除当前所选的 blob 容器。

    ![删除 blob 容器上下文菜单][4]

1.  确认对话框时，选择**是**。

    ![删除 blob 容器确认][5]

## <a name="copy-a-blob-container"></a>复制一个 blob 容器

存储资源管理器 （预览） 使您能够将 blob 容器复制到剪贴板中，然后再将该 blob 容器粘贴到另一个存储帐户。 （若要查看如何复制单个 blob，请参阅节， [blob 容器中的管理 blob](./#managing-blobs-in-a-blob-container)。）

以下步骤说明了如何将 blob 容器复制到另一个存储帐户。

1.  打开存储资源管理器 （预览）。
1.  在左窗格中，展开要复制包含 blob 容器的存储帐户。
1.  展开存储帐户的**Blob 容器**。
1.  右键单击您想要复制的 blob 容器-并从上下文菜单中的选择**复制的 Blob 容器**。

    ![复制 blob 容器上下文菜单][6]

1.  用鼠标右键单击要向其中粘贴该 blob 容器，所需的"目标"存储帐户-并从上下文菜单中的选择**粘贴 Blob 容器**。

    ![粘贴 blob 容器上下文菜单][7]

## <a name="get-the-sas-for-a-blob-container"></a>SAS 获取 blob 容器

[共享的访问签名 (SAS)](./storage/storage-dotnet-shared-access-signature-part-1.md)提供委派的访问您的存储帐户中的资源。
这意味着您可以授予客户端而无需共享您的帐户访问键指定期限内的时间，并具有一组指定的权限，限制对您的存储帐户中的对象的权限。

下面的步骤演示如何创建 blob 容器 SA:

1.  打开存储资源管理器 （预览）。
1.  在左窗格中，展开包含要为其获取 SAS 的 blob 容器的存储帐户。
1.  展开存储帐户的**Blob 容器**。
1.  用鼠标右键单击所需的 blob 容器，-并从上下文菜单中的选择**获得共享访问签名**。

    ![获得 SAS 上下文菜单][8]

1.  在**共享访问签名**对话框中，指定策略、 开始日期和到期日期、 时区和访问的级别所需的资源。

    ![得到 SA 选项][9]

1.  当您完成指定的 SAS 选项时，选择**创建**。

1.  第二个**共享访问签名**对话框将显示列出的 URL 和 QueryStrings 可用于访问存储资源 blob 容器。
选择您要复制到剪贴板的 URL 旁边的**副本**。

    ![复制 SAS 的 Url][10]

1.  完成后，选择**关闭**。

## <a name="manage-access-policies-for-a-blob-container"></a>Blob 容器管理的访问策略

下面的步骤说明如何管理 （添加和删除） 访问 blob 容器的策略︰

1.  打开存储资源管理器 （预览）。
1.  在左窗格中，展开包含您要管理其访问策略 blob 容器的存储帐户。
1.  展开存储帐户的**Blob 容器**。
1.  选择所需的 blob 容器，-并从上下文菜单中的选择**管理访问策略**。

    ![管理访问策略上下文菜单][11]

1.  **访问策略**对话框将列出已为选定的 blob 容器创建任何访问策略。

    ![访问策略选项][12]        

1.  按照以下步骤根据访问策略管理任务︰

    - **添加新访问策略**-选择**添加**。 一旦生成，**访问策略**对话框中将显示新添加的访问策略 （默认设置）。
    - **编辑策略访问**的进行必要的编辑，并选择**保存**。
    - **删除访问策略**-选择**删除**旁边要移除的访问策略。

## <a name="set-the-public-access-level-for-a-blob-container"></a>为 blob 容器设置公共访问级别

默认情况下，每一个 blob 容器设置为"无公共访问权限"。

以下步骤说明了如何指定 public 访问级别为 blob 容器。

1.  打开存储资源管理器 （预览）。
1.  在左窗格中，展开包含您要管理其访问策略 blob 容器的存储帐户。
1.  展开存储帐户的**Blob 容器**。
1.  选择所需的 blob 容器，-并从上下文菜单中的选择**设置公共访问级别**。

    ![设置公共访问级别上下文菜单][13]

1.  在**设置容器的公共访问级别**对话框中，指定所需的访问级别。

    ![设置公共访问级别选项][14]

1.  选择**应用**。

## <a name="managing-blobs-in-a-blob-container"></a>管理 blob 容器中的 blob

一旦您创建了一个 blob 容器，可以将 blob 上载到该 blob 容器、 下载到本地计算机上的斑点、 打开本地计算机，以及更多的斑点。

以下步骤说明如何管理 blob （和文件夹） blob 容器内。

1.  打开存储资源管理器 （预览）。
1.  在左窗格中，展开包含 blob 容器要管理的存储帐户。
1.  展开存储帐户的**Blob 容器**。
1.  双击您要查看的 blob 容器。
1.  主窗格中将显示该 blob 容器的内容。

    ![视图的 blob 容器][3]

1.  主窗格中将显示该 blob 容器的内容。

1.  按照以下步骤根据您要执行的任务︰

    - **将文件上载到 blob 容器**

        1.  在主窗格中的工具栏上，从下拉菜单中选择**上载**，然后**上载文件**。

            ![上载的文件菜单][15]

        1.  在**上载文件**对话框中，选择**文件**文本框中，选择想要上传的文件右侧的省略号 （**...**）。

            ![上载文件选项][16]

        1.  指定**Blob 类型**的类型。 [有关使用.NET 的 Azure Blob 存储入门](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts)文章解释各种 blob 类型之间的差异。

        1.  （可选） 指定所选的文件将上载到其中的目标文件夹。 如果目标文件夹不存在，将创建它。

        1.  选择**上载**。

    - **将一个文件夹上载到 blob 容器**

        1.  在主窗格中的工具栏上，从下拉菜单中选择**上载**，然后**上载文件夹**。

            ![将上载文件夹菜单][17]

        1.  在**上载文件夹**对话框中，选择**文件夹**文本框中，选择想要上传其内容的文件夹右侧的省略号 （**...**）。

            ![将上载文件夹选项][18]

        1.  指定**Blob 类型**的类型。 [有关使用.NET 的 Azure Blob 存储入门](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts)文章解释各种 blob 类型之间的差异。

        1.  （可选） 指定所选的文件夹中的内容将被上载到其中的目标文件夹。 如果目标文件夹不存在，将创建它。

        1.  选择**上载**。

    - **下载到本地计算机上的斑点**

        1.  选择您要下载的 blob。

        1.  在主窗格中的工具栏上，选择**下载**。

        1.  在**指定要保存已下载的斑点的位置**对话框中，指定您希望下载，斑点的位置和您想要为其指定的名称。  

        1.  选择**保存**。

    - **打开本地计算机上的斑点**

        1.  选择要打开的 blob。

        1.  在主窗格中的工具栏上，选择**打开**。

        1.  该 blob 会下载并使用 blob 的基础文件类型关联的应用程序打开。

    - **复制到剪贴板的 blob**

        1.  选择要复制的 blob。

        1.  在主窗格中的工具栏上，选择**复制**。

        1.  在左窗格中，导航到另一个 blob 容器，并双击它，以在主窗格中查看它。

        1.  在主窗格中的工具栏上，选择**粘贴**创建 blob 的副本。

    - **删除一个 blob**

        1.  选择您想要删除该 blob。

        1.  在主窗格中的工具栏上，选择**删除**。

        1.  确认对话框时，选择**是**。

## <a name="next-steps"></a>下一步行动

- 查看[最新存储浏览器 （预览） 发行说明和视频](http://www.storageexplorer.com)。
- 了解如何[创建使用 Azure blob、 表、 队列和文件的应用程序](https://azure.microsoft.com/documentation/services/storage/)。

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png