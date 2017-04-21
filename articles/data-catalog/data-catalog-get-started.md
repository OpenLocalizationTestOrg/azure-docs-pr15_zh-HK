<properties
    pageTitle="开始使用数据目录 |Microsoft Azure"
    description="端到端方案和 Azure 数据目录的功能演示教程。"
    documentationCenter=""
    services="data-catalog"
    authors="steelanddata"
    manager="jhubbard"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/20/2016"
    ms.author="spelluru"/>

# <a name="get-started-with-azure-data-catalog"></a>开始使用 Azure 数据目录
Azure 数据目录是发现的一种完全托管的云服务，作为注册的系统和系统的企业数据资产。 有关的详细概述，请参阅[什么是 Azure 数据目录](data-catalog-what-is-data-catalog.md)。

本教程将帮助您开始使用 Azure 数据目录。 在本教程中，您执行以下过程︰

| 过程 | 说明 |
| :--- | :---------- |
| [设置数据目录](#provision-data-catalog) | 在此过程中，您可以设置或设置 Azure 数据目录。 只有当目录未建立之前，您可以执行此步骤。 即使有多个订阅您 Azure 帐户相关联，您可以只有一个数据目录，每个组织 （Microsoft Azure Active Directory 域）。 |
| [注册数据资产](#register-data-assets) | 在此过程中，则数据目录中注册 AdventureWorks2014 示例数据库中的数据资产。 注册是从数据源中提取关键的结构化元数据，如名称、 类型和位置并将该元数据复制到该目录的过程。 数据源和数据资产保持在其中，但目录使用的元数据以使其更容易被发现和理解。 |
| [发现数据资产](#discover-data-assets) | 在此过程中，您可以使用 Azure 数据目录入口发现上一步中注册的数据资产。 Azure 数据目录中注册的数据源后，它的元数据编制索引服务，以便用户可以轻松地搜索所需的数据。 |
| [批注的数据资产](#annotate-data-assets) | 在此过程中，您的数据资产提供注释 （信息，如说明、 标记、 文档或专家）。 该信息补充了从数据源中，并让更多的人更容易理解数据源中提取元数据。 |
| [连接到数据资产](#connect-to-data-assets) | 在此过程中，您可以打开集成的客户端工具 （如 Excel 和 SQL Server 数据工具） 中的数据资产和非集成的工具 (SQL Server 管理 Studio)。 |
| [管理数据资产](#manage-data-assets) | 在此过程中，您设置安全为您的数据资产。 数据目录并不赋予用户访问数据本身。 数据源的所有者来控制数据访问。 <br/><br/> 使用数据目录中，可以发现数据源和相关**元数据**目录中注册的源视图。 可能的情况下，但是，数据源应该是可见的只对特定的用户或特定组的成员。 对于这些情况，您可以使用数据目录目录中注册的数据资产的所有权和控制您所拥有的资产的可见性。 |
| [删除数据资产](#remove-data-assets) | 在此过程中，您将学习如何删除数据目录中的数据资产。 |  

## <a name="tutorial-prerequisites"></a>教程的系统必备组件

### <a name="azure-subscription"></a>Azure 的订阅
若要设置 Azure 数据目录，您必须是所有者或订阅了 Azure 的合作者。

Azure 订阅帮助您组织类似 Azure 数据目录的云服务资源的访问。 它们还可以帮助您控制资源使用状况的报告方式，记帐时间，以及支付的。 每个订阅可以具有不同的帐单和付款的安装，因此可以有不同的预订和不同的部门、 项目、 区域办事处，等计划。 每个云服务属于订阅，您需要设置 Azure 数据目录之前订购。 若要了解详细信息，请参阅[管理帐户、 预订和管理角色](../active-directory/active-directory-how-subscriptions-associated-directory.md)。

如果没有预订，您可以在几分钟创建免费的试用帐户。 [免费试用版](https://azure.microsoft.com/pricing/free-trial/)的详细信息，请参见

### <a name="azure-active-directory"></a>Azure 的活动目录
若要设置 Azure 数据目录，必须登录到 Azure 活动目录 (AD Azure) 用户帐户。 您必须是所有者或订阅了 Azure 的合作者。  

Azure 的广告提供了一种简单的方法为您的业务来管理身份和访问，同时在云和内部。 可以使用单个工作或学校的帐户登录到任何云或内部部署 web 应用程序。 Azure 数据目录使用 Azure 的广告来对登录进行身份验证。 若要了解详细信息，请参阅[什么是 Azure 活动目录](../active-directory/active-directory-whatis.md)。

### <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory 策略配置

在您可以登录到 Azure 数据目录入口，但当您尝试登录到数据源的注册工具时，您可能会遇到一种情况，您会遇到错误消息阻止您登录。 当您在公司网络上或者从公司网络外部连接时，可能会发生此错误。

注册工具使用*窗体身份验证*来验证用户登录对 Azure Active Directory。 为成功登录，Azure Active Directory 管理员必须启用窗体身份验证中*全局身份验证策略*。

与全局身份验证策略，您可以启用身份验证，分别用于内部网和外部网连接，如下图中所示。 如果要连接的网络未启用窗体身份验证，则可能会出现登录错误。

 ![Azure Active Directory 全局身份验证策略](./media/data-catalog-prerequisites/global-auth-policy.png)

有关详细信息，请参阅[配置身份验证策略](https://technet.microsoft.com/library/dn486781.aspx)。

## <a name="provision-data-catalog"></a>设置数据目录
您可以设置每个组织 （Azure Active Directory 域） 只能有一个数据目录。 因此，如果所有者或共同属于此 Azure Active Directory 域 Azure 的订阅的所有者已经创建了一个目录，您将不能再次创建一个目录，即使有多个 Azure 的订阅。 要测试是否通过 Azure Active Directory 域中的用户而创建的数据目录，请转到[Azure 数据目录主页](http://azuredatacatalog.com)并验证您是否看到目录。 如果已为您创建一个目录，请跳过以下过程并转到下一节。    

1. 转到[数据目录服务页](https://azure.microsoft.com/services/data-catalog)，然后单击**开始**。

    ![Azure 数据目录-营销登录页](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. 使用是所有者或共同 Azure 的订阅的所有者的用户帐户登录。 您登录后看到下面的页。

    ![Azure 数据目录-提供数据目录](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. 指定**名称**的数据目录，**订购**所需和目录的**位置**。
4. 展开**定价**并选择 Azure 数据目录**版本**（闲或标准）。
    ![Azure 数据目录-选择版](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. 展开**目录的用户**，然后单击**添加**以添加数据目录的用户。 您将自动添加到此组。
    ![Azure 数据目录-用户](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. 展开**目录管理员**，然后单击**添加**来添加其他的管理员的数据目录。 您将自动添加到此组。
    ![Azure 数据目录-管理员](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. 单击**创建目录**以创建您的组织的数据目录。 创建后，您会看到数据目录主页。
    ![Azure 数据目录创建](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-the-azure-portal"></a>在 Azure 门户中查找数据目录
1. 在单独的选项卡中，在 web 浏览器中或在单独的 web 浏览器窗口中，转到[Azure 门户](https://portal.azure.com)并使用上一步中创建数据目录的同一帐户登录。
2. 选择**浏览**，然后单击**数据目录**。

    ![Azure 数据目录-浏览 Azure](media/data-catalog-get-started/data-catalog-browse-azure-portal.png)您看到您创建的数据目录。

    ![Azure 数据目录-查看列表中的目录](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
4.  单击您创建的目录。 您会看到**数据目录**刀片式服务器中的门户。

    ![Azure 数据目录-刀片式服务器在门户网站中 ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
5. 可以查看数据目录的属性，并对其进行更新。 例如，单击**定价层**和更改版本。

    ![Azure 数据目录-定价层](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>探险工作示例数据库
在本教程中，（表） 的数据资产 AdventureWorks2014 示例数据库中注册的 SQL Server 数据库引擎，但如果您愿意使用熟悉并且与您的角色相关的数据，则可以使用任何支持的数据源。 支持的数据源的列表，请参阅[支持的数据源](data-catalog-dsr.md)。

### <a name="install-the-adventure-works-2014-oltp-database"></a>探险工作 2014 OLTP 数据库安装
艾德数据库为虚构的自行车制造商 （探险工作周期），其中包括产品、 销售和采购支持标准的联机事务处理方案。 在本教程中，您可以注册产品信息到 Azure 数据目录。

若要安装艾德示例数据库︰

1. 下载在 CodePlex 上[探险工作 2014年全文数据库 Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) 。
2. 若要还原您的计算机上的数据库，按照说明在[通过 SQL Server 管理 Studio 数据库备份还原](http://msdn.microsoft.com/library/ms177429.aspx)，或通过执行以下步骤︰
    1. 打开 SQL Server 管理 Studio 并连接到 SQL Server 数据库引擎。
    2. 右键单击**数据库**，然后单击**还原数据库**。
    3. 在**还原数据库**，请单击**源****设备**选项，单击**浏览**。
    4. **选择备份设备**，请在下单击**添加**。
    5. 转到其中有**AdventureWorks2014.bak**文件，选择该文件，并单击**确定**以关闭**查找备份文件**对话框中的文件夹。
    6. 单击**确定**以关闭**选择备份设备**对话框。    
    7. 单击**确定**以关闭**还原数据库**对话框。

现在，您可以通过使用 Azure 数据目录注册艾德示例数据库中的数据资产。

## <a name="register-data-assets"></a>注册数据资产

在本练习中，您可以使用注册工具目录中注册艾德数据库中的数据资产。 注册是从数据源和它所包含的资产中提取关键的结构化元数据，如名称、 类型和位置并将该元数据复制到该目录的过程。 数据源和数据资产保持在其中，但目录使用的元数据以使其更容易被发现和理解。

### <a name="register-a-data-source"></a>注册数据源

1.  转到[Azure 数据目录主页](https://azuredatacatalog.com)并单击**发布数据**。

    ![Azure 数据目录 — 发布数据按钮](media/data-catalog-get-started/data-catalog-publish-data.png)

2.  单击以下载、 安装和运行您的计算机上注册工具**启动应用程序**。

    ![Azure 数据目录-启动按钮](media/data-catalog-get-started/data-catalog-launch-application.png)

3. 在**欢迎**页上，单击**登录**并输入您的凭据。    

    ![Azure 数据目录-欢迎页](media/data-catalog-get-started/data-catalog-welcome-dialog.png)

4. 在**Microsoft Azure 数据目录**页中，单击**SQL Server**和**下一步**。

    ![Azure 数据目录-数据源](media/data-catalog-get-started/data-catalog-data-sources.png)

5.  **AdventureWorks2014**中输入 SQL Server 连接属性 （请参见下面的示例），然后单击**连接**。

    ![Azure 数据目录-SQL Server 连接设置](media/data-catalog-get-started/data-catalog-sql-server-connection.png)

6.  注册您的数据资产的元数据。 在此示例中，您可以注册**生产/产品**AdventureWorks 生产命名空间中的对象︰

    1. 在**服务器层次结构**树中，展开**AdventureWorks2014** ，然后单击**生产**。
    2. 通过使用 Ctrl + 单击来选择**产品**、**为**、 **ProductDescription**和**ProductPhoto** 。
    3. 单击**移动选定的箭头**(**>**)。 此操作将所有选定的对象移动到**要注册的对象**列表。

        ![Azure 数据目录教程-浏览并选择对象](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
    4. 选择要包含的数据的快照预览**包含预览**。 快照中包括的每个表，记录最多 20 个，则将其复制到该目录。
    5. 选择要包括的数据配置文件对象的统计信息的快照的**包含数据配置文件**(例如︰ 列，行数的最小值、 最大和平均值)。
    6. 在**添加标签**字段中，输入**通恒，周期**。 此操作将添加搜索标记这些数据资产。 标记是好的方法来帮助用户找到已注册的数据源。
    7. 在此数据 （可选） 指定的**专业**名称。

        ![Azure 数据目录教程-要注册的对象](media/data-catalog-get-started/data-catalog-objects-register.png)

    8. 单击**注册**。 Azure 数据目录中注册选定的对象。 在本练习中，所选的对象从艾德进行注册。 注册工具元数据提取的数据资产，并将该数据复制到 Azure 数据目录服务。 在当前驻留，并会在管理员的控制和当前系统的策略下，保持数据的保持。

        ![Azure 数据目录-注册对象](media/data-catalog-get-started/data-catalog-registered-objects.png)

    9. 若要查看已注册的数据源对象，请单击**查看门户**。 在 Azure 数据目录门户中，确认您看到所有四个表，并在网格视图的数据库。

        ![Azure 数据目录入口中的对象 ](media/data-catalog-get-started/data-catalog-view-portal.png)


在本练习中，您注册艾德示例数据库中的对象，以便它们可以轻松地跨组织由用户发现。 在下一个练习中，您将学习如何发现已注册的数据资产。

## <a name="discover-data-assets"></a>发现数据资产
在 Azure 数据目录中的搜索功能使用两种主要机制︰ 搜索和筛选。

被旨在作为直观、 功能强大的搜索。 默认情况下，搜索词匹配对照目录，包括用户提供的批注的任何属性。

筛选用于补充搜索。 您可以选择特定的特征，如专家、 数据源类型、 对象类型和标记以查看匹配的数据资源和约束匹配资源的搜索结果。

通过使用搜索和筛选的组合，可以快速导航的数据源的已注册的 Azure 数据目录，以查找您需要的数据资产。

在本练习中，您可以使用 Azure 数据目录入口发现您在前面的练习中注册的数据资产。 有关搜索语法的详细信息，请参阅[数据目录搜索语法参考](https://msdn.microsoft.com/library/azure/mt267594.aspx)。

下面是一些示例，用于发现数据在目录中的资产。  

### <a name="discover-data-assets-with-basic-search"></a>发现使用基本搜索的数据资产
基本搜索将帮助您使用一个或多个搜索条件来搜索目录。 结果是与一个或多个指定的条款的任何属性匹配的任何资产。

1. 在 Azure 数据目录门户，单击**主页**。 如果您关闭 web 浏览器，请转到[Azure 数据目录主页](https://www.azuredatacatalog.com)。
2. 在搜索框中，输入`cycles`，然后按**enter 键**。

    ![Azure 数据目录-基本文本搜索](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. 确认，您会看到所有四个表和数据库 (AdventureWorks2014) 在结果中。 可以通过单击工具栏上的按钮，如下图中所示**网格视图**和**列表视图**之间进行切换。 请注意，搜索关键字将突出显示在搜索结果中因为**突出显示**选项设置为**ON**。 您还可以在搜索结果中指定的**每页的结果**数。

    ![Azure 数据目录-基本文本搜索结果](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)

    **搜索**面板位于左侧，位于右侧的**属性**面板中。 在**搜索**面板中，可以更改搜索条件和筛选结果。 **属性**面板显示在网格或列表中所选对象的属性。

4. 在搜索结果中，请单击**产品**。 单击**预览**、**列**、**数据配置文件**和**文档**选项卡，或单击箭头以展开底部窗格。  

    ![Azure 数据目录-底部窗格](media/data-catalog-get-started/data-catalog-data-asset-preview.png)

    在**预览**选项卡上，您将看到**产品**表中的数据的预览。  
5. 单击**列**选项卡来查找详细信息 （如**名称**和**数据类型**） 的列在数据资产。
6. 单击**数据配置文件**选项卡以查看分析数据 (例如︰ 数行，大小的数据或在某一列中的最小值) 数据资产中。
7. 通过在左侧使用**筛选器**筛选结果。 例如，单击**表****对象类型**，并且您只查看四个表，不是数据库。

    ![Azure 数据目录-过滤搜索结果](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>发现与属性范围的数据资产
属性作用域可帮助您发现其中与指定的属性匹配搜索条件的数据资产。

1. 清除**表**筛选器在**筛选器**中的**对象类型**。  
2. 在搜索框中，输入`tags:cycles`，然后按**enter 键**。 可用于搜索数据目录的所有属性，请都参阅[数据目录搜索语法参考](https://msdn.microsoft.com/library/azure/mt267594.aspx)。
3. 确认，您会看到所有四个表和数据库 (AdventureWorks2014) 在结果中。  

    ![数据目录-搜索结果范围的属性](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>保存搜索
1. 在**当前搜索**部分中的**搜索**窗格中，输入搜索的名称，然后单击**保存**。

    ![Azure 数据目录-保存搜索](media/data-catalog-get-started/data-catalog-save-search.png)
2. 请确认已保存的搜索**保存的搜索**下显示。

    ![Azure 数据目录-保存的搜索](media/data-catalog-get-started/data-catalog-saved-search.png)
3. 选择一个已保存的搜索 （**重命名**、**删除**、 搜索**将保存为默认值**） 可以执行的操作。

    ![Azure 数据目录-保存搜索选项](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>布尔运算符
您可以扩大或缩小您的搜索与布尔运算符。

1. 在搜索框中，输入`tags:cycles AND objectType:table`，然后按**enter 键**。
2. 确认您看到只表 （而不是数据库） 在结果中。  

    ![Azure 数据目录-布尔搜索运算符](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>带括号的分组
通过使用括号分组，您可以分组实现逻辑隔离，尤其是和布尔运算符的查询的部分。

1. 在搜索框中，输入`name:product AND (tags:cycles AND objectType:table)`，然后按**enter 键**。
2. 确认您看到**Product**表将在搜索结果中。

    ![Azure 数据目录-分组搜索](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>比较运算符
比较运算符，您可以使用相等以外的比较具有数字和日期数据类型的属性。

1. 在搜索框中，输入`lastRegisteredTime:>"06/09/2016"`。
2. 清除在**对象类型**下的**表**筛选器。
3. 按**enter 键**。
4. 确认您看到的**产品**、**为**、 **ProductDescription**和**ProductPhoto**的表，您在搜索结果中注册的 AdventureWorks2014 数据库。

    ![Azure 数据目录-比较搜索结果](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

发现数据资产和[数据目录搜索语法引用](https://msdn.microsoft.com/library/azure/mt267594.aspx)搜索语法的详细信息，请参阅[如何发现数据资产](data-catalog-how-to-discover.md)。

## <a name="annotate-data-assets"></a>批注的数据资产
在本练习中，您使用 Azure 数据目录门户添加批注 （添加信息，如说明、 标记或专家） 以前已在目录中注册的数据资产。 注释补充和增强在注册期间从数据源中提取结构化元数据和数据资产非常容易发现和理解。

在本练习中，您将注释一单个数据资产 (ProductPhoto)。 ProductPhoto 数据资产中添加一个友好名称和说明。  

1.  转到[Azure 数据目录主页](https://www.azuredatacatalog.com)和搜索与`tags:cycles`找到已注册的数据资产。  
2. 在搜索结果中单击**ProductPhoto** 。  
3. 输入的**友好名称**和**营销材料的产品照片****产品图像****说明**。

    ![Azure 数据目录-ProductPhoto 说明](media/data-catalog-get-started/data-catalog-productphoto-description.png)

    **说明**可帮助其他人发现并了解其中的原因，以及如何使用所选的数据资产。 此外可以添加更多标记，然后查看列。 现在，您可以尝试搜索和过滤功能可以使用描述性元数据已添加到编录中发现数据资产。

您还可以执行下列操作在此页上︰

- 添加数据资产的专家。 单击**添加**方面**的专家**。
- 在数据集级别添加标记。 单击**添加****标签**区域中。 标记可以是用户标记或词汇表标记。 标准版本的数据目录包括业务术语表，可帮助定义一个中心业务分类目录管理员。 目录用户可以然后注释术语表的数据资产。 有关详细信息，请参阅[如何设置控制标记的业务术语表](data-catalog-how-to-business-glossary.md)
- 列级别添加标记。 您想要添加批注的列**标签**下单击**添加**。
- 列级别添加描述。 输入列的**说明**。 您还可以查看描述元数据从数据源中提取。
- 添加**请求访问**信息，向用户显示如何请求访问数据资产。

    ![Azure 数据目录-添加标签，说明](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)

- 选择**文档**选项卡，并提供文档数据资产。 Azure 数据目录文档可用数据目录作为一个内容存储库来创建您的数据资产的完整叙述。

    ![Azure 数据目录-文档选项卡](media/data-catalog-get-started/data-catalog-documentation.png)


您还可以对多个数据资产添加批注。 例如，可以选择注册您的所有数据资产和指定为这些专家。

![Azure 数据目录-对多个数据资产进行批注](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Azure 数据目录支持注释的人群采购方法。 数据目录中的任何用户可以添加标签 （用户或术语表），描述和其他元数据，以使数据资产和其使用的角度来看具有任何用户可以捕获并可供其他用户使用的角度来看。

有关添加批注的数据资产的详细信息，请参阅[如何批注数据资产](data-catalog-how-to-annotate.md)。

## <a name="connect-to-data-assets"></a>连接到数据资产
在本练习中，您通过使用连接信息打开在一个集成的客户端工具 (Excel) 和非集成的工具 (SQL Server 管理 Studio) 的数据资产。

> [AZURE.NOTE] 请务必记住，Azure 数据目录并不授予您访问实际数据源 — — 它只是使您更容易地发现并理解它。 连接到数据源时，您选择的客户端应用程序使用您的 Windows 凭据，或提示您提供凭据所需。 如果您未先前被授予访问权限的数据源，则需要被授予访问权限，然后才能连接。

### <a name="connect-to-a-data-asset-from-excel"></a>从 Excel 连接到数据资产

1. 从搜索结果中选择**产品**。 单击工具栏上**打开**，单击**Excel**。

    ![Azure 数据目录-连接到数据资产](media/data-catalog-get-started/data-catalog-connect1.png)
2. 单击**打开**下载弹出窗口中。 这种体验可能会有所不同，这取决于浏览器。

    ![Azure 数据目录-下载连接的 Excel 文件](media/data-catalog-get-started/data-catalog-download-open.png)
3. 在**Microsoft Excel 安全通知**窗口中，单击**启用**。

    ![Azure 数据目录-Excel 安全弹出菜单](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. 将在**导入数据**对话框中的默认值并单击**确定**。

    ![Azure 数据目录-Excel 导入数据](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. 在 Excel 中查看数据源。

    ![Azure 数据目录-Excel 中的产品表](media/data-catalog-get-started/data-catalog-connect2.png)

在本练习中，您将连接到发现使用 Azure 数据目录的数据资产。 Azure 数据目录入口中，您可以直接通过使用客户端应用程序集成到**中打开**的菜单连接。 您还可以选择使用连接的位置信息资产元数据中包含所有应用程序的连接。 例如，您可以使用 SQL Server 管理 Studio 连接到 AdventureWorks2014 数据库访问中在本教程中注册的数据资产的数据。

1. 打开**SQL Server 管理 Studio**。
2. 在**连接到服务器**对话框中，从 Azure 数据目录门户中的**属性**窗格中输入服务器名称。
3. 使用适当的身份验证和凭据访问数据资产。 如果您没有访问权限，使用在**请求访问**字段信息即可使用它。

    ![Azure 数据目录-请求访问](media/data-catalog-get-started/data-catalog-request-access.png)

单击**视图连接字符串**，以便查看并复制到剪贴板上，以便在您的应用程序中使用 ADF.NET、 ODBC 和 OLEDB 连接字符串。

## <a name="manage-data-assets"></a>管理数据资产
在此步骤中，您将了解如何设置您的数据资产的安全性。 数据目录并不赋予用户访问数据本身。 数据源的所有者来控制数据访问。

发现数据源并查看目录中注册的源与相关的元数据，可以使用数据目录。 可能的情况下，但是，在数据源只应可见到特定的用户或特定组的成员。 对于这些情况，您可以使用数据目录目录中注册的数据资产的所有权，然后控制的可见性资产属于您。

> [AZURE.NOTE] 在本练习中所述的管理功能是仅在标准版的 Azure 数据目录中进行，而不是在免费版。
在 Azure 数据目录中，可以获得数据资产的所有权、 共同所有者添加数据资产，并设置数据资产的可见性。

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>数据资产的所有权，并限制可见性

1. 转到[Azure 数据目录主页](https://www.azuredatacatalog.com)。 在**搜索**文本框中，输入`tags:cycles`，然后按**enter 键**。
2. 单击结果列表中的项目，然后单击工具栏上的**取得所有权**。
3. 在**属性**面板中的**管理**部分中，单击**取得所有权**。

    ![Azure 数据目录-取得所有权](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. 要限制可见性，请在**可见性**部分中选择**所有者和这些用户**，然后单击**添加**。 在文本框中输入用户的电子邮件地址，然后按**ENTER**。

    ![Azure 数据目录-限制访问](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>删除数据资产

在本练习中，您可以使用 Azure 数据目录门户预览数据删除已注册的数据资产，并从目录中删除的数据资产。

在 Azure 数据目录中，您可以删除一个单独的资源或删除多个资产。

1. 转到[Azure 数据目录主页](https://www.azuredatacatalog.com)。
2. 在**搜索**文本框中，输入`tags:cycles`，然后单击**ENTER**。
3. 结果列表中选择一项然后单击**删除**工具栏上如下图所示︰

    ![Azure 数据目录-删除网格项](media/data-catalog-get-started/data-catalog-delete-grid-item.png)

    如果您使用列表视图，该复选框是项的左侧，如下图所示︰

    ![Azure 数据目录-删除列表项](media/data-catalog-get-started/data-catalog-delete-list-item.png)

    此外可以选择多个数据资源，然后删除它们，如下图所示︰

    ![Azure 数据目录-删除多个数据资产](media/data-catalog-get-started/data-catalog-delete-assets.png)


> [AZURE.NOTE] 该目录的默认行为是允许任何用户注册任何数据源，并允许用户删除任何已注册的数据资产。 包括在标准版的 Azure 数据目录管理功能提供更多的选项，来接管资产，限制可以发现资产，以及限制谁可以删除资产。


## <a name="summary"></a>摘要

在本教程中，您研究了 Azure 数据目录，包括注册、 添加注释、 发现，和管理企业数据资产的基本功能。 现在，您已经完成本教程，就可以开始。 您可以通过注册的数据源，您和您的团队依赖，并邀请同事可以使用目录今天开始。

## <a name="references"></a>引用

- [如何注册数据资产](data-catalog-how-to-register.md)
- [如何发现数据资产](data-catalog-how-to-discover.md)
- [如何进行批注的数据资产，](data-catalog-how-to-annotate.md)
- [如何记录数据资产](data-catalog-how-to-documentation.md)
- [如何连接到数据资产](data-catalog-how-to-connect.md)
- [如何管理数据资产](data-catalog-how-to-manage.md)
