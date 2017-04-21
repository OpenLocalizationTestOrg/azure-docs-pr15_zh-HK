<properties
   pageTitle="密钥存储库开发人员指南 》 |Microsoft Azure"
   description="开发人员可以使用 Azure 密钥存储库来管理 Microsoft Azure 环境内的加密密钥。 "
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/03/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-developers-guide"></a>Azure 的密钥存储库开发人员指南 》
使用密钥存储库，您将能够安全地访问您的应用程序中的敏感信息，例如︰

- 密钥和密码将受到保护而无需自己编写代码，您将能够轻松地从您的应用程序中使用它们。
- 您可以有您自己的客户和管理其自己的密钥，因此您可以专注于提供软件功能的核心。 以这种方式您的应用程序不属于责任或潜在客户的租户键和秘密的责任。
- 您的应用程序可以使用密钥进行签名并加密尚未使密钥的管理从应用程序外部这样的解决方案是适用于地理位置分散的应用程序。

- 随着 9 月 2016年的密钥存储库，您的应用程序现在可以进行密钥存储库的使用证书。 有关详细信息，请参阅**有关密钥、 秘密和证书**文章[其余部分的引用](https://msdn.microsoft.com/library/azure/dn903623.aspx)中。

在 Azure 密钥存储库的详细信息，请参阅[什么是密钥存储库](key-vault-whatis.md)。

## <a name="videos"></a>视频
该视频演示了如何创建您自己的密钥存储库以及如何从 Hello 密钥存储库示例应用程序中使用它。

[AZURE.VIDEO azure-key-vault-developer-quick-start]

在视频中提到的资源的链接︰
- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Azure 的密钥存储库的示例代码](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

若要了解更可以按照[密钥存储库的博客](http://aka.ms/kvblog)并参与[密钥存储库论坛](http://aka.ms/kvforum)。

## <a name="creating-and-managing-key-vaults"></a>创建和管理密钥存储库

之前在您的代码中使用 Azure 密钥存储库，可以创建和管理存储库通过其余部分、 资源管理器模板，PowerShell 或 CLI，以下文章中所述︰

- [创建和管理密钥存储库与其余部分](https://msdn.microsoft.com/library/azure/mt620024.aspx)
- [创建和管理密钥存储库使用 PowerShell](key-vault-get-started.md)
- [创建和管理密钥存储库使用 CLI](key-vault-manage-with-cli.md)
- [创建密钥存储库，然后添加一个秘密通过 Azure 资源管理器模板](../resource-manager-template-keyvault.md)

>[AZURE.NOTE] 对密钥存储库操作都通过 AAD 身份验证和授权通过每个存储库定义的密钥存储库的自己访问策略。

## <a name="coding-with-key-vault"></a>密钥存储库的编码

密钥存储库管理系统的程序员的几个接口，与其余部分为基础，包括[密钥存储库 REST API 参考](https://msdn.microsoft.com/library/azure/dn903609.aspx)。

您可以经过成功的授权，执行以下操作︰

- 管理加密密钥，使用[创建](https://msdn.microsoft.com/library/azure/dn903634.aspx)、[导入](https://msdn.microsoft.com/library/azure/dn903626.aspx)、[更新](https://msdn.microsoft.com/library/azure/dn903616.aspx)、[删除](https://msdn.microsoft.com/library/azure/dn903611.aspx)和其他操作

- 管理机密信息[获取](https://msdn.microsoft.com/library/azure/dn903633.aspx)、[更新](https://msdn.microsoft.com/library/azure/dn986818.aspx)、[删除](https://msdn.microsoft.com/library/azure/dn903613.aspx)和其他操作使用

- 加密密钥使用[符号](https://msdn.microsoft.com/library/azure/dn878096.aspx)/[验证](https://msdn.microsoft.com/library/azure/dn878082.aspx)、 [WrapKey](https://msdn.microsoft.com/library/azure/dn878066.aspx)/[UnwrapKey](https://msdn.microsoft.com/library/azure/dn878079.aspx)和[加密](https://msdn.microsoft.com/library/azure/dn878060.aspx)/[解密](https://msdn.microsoft.com/library/azure/dn878097.aspx)操作

下面的 Sdk 是可供使用的密钥存储库︰

|[![.NET](./media/key-vault-developers-guide/msft.netlogo_purple.png)](https://msdn.microsoft.com/library/mt765854.aspx)|[![Node.js](./media/key-vault-developers-guide/nodejs.png)](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)
|:--:|:--:|
|[.NET SDK 文档](https://msdn.microsoft.com/library/mt765854.aspx)|[Node.js SDK 文档](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)|
|[Nuget 在.NET SDK 包](http://www.nuget.org/packages/Microsoft.Azure.KeyVault)|[Node.js SDK 包](https://www.npmjs.com/package/azure-keyvault)|

有关.NET SDK 的 2.x 版本的详细信息，请参阅[发行说明](key-vault-dotnet2api-release-notes.md)。

## <a name="example-code"></a>代码示例
与您的应用程序使用密钥存储库的完整示例，请参阅︰

- .NET 示例应用程序*HelloKeyVault*和 Azure web 服务示例。 [Azure 的密钥存储库代码示例](http://www.microsoft.com/download/details.aspx?id=45343)
- 为了帮助您了解如何使用 Azure 密钥存储库从 Azure 中的 web 应用程序的教程。 [使用 Web 应用程序从 Azure 密钥存储库](key-vault-use-from-web-application.md)

## <a name="how-tos"></a>操作方法

下面的项目和方案提供了使用 Azure 密钥存储库的任务具体指导︰

- [更改密钥存储库租户 ID 后订阅移动](key-vault-subscription-move-fix.md)-将 Azure 订购从租户 A 移到 B 租户时，现有密钥存储库是无法访问的租户 B.修复此使用本指南中的主体 （用户和应用程序）。
- [防火墙后访问密钥存储库](key-vault-access-behind-firewall.md)的访问密钥存储库客户端应用程序必须能够访问各种功能的多个终结点的密钥存储库。

- [如何生成和 Azure 密钥存储库的 Transfer HSM-Protected 键](key-vault-hsm-protected-keys.md)-这将帮助您规划、 生成和再传输自己 HSM 保护键使用 Azure 密钥存储库。
- [如何来传递 （如密码） 的安全值，在部署期间](../resource-manager-keyvault-parameter.md)-当您需要在部署过程中作为参数进行传递 （如密码） 的安全值时，可以将该值存储为机密在 Azure 密钥存储库和资源管理器中的其他模板中的值的引用。
- [如何使用与 SQL Server 的可扩展密钥管理密钥存储库](https://msdn.microsoft.com/library/dn198405.aspx)的 SQL Server Azure 密钥存储库连接器允许 SQL Server 和 SQL 中 VM 为可扩展密钥管理 (EKM) 提供商来保护其加密密钥的应用程序链接; 利用 Azure 密钥存储库服务透明数据加密、 备份加密和列级加密。
- [如何部署证书的密钥存储库中的虚拟机](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/)-Azure 上运行的虚拟机中的云应用程序需要一个证书。 如何获得此证书到此 VM 今天？
- [如何设置密钥存储库端到端密钥轮替和审核](key-vault-key-rotation-log-monitoring.md)-此演练如何设置密钥轮替和审核与 Azure 密钥存储库。

集成和使用 Azure 的密钥存储库的多个特定于任务的指导，请参阅[Ryan Jones Azure 资源管理器的密钥存储库的模板示例](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)。

## <a name="integrated-with-key-vault"></a>与密钥存储库集成

这些文章是关于方案和使我们的或与密钥存储库集成的服务。

- [Azure 磁盘加密](../security/azure-security-disk-encryption.md)利用行业标准[BitLocker](https://technet.microsoft.com/library/cc732774.aspx)功能的 Windows 和 linux 操作系统提供的操作系统和数据磁盘卷加密的[DM Crypt](https://en.wikipedia.org/wiki/Dm-crypt)功能。 与 Azure 密钥存储库，以帮助您控制和管理的磁盘加密密钥和密钥存储库订购，同时确保在 Azure 存储中存放时，虚拟机磁盘中的所有数据均已都加密的机密信息集成解决方案。


## <a name="supporting-libraries"></a>支持库

- [Microsoft Azure 密钥存储库核心库](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core)提供了`IKey`，`IKeyResolver`定位键标识符以及使用密钥执行操作的接口。

- [Microsoft Azure 密钥存储库扩展](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions)的 Azure 密钥存储库提供扩展的功能。

## <a name="other-key-vault-resources"></a>密钥存储库中的其他资源
- [密钥存储库日志](http://aka.ms/kvblog)
- [密钥存储库论坛](http://aka.ms/kvforum)
