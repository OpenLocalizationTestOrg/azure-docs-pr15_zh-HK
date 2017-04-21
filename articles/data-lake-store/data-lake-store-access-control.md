<properties
   pageTitle="数据湖存储区中的访问控制概述 |Microsoft Azure"
   description="了解如何访问 Azure 数据湖存储区中的控件"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="access-control-in-azure-data-lake-store"></a>Azure 数据湖存储区中的访问控制

数据湖商店实现派生反过来从 HDFS，和 POSIX 的访问控制模型的访问控制模型。 本文总结了数据湖存储区的访问控制模型的基础知识。 若要了解更多有关 HDFS 的访问控制模型，请参见[HDFS 权限指南](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)。

## <a name="access-control-lists-on-files-and-folders"></a>文件和文件夹上的访问控制列表

有两种类型的访问控制列表 (Acl) 的**Acl 访问权限**和**默认 Acl**。

* **访问权限 Acl** – 这些控件对象的访问权限。 文件和文件夹具有访问权限 Acl。

* **默认 Acl** –"模板"的 Acl 与文件夹相关联，以确定在该文件夹下创建所有子项目的访问权限 Acl。 文件没有默认 Acl。

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Acl 访问权限和默认 Acl 具有相同的结构。

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-acls-2.png)

>[AZURE.NOTE] 更改父级中的默认 ACL 不会影响访问 ACL 或已经存在的子项的默认 ACL。

## <a name="users-and-identities"></a>用户和标识

每个文件和文件夹具有不同权限的这些标识︰

* 该文件的负责人用户
* 所属组
* 命名的用户
* 已命名的组
* 所有其他用户

用户和组的标识都是 Azure 活动目录 (AAD) 的身份，因此除非另有说明"用户"，在上下文中数据湖存储区，或者意味着 AAD 用户或 AAD 的安全组。

## <a name="permissions"></a>权限

文件系统对象上的权限是**读取**、**写入**和**执行**，并且可以使用文件和文件夹下表中所示。

|            |    文件     |   文件夹 |
|------------|-------------|----------|
| **Read (R)** | 可以读取文件的内容 | 需要**读取**和**执行**列出文件夹的内容。|
| **写入 (W)** | 可以写入或追加到文件 | 需要**编写和执行**在文件夹中创建子项目。 |
| **执行 (X)** | 并不意味着数据湖存储的上下文中的任何内容 | 需要遍历的文件夹的子项目。 |

### <a name="short-forms-for-permissions"></a>缩写形式的权限

**RWX**用于指示**读取 + 写入 + 执行**。 更简洁的数字形式存在的**读 = 4**，**编写 = 2**，和**执行 = 1**和其总和表示的权限。 下面举了一些例子。

| 数字形式 | 短格式 |      它意味着什么     |
|--------------|------------|------------------------|
| 7            | RWX        | 读取 + 写入 + 执行 |
| 5            | R-X        | 读取 + 执行         |
| 4            | R —        | 读取                   |
| 0            | ---        | 无权限         |


### <a name="permissions-do-not-inherit"></a>不继承权限

在 POSIX 样式模型中使用的数据湖商店，对项目的权限存储在项目本身。 换句话说，无法从父项继承权限的项。

## <a name="common-scenarios-related-to-permissions"></a>与权限相关的常见方案

以下是一些常见的情况，以了解需要哪些权限对数据湖存储帐户执行某些操作。

### <a name="permissions-needed-to-read-a-file"></a>读取一个文件所需的权限

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* 要读取的文件调用方需要**读取**权限
* 调用方需要**执行**权限的文件夹结构中所有包含该文件的文件夹

### <a name="permissions-needed-to-append-to-a-file"></a>追加到文件所需的权限

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* 要追加的文件调用方需要**写**权限
* 为所有文件夹，其中包含该文件的调用方需要**执行**权限

### <a name="permissions-needed-to-delete-a-file"></a>删除文件所需的权限

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* 对父文件夹中的调用方需要**写入 + 执行**权限
* 对于所有其他文件夹中的文件的路径-调用方需要**执行**权限

>[AZURE.NOTE] 写入文件的权限，则不需要，只要上述两个条件都为真，则删除该文件。

### <a name="permissions-needed-to-enumerate-a-folder"></a>枚举文件夹所需的权限

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* 要枚举的文件夹调用方需要**读 + 执行**权限
* 所有祖先文件夹-调用方都需要**执行**权限

## <a name="viewing-permissions-in-the-azure-portal"></a>在 Azure 门户的查看权限

从数据湖存储帐户的**数据资源管理器**刀片，请单击以查看文件或文件夹的 Acl 的**访问**。 在下面的屏幕快照，请参阅**mydatastore**帐户下的**目录**文件夹的 Acl 的访问权限。

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

在此之后，**访问**刀片式服务器，单击**简单视图**，以查看简单的视图。

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

