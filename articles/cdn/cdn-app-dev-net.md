<properties
    pageTitle="学习如何使用 Azure CDN 库.net |Microsoft Azure"
    description="学习如何编写.NET 应用程序来管理 Azure 使用 Visual Studio 的 CDN。"
    services="cdn"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Azure CDN 开发入门

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

您可以使用[Azure CDN.NET 库](https://msdn.microsoft.com/library/mt657769.aspx)自动化的 CDN 的配置文件和终结点创建和管理。  本教程将带领通过创建一个简单的.NET 控制台应用程序演示几个可用的操作。  本教程不是详细说明针对.NET 的 Azure CDN 库的所有方面。

Visual Studio 2015 完成本教程，您需要。  [Visual Studio 社区 2015年](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)是免费下载。

> [AZURE.TIP] [完成本教程中的项目，](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c)则可以在 MSDN 上的下载。

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>创建项目和添加 Nuget 程序包

现在，我们已经为我们 CDN 配置文件创建的资源组，并给我们 Azure AD 的应用程序权限管理 CDN 配置文件和组中的终结点，我们可以开始创建我们的应用程序。

从内 Visual Studio 2015，单击**文件**，**新建**以打开新建项目对话框**项目...** 。  展开**C#**，然后在左窗格中选择**窗口**。  在中心窗格中，单击**控制台应用程序**。  命名您的项目，然后单击**确定**。  

![新建项目](./media/cdn-app-dev-net/cdn-new-project.png)

我们的项目要使用 Nuget 程序包中包含某些 Azure 库。  让我们添加到项目中的那些。

1. 单击**工具**菜单， **Nuget 程序包管理器**，然后**程序包管理器控制台**。

    ![管理 Nuget 程序包](./media/cdn-app-dev-net/cdn-manage-nuget.png)

2. 在程序包管理器控制台上，执行以下命令以安装**活动目录身份验证库 (ADAL)**:

    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`

3. 执行以下操作以安装**Azure CDN 管理库**︰

    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>指令、 常数、 主要方法和帮助器方法

让我们得到我们编写程序的基本结构。

1. 返回在 Program.cs 选项卡，将替换为`using`顶部用以下指令︰

    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

2. 我们需要定义一些常量，将使用我们的方法。  在`Program`类，之前`Main`方法中，添加以下。  一定要替换占位符，包括**&lt;尖括号&gt;**，使用您自己的值根据需要。

    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";

    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. 此外在类级别，定义这两个变量。  稍后将使用它们来确定是否我们的配置文件和端点已存在。

    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```

4.  更换`Main`方法，如下所示︰

    ```csharp
    static void Main(string[] args)
    {
        //Get a token
        AuthenticationResult authResult = GetAccessToken();

        // Create CDN client
        CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
            { SubscriptionId = subscriptionId };

        ListProfilesAndEndpoints(cdn);

        // Create CDN Profile
        CreateCdnProfile(cdn);

        // Create CDN Endpoint
        CreateCdnEndpoint(cdn);
        
        Console.WriteLine();

        // Purge CDN Endpoint
        PromptPurgeCdnEndpoint(cdn);

        // Delete CDN Endpoint
        PromptDeleteCdnEndpoint(cdn);

        // Delete CDN Profile
        PromptDeleteCdnProfile(cdn);

        Console.WriteLine("Press Enter to end program.");
        Console.ReadLine();
    }
    ```

5. 一些我们其他的方法将提示用户用"是/否"的问题。  添加下面的方法来使它变得稍微容易︰

    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

既然我们程序的基本结构编写的我们应该创建调用的方法`Main`方法。

## <a name="authentication"></a>身份验证

我们可以使用 Azure CDN 管理库之前，我们需要对我们服务的主体进行身份验证并获取身份验证令牌。  此方法使用 ADAL 来检索令牌。

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

如果您使用的单个用户验证`GetAccessToken`方法的外观略有不同。

>[AZURE.IMPORTANT] 如果您选择了单个用户身份验证，而不是服务主体，仅使用此代码示例。

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

一定要替换`<redirect URI>`与重定向 URI Azure AD 中注册应用程序时输入的。

## <a name="list-cdn-profiles-and-endpoints"></a>列表 CDN 配置文件和终结点

现在，我们就可以执行 CDN 运营了。  我们方法的第一件事是所有配置文件和我们的资源组中的终结点的列表，如果找到适合我们的常数中指定的配置文件和终结点名称使记下的，稍后以便我们不试图创建重复项。

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN 的配置文件和终结点创建

接下来，我们将创建一个配置文件。

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

创建配置文件后，我们将创建一个终结点。

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

>[AZURE.NOTE] 上面的示例中将该终结点分配来源主机名为*Contoso* `www.contoso.com`。  您应更改此设置以指向您自己的来源主机名。

## <a name="purge-an-endpoint"></a>清除某个终结点

假定已创建终结点，我们可能想要执行我们的计划中的一个常见任务清除我们终结点中的内容。

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

>[AZURE.NOTE] 在上例中，字符串`/*`表示我想要清除一切终结点路径的根目录中的。  这相当于在 Azure 门户的"清除"对话框中检查**所有清除**。 在`CreateCdnProfile`方法中，我创建了我们的配置文件使用代码**从 Verizon 的 Azure CDN**配置文件为`Sku = new Sku(SkuName.StandardVerizon)`，因此这将会成功。  但是， **Akamai 从 Azure CDN**配置文件不支持**全部清除**，所以如果我使用的一个 Akamai 的配置文件，对于本教程，我将需要包括清除的特定路径。

## <a name="delete-cdn-profiles-and-endpoints"></a>删除 CDN 配置文件和终结点

我们的终结点和配置文件，则将删除最后一个方法。

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>运行程序

我们现在可以编译并运行该程序，单击在 Visual Studio 中的**开始**按钮。

![运行程序](./media/cdn-app-dev-net/cdn-program-running-1.png)

当程序到达上面的提示时，您应该能够返回到您在 Azure 门户的资源组和查看已创建的配置文件。

![成功 ！](./media/cdn-app-dev-net/cdn-success.png)

我们就可以确定要运行的程序的其余部分的提示。

![程序完成](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>下一步行动

若要查看此演练中，[下载示例](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c)从已完成的项目。

若要查找在 Azure CDN 管理库.net 的其他文档，查看[MSDN 上的参考](https://msdn.microsoft.com/library/mt657769.aspx)。

管理与[PowerShell](./cdn-manage-powershell.md)CDN 资源。


