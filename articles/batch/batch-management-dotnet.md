<properties
    pageTitle="帐户使用批次管理.NET 资源管理 |Microsoft Azure"
    description="创建、 删除和修改批次管理.NET 库 Azure 批客户资源。"
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/19/2016"
    ms.author="marsma"/>

# <a name="manage-azure-batch-accounts-and-quotas-with-batch-management-net"></a>管理 Azure 批帐户和批次管理.NET 的配额

> [AZURE.SELECTOR]
- [Azure 门户](batch-account-create-portal.md)
- [批次管理.NET](batch-management-dotnet.md)

您可以降低维护开销 Azure 批处理应用程序中使用[批处理管理.NET] [api_mgmt_net]库自动批帐户创建、 删除、 密钥管理和配额发现。

- **创建和删除批处理帐户**在任何区域内。 如果作为一个独立软件供应商 (ISV) 例如，您为客户提供服务您在其中每个分配一个单独的批处理帐户用于记帐的目的，您可以向客户门户添加帐户的创建和删除功能。
- **检索并重新生成帐户密钥**以编程方式为任何批次帐户。 这可以帮助您遵守强制定期翻转或过期的帐户密钥的安全策略。 当各种 Azure 的地区有几批客户时，这一翻转过程的自动化提高解决方案的效率。
- **检查帐户配额**并拍摄试验错误正确判断哪些批帐户有什么限制。 通过在开始作业之前检查帐户配额，创建池，或添加计算节点，您可以主动地调整位置或这些计算会创建资源。 您可以确定哪些帐户需要分配额外的资源，在这些客户中之前增加配额。
- 为用户提供全功能的管理体验 — 使用批次管理.NET， [Azure Active Directory]**将其他 Azure 服务功能组合**[aad_about]，和[Azure 资源管理器][resman_overview]在同一应用程序中组合在一起。 通过使用这些功能和其 Api，您可以提供顺畅的身份验证经验，创建和删除资源组和上述的端到端管理解决方案的功能的能力。

> [AZURE.NOTE] 虽然这篇文章重点介绍编程批帐户、 键和配额的管理，您可以执行许多这些活动使用[Azure 的门户网站][azure_portal]。 有关详细信息，请参阅[创建批处理 Azure 帐户使用 Azure 的门户](batch-account-create-portal.md)和[配额和 Azure 批服务的限制](batch-quota-limit.md)。

## <a name="create-and-delete-batch-accounts"></a>创建和删除批处理帐户

如上所述，批次管理 API 的主要功能之一是创建和删除批处理帐户在 Azure 的地区。 若要执行此操作，请使用[BatchManagementClient.Account.CreateAsync] [ net_create] [DeleteAsync]，[net_delete]，或与其同步。

