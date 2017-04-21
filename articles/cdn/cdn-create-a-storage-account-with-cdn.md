<properties
    pageTitle="与 CDN 集成存储帐户 |Microsoft Azure"
    description="了解如何使用 Azure 内容传递网络 (CDN) 通过从 Azure 存储 blob 缓存提供高带宽内容。"
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="integrate-a-storage-account-with-cdn"></a>与 CDN 集成存储帐户

CDN 可以从 Azure 存储启用缓存内容。 它为开发人员提供提供高带宽内容缓存 blob 和静态内容的计算实例，在美国、 欧洲、 亚洲、 澳大利亚和南美的物理节点在全球解决方案。


## <a name="step-1-create-a-storage-account"></a>步骤 1︰ 创建存储帐户。

使用以下过程来创建新的存储帐户 Azure 订阅。 存储帐户有权访问 Azure 存储服务。 存储帐户代表访问了 Azure 存储服务组件的每个命名空间的最高级别︰ 服务、 队列服务和表服务的 Blob。 有关详细信息，请参阅[介绍 Microsoft Azure 存储](../storage/storage-introduction.md)。

若要创建存储帐户，您必须是服务管理员或相关联的订阅的联管理员。

> [AZURE.NOTE] 有几种方法可用于创建存储帐户，包括 Azure 门户和 Powershell。  对于本教程，我们将使用 Azure 门户。  

**若要创建订阅了 Azure 存储帐户**

1.  登录到[Azure 的门户](https://portal.azure.com)。
2.  在左上角，选择**新建**。 在**新建**对话框中，选择**数据 + 存储**，然后单击**存储帐户**。

    此时将显示**创建存储帐户**刀片式服务器。

    ![创建存储帐户][create-new-storage-account]

4. 在**名称**字段中，键入一个子域的名称。 此项可以包含 3-24 个小写字母和数字。

    此值将成为用于订阅的 Blob、 队列或表资源的地址的 URI 中的主机名称。 为解决斑点服务中的容器资源，URI 中使用下面的格式，其中*&lt;StorageAccountLabel&gt;*指的是您在**输入 URL**中键入的值︰

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **重要︰**URL 标签窗体存储帐户 URI 子域和 Azure 中的所有托管服务中必须唯一。

    此值还用作在门户中，或此存储帐户的名称以编程方式访问该帐户时。

5. **部署模型**、**帐户种类**、**性能**、 和**复制**中保留默认值。 

6. 选择将用于存储帐户**订阅**。

7. 选择或创建**资源组**。  有关资源组的详细信息，请参阅[Azure 资源管理器概述](azure-resource-manager/resource-group-overview.md#resource-groups)。

8. 选择您的存储帐户的位置。

8. 单击**创建**。 创建存储帐户的过程可能需要几分钟才能完成。


## <a name="step-2-create-a-new-cdn-profile"></a>步骤 2︰ 创建新的 CDN 配置文件

CDN 配置文件是一套 CDN 的终结点。  每个配置文件包含一个或多个 CDN 终结点。  您可能希望使用多个配置文件来组织您的 internet 域，web 应用程序或一些其他条件的 CDN 终结点。

> [AZURE.TIP] 如果您已经有您想要使用在本教程的 CDN 配置文件，请继续[步骤 3](#step-3-create-a-new-cdn-endpoint)。

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="step-3-create-a-new-cdn-endpoint"></a>步骤 3︰ 创建新的 CDN 终结点

**若要创建新存储帐户的 CDN 终结点**

1. 在[Azure 管理门户](https://portal.azure.com)中，导航到您 CDN 的配置文件。  您可能已经固定其到上一步中的仪表板。  如果您没有，您可以发现它通过单击**浏览**，然后**CDN 的配置文件**，然后单击要添加到配置文件。

    CDN 配置文件刀片式服务器出现。

    ![CDN 的配置文件][cdn-profile-settings]

2. 单击**添加终结点**按钮。

    ![添加终结点按钮][cdn-new-endpoint-button]

    此时将显示**添加终结点**刀片式服务器。

    ![添加终结点刀片式服务器][cdn-add-endpoint]

3. 输入此 CDN 终结点的**名称**。  此名称将用于访问您在域的缓存的资源`<endpointname>.azureedge.net`。

4. 在**源类型**下拉列表中选择*存储*。  

5. 在**来源主机名**下拉列表中选择您的存储帐户。

6. 让**原始路径**、**原始主机标头**和**原始协议/端口**的默认值。  您必须指定至少一个协议 （HTTP 或 HTTPS）。

    > [AZURE.NOTE] 此配置将使所有公共可见载体在 CDN 缓存存储帐户中。  如果您想要将范围限制为单个容器，使用**原始路径**。  请注意容器必须设为公共的可见性。

7. 单击**添加**按钮来创建新的终结点。

8. 创建终结点后，它的终结点配置文件列表中显示。 列表视图显示用于访问缓存的内容，以及原始域的 URL。

    ![CDN 终结点][cdn-endpoint-success]

    > [AZURE.NOTE] 终结点不立即可供使用。  它可能需要最多 90 分钟时间注册通过 CDN 网络传播。 通过 CDN 可用内容之前，请立即使用 CDN 的域名用户可能会收到状态代码 404。


## <a name="step-4-access-cdn-content"></a>第 4 步︰ 访问 CDN 的内容

若要访问在 CDN 缓存的内容，使用 CDN URL 提供的门户。 Blob 缓存的地址将类似如下︰

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\>

> [AZURE.NOTE] 一旦您启用 CDN 访问存储帐户或承载服务，所有公开可用的对象有资格 CDN 边缘缓存。 如果您修改当前缓存在 CDN 的对象时，新内容不能通过 CDN 直到 CDN 缓存内容生存时间段到期时刷新其内容。

## <a name="step-5-remove-content-from-the-cdn"></a>步骤 5︰ 从 CDN 中删除内容

如果您不再希望缓存对象在 Azure 内容传递网络 (CDN)，您可以执行下列步骤之一︰

-   您可以使容器而不是公钥的私钥。 有关更多信息，请参见[管理匿名的读访问，而对容器和 blob](../storage/storage-manage-access-to-resources.md) 。
-   您可以禁用或删除使用管理门户的 CDN 终结点。
-   您可以修改您的托管的服务不再响应请求的对象。

在 CDN 中已缓存的对象将保持缓存，直到对象的生存时间段到期或被清除该终结点。 当生存时间段到期时，将检查 CDN CDN 终结点是否仍然有效，仍然可以以匿名方式访问该对象，请参阅。 如果不是，然后不再缓存对象。


## <a name="additional-resources"></a>其他资源

-   [如何 CDN 的内容映射到自定义的域](cdn-map-content-to-custom-domain.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png

[cdn-profile-settings]: ./media/cdn-create-a-storage-account-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-a-storage-account-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-a-storage-account-with-cdn/cdn-endpoint-success.png
