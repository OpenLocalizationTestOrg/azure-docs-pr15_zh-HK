<properties 
   pageTitle="Azure 自动化数据管理 |Microsoft Azure"
   description="这篇文章包含管理 Azure 自动化环境的多个主题。  目前包括数据保留和备份在 Azure 自动化 Azure 自动化灾难恢复。"
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/02/2016"
   ms.author="bwren;sngun" />

# <a name="managing-azure-automation-data"></a>Azure 自动化数据管理

这篇文章包含管理 Azure 自动化环境的多个主题。

## <a name="data-retention"></a>数据保留

当您删除 Azure 自动化中的资源时，则将其保留为审计目的在被永久删除之前的 90 天。  无法看到或使用该资源，在这段时间。  此政策还适用于属于自动化帐户被删除的资源。

Azure 自动化自动删除并永久删除作业时间超过 90 天。

下表总结了不同资源的保留策略。

|数据|策略|
|:---|:---|
|帐户|永久删除此帐户删除用户后的 90 天。|
|资产|永久删除资产删除一个用户，通过后的 90 天或 90 天后持有资产被删除的用户帐户。|
|模块|永久删除模块删除一个用户，通过后的 90 天或 90 天后保存模块删除用户帐户。|
|运行手册|永久删除资源删除一个用户，通过后的 90 天或 90 天后的帐户包含由用户删除的资源。|
|作业|已删除并永久删除 90 天后最后被修改。 这可能是作业完成、 已停止或暂停后。|
|节点配置/MOF 文件| 旧的节点配置会永久删除生成新的节点配置后的 90 天。|
|DSC 节点| 永久删除的节点是从自动化帐户在 Windows PowerShell 使用 Azure 的门户网站或[注销 AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) cmdlet 注销后的 90 天。 节点还永久删除包含该节点删除的用户帐户后 90 天。 |
|节点的报告| 永久删除该节点生成新的报告后 90 天|

保留策略应用于所有用户，当前不能自定义。

## <a name="backing-up-azure-automation"></a>Azure 自动化备份

删除 Microsoft Azure 中的自动化帐户时，帐户中的所有对象被都删除包括运行手册、 模块、 配置、 设置、 作业和资产。 该帐户被删除后将无法恢复对象。  以下信息可用于在删除之前备份自动化客户的内容。 

### <a name="runbooks"></a>运行手册

您可以用 Windows PowerShell Azure 管理门户或[获取 AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) cmdlet 的脚本文件导出运行手册。  这些脚本文件可以导入到另一个自动化的帐户，如下所述[创建或导入 Runbook](https://msdn.microsoft.com/library/dn643637.aspx)。


### <a name="integration-modules"></a>集成模块

不能导出从 Azure 自动化的集成模块。  必须确保有可用自动化帐户之外。

### <a name="assets"></a>资产

您不能导出从 Azure 自动化的[资产](https://msdn.microsoft.com/library/dn939988.aspx)。  使用 Azure 管理门户，您必须注意变量、 凭据、 证书、 连接和时间表的详细信息。  然后，您必须手动创建使用导入另一个自动化的运行手册的任何资产。

可以使用[Azure cmdlet](https://msdn.microsoft.com/library/dn690262.aspx)来检索详细信息的未加密的资产，然后将它们另存供将来参考或其他自动化帐户中创建等效的资产。

无法检索加密的变量或凭据使用 cmdlet 的密码字段的值。  如果您不知道这些值，然后可从它们使用[Get AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx)和[Get AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx)活动 runbook。

您不能从 Azure 自动化导出证书。  您必须确保任何证书可用在 Azure 之外。

### <a name="dsc-configurations"></a>DSC 配置

在 Windows PowerShell 使用 Azure 管理门户或[导出 AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) cmdlet 的脚本文件，您可以导出您的配置。 这些配置可以导入和使用自动化的另一个帐户中。


##<a name="geo-replication-in-azure-automation"></a>在 Azure 自动化的 Geo 复制

各地区复制，在 Azure 自动化客户的标准备份到不同的地区，以实现冗余帐户数据。 设置您的帐户时，您可以选择的主要区域，然后一个第二区域分配给它自动。 在数据丢失的情况下，辅助数据，复制从主要区域中，将不断更新。  

下表显示了可用的主要和次要地区配对。

|主要            |第二
| ---------------   |----------------
|美国中南部   |美国中北部
|美国东部 2          |美国中部
|西欧        |北欧
|东南亚    |东亚
|日本东         |日本西部

万一主区域数据将丢失，Microsoft 尝试对其进行恢复。 如果不能恢复的主数据，然后执行地理故障切换，将通过其订阅关于这通知受影响的客户。