下面的代码段创建一个帐户、 批次服务中，从获取新创建的帐户，然后删除它。 在此代码段和其他人在此文章中，`batchManagementClient`是完全初始化的实例的[BatchManagementClient][net_mgmt_client]。

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [AZURE.NOTE] 使用批次管理.NET 库和其 BatchManagementClient 类的应用程序需要拥有批帐户管理订阅**服务管理员**或**coadministrator**访问。 有关详细信息，请参阅[Azure Active Directory](#azure-active-directory)部分和[AccountManagement] [acct_mgmt_sample]代码示例。

## <a name="retrieve-and-regenerate-account-keys"></a>检索并重新生成帐户密钥

从您的订阅中的任何批次帐户获得主要和辅助帐户密钥，使用[ListKeysAsync][net_list_keys]。 您可以使用[RegenerateKeyAsync]重新生成这些密钥[net_regenerate_keys]。

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [AZURE.TIP] 可以优化的连接工作流管理应用程序。 首先，获取您想要使用[ListKeysAsync]管理批次帐户帐户密钥[net_list_keys]。 然后，使用此密钥时初始化批.NET 库的[BatchSharedKeyCredentials] [net_sharedkeycred]类，它用来在初始化[BatchClient][net_batch_client]。

## <a name="check-azure-subscription-and-batch-account-quotas"></a>检查 Azure 订阅和批处理帐户配额

Azure 订阅和个别的 Azure 服务像批处理所有具有默认配额限制的其中某些实体数。 Azure 订阅的默认配额，请参阅[Azure 订阅和服务限制、 配额和限制](./../azure-subscription-service-limits.md)。 批处理服务的默认配额，请参阅[配额和 Azure 批服务的限制](batch-quota-limit.md)。 通过使用批处理管理.NET 库，您可以在您的应用程序中检查这些配额。 这使您可以添加帐户或计算资源，如池和计算节点之前作出分配决策。

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>检查批处理帐户配额 Azure 订阅

才能在区域创建一个批次的帐户，您可以检查 Azure 订阅，查看您是否能够在该区域中添加一个帐户。

下面的代码段中，在我们第一次使用[BatchManagementClient.Account.ListAsync] [net_mgmt_listaccounts]以获取订阅中的所有批次帐户的集合。 一旦我们已经获得此集合，我们确定多少个帐户在目标区域中。 然后我们使用[BatchManagementClient.Subscriptions] [net_mgmt_subscriptions]以获取批帐户配额，并确定在该区域中可以创建多少个帐户 （如果有的话）。

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

在段中，`creds`是[TokenCloudCredentials][azure_tokencreds]。 创建此对象的示例，请参见[AccountManagement] [acct_mgmt_sample]在 GitHub 代码示例。

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>检查计算资源配额批帐户

之前增加批次解决方案中的计算资源，您可以检查以确保您希望分配的资源不会超过该帐户的配额。 我们在下面的代码段，打印名为批处理帐户的配额信息`mybatchaccount`。 在您自己的应用程序，可以使用此类信息来确定客户是否可以处理更多的资源，来创建。

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [AZURE.IMPORTANT] 虽然有 Azure 订阅和服务上的默认配额，许多这些限制可以引发发出请求在[Azure 的门户网站][azure_portal]。 例如，看到如何增加批帐户配额[配额和 Azure 批服务的限制](batch-quota-limit.md)。

## <a name="batch-management-net-azure-ad-and-resource-manager"></a>批次管理.NET，Azure 的广告，和资源管理器

在处理与批次管理.NET 库时，您通常还使用[Azure Active Directory] [ aad_about] (Azure AD) 和[Azure 资源管理器][resman_overview]。 下面讨论的示例项目使用 Azure Active Directory 和资源管理器，而它演示批处理管理.NET API。

### <a name="azure-active-directory"></a>Azure 的活动目录

Azure 本身对其客户、 服务管理员和组织用户的身份验证使用 Azure 的广告。 在批次管理.NET 上下文中，使用 Azure 广告预订管理员或共同的管理员进行身份验证。 这使管理库查询批处理服务并执行本文中描述的操作。

示例项目中讨论在 Azure 的[活动目录身份验证库]下列[aad_adal](ADAL) 被用来提示用户输入其 Microsoft 凭据。 当提供服务管理员或 coadministrator 凭据时，应用程序可以查询预订-列表 Azure，并可以创建和删除资源组和批次帐户。

### <a name="resource-manager"></a>资源管理器

与批次管理.NET 库创建批处理帐户时，通常要创建这些[资源组]内[resman_overview]。 您可以以编程方式创建资源组使用[ResourceManagementClient] [resman_client]在[资源管理器.NET]类[resman_api]库。 可以将帐户添加到现有资源组创建以前通过使用[Azure 的门户网站][azure_portal]。

## <a name="sample-project-on-github"></a>在 GitHub 上的样例项目

若要在操作中查看批次管理.NET，签出[AccountManagment] [acct_mgmt_sample]在 GitHub 上的样例项目。 该控制台应用程序演示了创建和使用的[BatchManagementClient] [net_mgmt_client]和[ResourceManagementClient][resman_client]。 它还演示如何使用[活动目录身份验证库]的 Azure 的[aad_adal](ADAL)，这所需的两个客户端。

若要成功运行此示例应用程序，您必须首先注册它使用 Azure 广告使用 Azure 的门户。 按照[Azure Active Directory 集成的应用程序]中的[添加应用程序](../active-directory/active-directory-integrating-applications.md#adding-an-application)部分中的步骤[aad_integrate]若要注册您自己的帐户中的示例应用程序的默认目录。 请务必选择**本机客户端应用程序**类型的应用程序，并且您可以指定任何有效的 URI (如`http://myaccountmanagementsample`) 的**重定向 URI**，它不需要真正的终结点。

添加您的应用程序之后, 委托到门户中的应用程序的设置的*Windows Azure 服务管理 API*应用程序**访问 Azure 服务管理作为组织**的权限︰

![在 Azure 的门户应用程序权限][2]

> [AZURE.TIP] 如果**Windows Azure 服务管理 API**没有出现在*其他应用程序的权限*，请单击**添加应用程序**，选择**Windows Azure 服务管理 API**，然后单击复选标记按钮。 然后，将委托与上面所指定的权限。

一旦您已经添加应用程序，如上面所述，更新`Program.cs`在[AccountManagment] [acct_mgmt_sample]示例项目使用您的应用程序重定向 URI 和客户机 id。 您可以在您的应用程序的**配置**选项卡中找到这些值︰

![在 Azure 的门户应用程序配置][3]

[AccountManagment] [acct_mgmt_sample]示例应用程序演示下列操作︰

1. 获取一个安全令牌从 Azure 广告使用[ADAL][aad_adal]。 如果没有已登录的用户，将提示他们输入 Azure 凭据。
2. 通过使用从 Azure 的广告获得的安全令牌，创建[SubscriptionClient] [ resman_subclient] Azure 查询与该帐户关联的订阅的列表。 这样，如果多个订阅的所选内容位于。
3. 创建与所选的订阅关联的凭据对象。
4. 创建[ResourceManagementClient] [resman_client]使用的凭据。
5. 使用[ResourceManagementClient] [resman_client]创建的资源组。
6. 使用[BatchManagementClient] [net_mgmt_client]执行多个批处理帐户操作︰
  - 新的资源组中创建一个批次的帐户。
  - 从批服务获取新创建的帐户。
  - 打印新帐户的帐户密钥。
  - 重新生成该帐户的新主键。
  - 打印帐户的配额信息。
  - 打印该订阅的配额信息。
  - 打印在预订中的所有帐户。
  - 删除新创建的帐户。
7. 删除资源组。

删除新创建的批处理帐户和资源组之前, 可以检查在[Azure 门户][azure_portal]:

![Azure 门户显示的资源组和批帐户][1]
<br />
*显示新的资源组和批帐户的 azure 门户*

[aad_about]: ../active-directory/active-directory-whatis.md "什么是 Azure 活动目录？"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Azure AD 身份验证方案"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "与 Azure Active Directory 集成应用程序"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspxs
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