单击**高级视图**以查看更多高级的视图。

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="the-super-user"></a>超级用户

超级用户具有最大权限的所有用户数据湖存储区中。 超级用户︰

* 对**所有**文件和文件夹具有 RWX 权限
* 可以更改的任何文件或文件夹上的权限。
* 可以更改所属用户或所属组的任何文件或文件夹。

在 Azure，数据湖存储帐户都有几个 Azure 角色︰

* 所有者
* 参与者
* 读取器
* 等。

数据湖存储帐户的**所有者**角色中的每个人会自动为该帐户的超级用户。 若要了解更多有关 Azure 角色基于访问控制 (RBAC) 请参见[基于角色的访问控制](../active-directory/role-based-access-control-configure.md)。

## <a name="the-owning-user"></a>拥有的用户

创建项目的用户将自动所属项的用户。 拥有的用户可以︰

* 更改所拥有的文件的权限
* 只要拥有的用户也是目标组的成员，请更改所拥有的文件所属的组。

>[AZURE.NOTE] 拥有用户**不能**更改另一个附属文件的属主用户。 只有超级用户才可以更改文件或文件夹所属的用户。

## <a name="the-owning-group"></a>所属组

在 POSIX Acl 中，每个用户都关联着"主要组"。 例如，用户"alice"可能属于"财务"组。 Alice 可能属于多个组，但一组始终被指定为她的主要组。 POSIX，Alice 创建一个文件，该文件所属的组设置为其主要的组，在这种情况下是"财务"。
 
当创建新的文件系统项时，数据湖商店将值赋给所属的组。 

* **案例 1** -根文件夹"/"。 创建数据湖存储帐户时，会创建此文件夹。 在这种情况下所属的组设置为创建该帐户的用户。
* **案例 2**（其他所有情况下）-创建新项目时，将从父文件夹复制所属的组。

可以通过更改所属的组︰
* 超级用户
* 拥有，如果用户所属的用户也是目标组的成员。

## <a name="access-check-algorithm"></a>访问检查算法

下图显示了数据湖存储帐户访问检查算法。

