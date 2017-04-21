
<properties
    pageTitle="与同步使用 Azure AD 连接健康 |Microsoft Azure"
    description="这是将讨论如何监视 Azure AD 连接同步 Azure AD 连接健康页。"
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-for-sync"></a>对于同步使用 Azure AD 连接运行状况
以下文档是特定于监视使用 Azure AD 连接健康的 Azure AD 连接 （同步）。  AD FS 使用 Azure AD 连接运行状况监视的信息请参阅[使用 Azure AD 连接健康 AD FS](active-directory-aadconnect-health-adfs.md)。 此外，有关监视 Azure AD 连接运行状况与活动目录域服务信息请参阅[使用 Azure AD 连接健康 AD DS](active-directory-aadconnect-health-adds.md)。

![Azure AD 同步的连接运行状况](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Azure AD 连接的运行状况同步警报
Azure AD 连接运行状况警报同步部分提供活动通知的列表。 每个通知包含相关的信息、 解决方法步骤和相关文档的链接。 通过选择一个活动或已解决的警报中，您将看到的其他信息，以及您可以采取的步骤来解决警报，以及链接到其他文档的新刀片。 您还可以查看历史数据，在过去已解决的警报。

通过选择的通知将为您提供其他信息，以及步骤即可解决的警报和链接到其他文档。

![Azure AD 连接同步错误](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>警报的有限的计算
Azure AD 连接 （例如，如果筛选属性从默认的配置更改为自定义配置） 没有使用默认的配置，如果 Azure AD 连接健康代理将不会上载到 Azure AD 连接相关的错误事件。

这将限制警报的计算服务。 您将会看到表示这种情况下您的服务的 Azure 门户的横幅。

![Azure AD 同步的连接运行状况](./media/active-directory-aadconnect-health-sync/banner.png)

您可以通过单击"设置"并允许上载所有错误日志 Azure AD 连接健康代理加以更改。

![Azure AD 同步的连接运行状况](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>同步的见解
管理员经常需要了解 Azure AD 同步更改和更改发生的时间。 此功能可以轻松地进行可视化这使用下图︰   

- 滞后时间的同步操作
- 对象变化趋势

### <a name="sync-latency"></a>同步延迟
此功能提供连接器的同步操作 （如导入、 导出等） 的滞后时间的图形化趋势。  这提供了快速简便的方式了解不仅 （如果您有很多的变化发生，这是较大的） 操作的延迟，而且还一种方法来检测异常情况中可能需要进一步研究的滞后时间。

![同步延迟](./media/active-directory-aadconnect-health-sync/synclatency02.png)

默认情况下，显示了只有 Azure AD 连接器的导出操作的延迟时间。  请参阅连接器上的多个操作或查看操作从其他连接器中，用鼠标右键单击图表上，选择编辑图表或者单击"编辑延迟图表"按钮，然后选择特定工序和连接器。

### <a name="sync-object-changes"></a>同步对象的更改
此功能提供了图形化趋势正在更改的数量的计算，并导出到 Azure 的广告。  如今，尝试从同步日志收集此信息会很困难。  图表使您，不仅监视您的环境中发生的更改的次数，但还发生故障的可视视图的更简单的方法。

![同步延迟](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>对象级别同步错误报告 （预览）
此功能提供有关 Windows 服务器 AD 和 Azure AD 使用 Azure AD 连接之间同步标识数据时可能会出现同步错误的报告。

- 报告涵盖的同步客户端记录错误 (Azure AD 连接版本 1.1.281.0 或更高版本)
- 同步引擎上最后一次同步操作过程中，它包括所发生的错误。 （"导出"Azure 的 AD 连接器上。）
- 同步的 azure AD 连接健康代理必须为报表以包含最新的数据所需的结束点与出站连接。 [要求一节](active-directory-aadconnect-health-agent-install.md#Requirements)了解详细信息，请参阅。
- 该报告为**每 30 分钟后更新**使用 Azure AD 连接健康代理同步上载的数据。
它提供了以下关键能力

    - 错误的分类
    - 每个类别的错误的对象的列表
    - 在一个位置的错误有关的所有数据
    - 并排侧比较的对象，因为冲突的错误
    - 下载错误报告为 CVS （即将提供）

### <a name="categorization-of-errors"></a>错误的分类
报告对现有同步错误按以下类别进行分类︰

| 类别 | 说明 |
| -------------- | ----------- |
| 重复属性 | 错误时 Azure AD 连接尝试创建或在 Azure 中的租户，如代理地址，范围内必须是唯一的 AD 中更新对象的一个或多个属性的重复值。 |
| 数据不匹配 | 软匹配不匹配导致同步错误的对象时出现错误。 |
| 数据验证失败 | 由于无效的数据，例如不支持的字符范围内，如关键属性中的错误格式之前写入在 Azure AD 验证失败的错误。|
| 大特性 | 当一个或多个特性都大于允许的大小、 长度或计数错误。|
| 其他 | 所有其他错误，不适合在上述类别中。 根据反馈信息，将子类别中拆分此类别。

![同步错误报告摘要](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![同步错误报表类别](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>每个类别的错误的对象的列表
深入每个类别将提供该类别中具有错误的对象的列表。
![同步错误报表列表](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>错误详细信息
下列数据可用于每个错误的详细资料视图

- 涉及到的*AD 对象*标识符
- （如适用） 所涉及的*Azure AD 对象*标识符
- 错误的说明以及如何修复问题
- 相关的文章

![同步错误报告的详细信息](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-the-error-report-as-csv"></a>为 CSV 下载错误报告
这种能力即将登场。 请继续关注更多的更新。



## <a name="related-links"></a>相关的链接
* [在同步过程中解决错误](active-directory-aadconnect-troubleshoot-sync-errors.md)
* [重复的属性可恢复性](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure AD 连接的运行状况](active-directory-aadconnect-health.md)
* [Azure AD 连接健康代理安装](active-directory-aadconnect-health-agent-install.md)
* [Azure AD 连接健康操作](active-directory-aadconnect-health-operations.md)
* [与 AD FS 使用 Azure AD 连接运行状况](active-directory-aadconnect-health-adfs.md)
* [与 AD DS 使用 Azure AD 连接运行状况](active-directory-aadconnect-health-adds.md)
* [Azure AD 连接健康常见问题解答](active-directory-aadconnect-health-faq.md)
* [Azure AD 连接健康版本历史记录](active-directory-aadconnect-health-version-history.md)
