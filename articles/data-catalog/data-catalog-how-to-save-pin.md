<properties
   pageTitle="如何保存搜索和固定数据资产 |Microsoft Azure"
   description="如何文章突出功能在 Azure 数据目录中的保存数据源和数据供以后重复使用的资产。"
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/10/2016"
   ms.author="maroche"/>

# <a name="how-to-save-searches-and-pin-data-assets"></a>如何保存搜索和固定数据资产

## <a name="introduction"></a>介绍

Microsoft Azure 数据目录提供了对数据源查询的功能。 用户可以快速搜索和筛选该目录，以查找数据源，并了解其预期的目的，以便更容易找到正确的数据对于手头的工作。

但是，用户需要经常使用相同的数据在什么时候呢？ 用户定期贡献其知识应用到目录中的相同数据源如何呢？ 在这些情况下，无需重复发出相同的搜索可能效率不高--这是在已保存的搜索和固定的数据资产可以帮助。

## <a name="saved-searches"></a>已保存的搜索

已保存的搜索在 Azure 数据目录是可重复使用，每个用户搜索定义。 一旦用户已定义搜索 （包括搜索条件、 标签和其他筛选器） 他可以保存它以供将来使用。 已保存的搜索定义然后可重新运行在更晚的日期，以返回符合其搜索条件的所有数据资产。

### <a name="creating-a-saved-search"></a>创建已保存的搜索

若要创建已保存的搜索，首先输入搜索条件以重新使用。 然后单击"保存"链接在 Azure 数据目录门户中的"当前搜索"框中。

 ![选择保存以保存当前的搜索设置](./media/data-catalog-how-to-save-pin/01-save-option.png)

出现提示时，输入已保存搜索的名称。 选择一个有意义且将由搜索返回的数据资产的描述性的名称。

 ![提供已保存搜索的名称](./media/data-catalog-how-to-save-pin/02-name.png)

### <a name="managing-saved-searches"></a>管理保存的搜索

一旦用户已保存一个或多个搜索"保存的搜索"选项将显示在"当前搜索"框下的 Azure 数据目录入口。 在展开时，将显示保存的搜索的完整列表。

 ![已保存的搜索的列表](./media/data-catalog-how-to-save-pin/03-list.png)

从列表中选择已保存的搜索时，将导致执行搜索。

选择下拉列表菜单将提供一组管理选项︰

 ![管理保存的搜索结果的选项](./media/data-catalog-how-to-save-pin/04-managing.png)

选择"重命名"时，将提示用户输入一个新名称保存搜索。 搜索定义不会更改。

选择"删除"将提示用户进行确认，并将再从用户列表中删除已保存的搜索。

选择"保存为默认值"会将保存为默认搜索用户的搜索所选标记。 如果用户从 Azure 数据目录主页上执行一个"空"的搜索，用户的默认搜索将会执行。 此外，搜索标记为默认值将出现在已保存的搜索列表的顶部。

### <a name="organizational-saved-searches"></a>组织已保存的搜索

每个用户可以保存搜索，供自己使用。 数据目录管理员还可以保存搜索组织内的所有用户。 保存搜索，管理员会显示一个选项共享已保存的搜索在公司内。 如果选择此选项，将对所有用户可用的搜索列表中包括已保存的搜索。

 ![组织已保存的搜索](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)


## <a name="pinned-data-assets"></a>固定的数据资产

已保存的搜索允许用户保存和重复使用搜索定义;由搜索返回的数据资产可能会更改目录更改的内容随着时间的推移。 固定数据资产，用户可以显式标识特定的数据资产，以使它们能够更轻松地访问而无需使用搜索。

固定数据资产非常简单--用户可以只需单击以将其添加到他们的固定列表数据资产的"图钉"图标。 此图标显示在资产平铺在平铺视图中，并在 Azure 数据目录门户中的列表视图中最左边的列中的角。

![固定数据资产](./media/data-catalog-how-to-save-pin/05-pinning.png)

解除锁定资产同样很简单，用户只需单击"图钉"图标再次打开此设置所选资产。

![解除锁定数据资产](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="my-assets"></a>"我的资产"
Azure 数据目录门户主页上包括"我的资产"部分，对当前用户显示资产感兴趣。 本部分包括两个固定的资产和已保存的搜索。

![我的资产的主页上](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>摘要
Azure 数据目录提供了使用户更容易地发现他们所需的数据源，以便他们可以花费更少的时间寻找数据和更多的时间与它合作的能力。 已保存的搜索并锁定资产生成基于这些核心功能，以便用户可以轻松地识别它们将使用重复数据源的数据。
