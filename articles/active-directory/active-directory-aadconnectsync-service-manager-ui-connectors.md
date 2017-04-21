<properties
    pageTitle="Azure AD 连接同步︰ 同步服务管理器用户界面 |Microsoft Azure"
    description="理解 Azure AD 连接同步服务管理器中的连接器选项卡。"
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Azure AD 连接同步︰ 同步服务管理器

[操作](active-directory-aadconnectsync-service-manager-ui-operations.md) | [连接器](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [元节设计器](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [元搜索](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)
--- | --- | --- | ---

![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

连接器选项卡用于管理所有同步引擎连接到系统。

## <a name="connector-actions"></a>连接器的操作

操作 | 注释
--- | ---
创建 | 不要使用。 对于连接到其他 AD 林，使用安装向导。
属性 | 用于域和 OU 筛选。
[删除](#delete) | 用于删除连接器空间中的数据或删除连接到一个目录林。
[配置运行配置文件](#configure-run-profiles) | 除了域筛选，没有要在此处配置的内容。 此操作可用于已配置运行配置文件，请参阅。
运行 | 用于启动一次性运行配置文件。
停止 | 停止当前运行的配置文件的连接器。
输出连接器 | 不要使用。
导入接头 | 不要使用。
更新连接器 | 不要使用。
刷新架构 | 刷新缓存的架构。 因为它还更新同步规则是首选方法相反，使用安装向导中的选项。
[搜索连接器空间](#search-connector-space) | 用来查找对象并[按照对象和系统中的数据](#follow-an-object-and-its-data-through-the-system)。

### <a name="delete"></a>删除
删除操作适用于两种不同的事物。
![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

**删除连接器空间只有**选项将删除所有数据，但保留配置。

**删除连接器，连接器空间**选项删除数据和配置。 当您不希望不再连接到的目录林时，使用此选项。

这两个选项同步的所有对象，并且更新的元节对象。 此操作是一个长时间运行的操作。

### <a name="configure-run-profiles"></a>配置运行配置文件
此选项允许您为连接器配置运行配置文件，请参阅。

![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>搜索连接器空间
搜索连接器空间操作可用于查找对象并解决数据问题。

![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

首先，选择一个**范围**。 您可以搜索基于的数据 （RDN，DN，锚，子树） 或状态的对象 （所有其他选项）。  
![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
如果例如执行子树搜索，将会获得一个 OU 中的所有对象。
![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)此网格，您可以选择对象、 选择**属性**和源连接器空间中的空间，通过元，并向目标连接器空间[在其后](#follow-an-object-and-its-data-through-the-system)。

## <a name="follow-an-object-and-its-data-through-the-system"></a>按照对象和系统中的数据
在解决数据问题时，到了元节从源连接器空间中，按照对象和目标连接器空间是理解为什么数据没有预期值的关键过程。

### <a name="connector-space-object-properties"></a>连接器空间对象属性
**导入**  
Cs 对象打开时，有几个选项卡的顶部。 **导入**选项卡显示在导入后转移的数据。
![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/csimport.png)的**旧值**显示当前存储系统以及**新值**什么已接收到来自源系统 / 尚未应用。 在这种情况下，由于没有同步错误，则无法应用更改。

**错误**  
错误页才可见，如果没有与该对象的问题。 有关如何[解决同步错误](active-directory-aadconnectsync-service-manager-ui-operations.md#troubleshoot-errors-in-operations-tab)详细信息操作页面上查看详细信息。

**沿袭**  
沿袭选项卡显示的连接器空间对象与元节对象。 您可以查看当连接器上次导入更改连接的系统和应用哪些规则来填充在元节中的数据。
![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/cslineage.png)在**操作**列中，您可以看到一个**入站**同步规则与**规定**的操作。 表明，只要存在此连接器空间对象时，元节对象仍然保留。 如果同步规则列表改为显示同步规则与**出站**方向和**资源调配**，它指示删除元节对象时，删除此对象。
![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/cslineageout.png)还可以看到在**PasswordSync**列中的入站的连接器空间可以参与对密码的更改，因为一个同步规则具有的值**，则返回 True**。 此密码然后被发送到 Azure AD 通过出站规则。

从沿袭选项卡上，您可以通过单击[元节对象属性](#metaverse-object-properties)到了元节获得。

所有选项卡的底部有两个按钮︰**预览**和**日志**。

**预览**  
预览页用于同步的单个对象。 如果您正在进行故障排除的一些客户同步规则并希望在单个对象上看到更改的效果，它很有用。 您可以选择**完全同步**和**增量同步**之间。 您还可以选择**生成的预览**，它只保留在内存中的更改，并**提交预览**，所有的阶段更改为目标连接器空格之间。
![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/preview1.png)您可以检查对象，对于特定属性流应用哪个规则。
![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/preview2.png)

**日志**  
日志页用于查看密码同步状态和历史记录的详细信息，请参阅[疑难解答密码同步](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization)。

### <a name="metaverse-object-properties"></a>元节对象属性
**属性**  
在属性选项卡上可看到的值和哪些连接器提供它。
![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/mvattributes.png)
**连接器**  
连接器选项卡显示所有连接器空间对象的表示形式。
![同步服务管理器](./media/active-directory-aadconnectsync-service-manager-ui/mvconnectors.png)此选项卡还允许您导航到[的连接器空间对象](#connector-space-object-properties)。

## <a name="next-steps"></a>下一步行动
了解更多有关[Azure AD 连接同步](active-directory-aadconnectsync-whatis.md)配置。

了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