![数据湖存储 Acl 算法](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="the-mask-and-effective-permissions"></a>掩码和"有效权限"

**掩码**是用于执行访问检查算法时限制访问**命名用户****所属组**，以及**命名组**RWX 值。 以下是为掩码的重要概念。 

* 掩码创建"有效权限"，它就是在访问检查时修改权限。
* 掩码可以按文件的所有者和任何超级用户直接编辑。
* 掩码有删除权限以创建有效的权限的能力。 掩码**不可以**添加权限的有效权限。 

让我们来看一些示例。 下面，掩码设置为**RWX**，这意味着掩码不移除任何权限。 指定的用户所属的组，以及命名的组的有效权限不会改变访问检查时的通知。

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

在下面的示例中，该掩码设置为**R X**。 因此，它**名为用户****所属组**，以及时访问的**已命名的组****将关闭写入权限**检查。

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

为了便于参考，下面是文件或文件夹掩码 Azure 门户中的显示位置。

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

>[AZURE.NOTE] 对于新数据湖存储帐户，ACL 访问权限和默认值 （"/"） 的根文件夹的 ACL 的掩码都被默认为 RWX。

## <a name="permissions-on-new-files-and-folders"></a>新文件和文件夹上的权限

在现有文件夹下创建新文件或文件夹时，在父文件夹上的默认 ACL 决定︰

* 子文件夹的默认 ACL 和访问 ACL
* 子文件的访问权限 ACL （文件不具有默认 ACL）

### <a name="a-child-file-or-folders-access-acl"></a>子文件或文件夹的访问权限 ACL

当创建一个子文件或文件夹时，父元素的默认 ACL 将被复制为子文件或文件夹的访问权限 ACL。 此外，如果**其他**用户拥有 RWX 权限以父元素的默认 ACL，它是完全从子项目的访问权限 ACL。

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

在大多数情况下，上述信息是您应该需要知道如何确定子项目的访问权限 ACL。 但是，如果您熟悉 POSIX 系统并希望了解深入，如何实现这种转换，请参阅本文后面的部分[中创建的新文件和文件夹访问权限 ACL 的 Umask 的角色](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders)。
 

### <a name="a-child-folders-default-acl"></a>子文件夹的默认 ACL

当父文件夹下创建子文件夹时，父文件夹的默认 ACL 复制，因为它是为子文件夹的默认 ACL。

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>了解数据湖存储区中的 Acl 的高级的主题

以下是几个高级的主题可帮助您了解如何确定 Acl 数据湖存储文件或文件夹。

### <a name="umasks-role-in-creating-the-access-acl-for-new-files-and-folders"></a>在创建新文件和文件夹的访问权限 ACL 的 Umask 的角色

在 POSIX 兼容系统中，常规的概念是，umask 是用于转换**所属用户****所属组**，以及**其他**新的子文件或文件夹的访问权限 ACL 权限的父文件夹上的 9 位值。 Umask 的位确定关闭子项目的访问权限 ACL 中的哪些位。 因此它是用于有选择地阻止传播拥有拥有组中的用户权限和其他。
  
在 HDFS 系统中，umask 通常是由管理员来控制站点范围的配置选项。 数据湖商店使用，不能更改**帐户范围 umask** 。 下表显示了数据湖商店的 umask。

| 用户组  | 设置 | 新子项目的访问权限 ACL 的影响 |
|------------ |---------|---------------------------------------|
| 拥有的用户 | ---     | 不起作用                             |
| 所属组| ---     | 不起作用                             |
| 其他       | RWX     | 删除读取 + 写入 + 执行         | 

下面的插图显示此 umask 中操作。 净效果是可移除**其他**用户**读取 + 写入 + 执行**。 由于 umask 未指定以及**所属用户****所属组**的位，将不转换这些权限。

![数据湖存储 Acl](./media/data-lake-store-access-control/data-lake-store-acls-umask.png) 

### <a name="the-sticky-bit"></a>粘滞位

粘滞位是 POSIX 文件系统的更高级的功能。 在湖边存储数据的上下文中，不太可能会需要粘滞位。

下表显示粘滞位数据湖存储区中的工作方式。

| 用户组         | 文件    | 文件夹 |
|--------------------|---------|-------------------------|
| 粘滞位**灭** | 不起作用   | 不起作用           |
| 粘滞位**上**  | 不起作用   | 可以防止任何人除**超级用户**和**所属用户**的删除或重命名该子项目中的子项目。               |

在 Azure 门户中不显示粘滞位。

## <a name="common-questions-for-acls-in-data-lake-store"></a>Acl 数据湖存储区中的常见问题

下面是根据 Acl 数据湖存储区中经常提出的一些问题。

### <a name="do-i-have-to-enable-support-for-acls"></a>是否需要启用对 Acl 的支持？

不。 通过 Acl 的访问控制始终处于数据湖存储帐户。

### <a name="what-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>以递归方式删除的文件夹和其内容需要什么权限？

* 父文件夹必须具有**写入 + 执行**。
* 要删除的文件夹和每个文件夹中，则需要**读取 + 写入 + 执行**。
>[AZURE.NOTE] 删除文件夹中的文件不需要写在这些文件上。 此外，根文件夹"/"**永远不会**被删除。

### <a name="who-is-set-as-the-owner-of-a-file-or-folder"></a>那些被设置为文件或文件夹的所有者？

文件或文件夹的创建者将成为所有者。

### <a name="who-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>谁是在创建时设置为所属组的文件或文件夹？

它将来自在其下创建新的文件或文件夹的父文件夹所属的组。

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>我拥有文件的用户，但是我没有我需要的 RWX 权限。 我该怎么办？

拥有的用户可以只需更改文件的权限，使自己所需的任何 RWX 权限。

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>数据湖存储区是否支持继承的 Acl？

不。

### <a name="what-is-the-difference-between-mask-and-umask"></a>掩码 umask 之间的区别是什么？

| 掩码 | umask|
|------|------|
| **掩码**属性可用于每个文件和文件夹的。 | **Umask**是数据湖存储帐户的属性。 因此，没有仅单个 umask 数据存储中存储湖。    |
| 可以通过将文件或超级用户所属组的所属用户改变 mask 属性上的文件或文件夹。 | Umask 属性不能修改任何用户，甚至超级用户。 它是一个不可更改的固定值。|
| 掩码属性用于过程在运行时访问检查算法来确定用户是否有权执行对文件或文件夹的操作。 掩码的作用是创建"有效权限"时的访问检查。 | Umask 是根本没有使用访问权限检查过程。 Umask 用于确定访问的新子项目文件夹的 ACL。 |
| 掩码是在访问检查时将应用于已命名的用户、 命名的组和所属用户 3 位 RWX 值。| Umask 是适用于拥有的用户、 所属的组中，和其他新子级的 9 位值。| 

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>其中了解有关 POSIX 的访问控制模型的详细信息？

* [http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [HDFS 权限指南](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) 

* [POSIX 常见问题解答](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008年](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1e 1997年](http://users.suse.com/~agruen/acl/posix/Posix_1003.1e-990310.pdf)

* [在 Linux 上的 POSIX ACL](http://users.suse.com/~agruen/acl/linux-acls/online/)

* [在 Linux 上使用访问控制列表的 ACL](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>请参见

* [Azure 湖存储数据的概览](data-lake-store-overview.md)

* [开始使用 Azure 数据湖分析](../data-lake-analytics/data-lake-analytics-get-started-portal.md)





