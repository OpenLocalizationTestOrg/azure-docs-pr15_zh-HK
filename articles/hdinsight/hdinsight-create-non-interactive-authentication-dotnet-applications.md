<properties
    pageTitle="创建非交互式身份验证.NET HDInsight applciations |Microsoft Azure"
    description="了解如何创建非交互式身份验证.NET HDInsight 应用程序。"
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>创建.NET HDInsight 应用程序的非交互式身份验证

您可以执行.NET Azure HDInsight 应用程序在应用程序自身的身份 （非交互式） 或下 （交互） 的应用程序中已登录的用户的标识。 交互式应用程序的示例，请参阅[使用 HDInsight.NET SDK 提交配置单元/猪/Sqoop 作业](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk)。 本文演示如何创建非交互式身份验证.NET 应用程序连接到 Azure HDInsight 并提交配置单元作业。

从.NET 应用程序中，您将需要︰

- 您的订阅 Azure 租户 ID
- Azure 目录应用程序客户端 ID
- Azure 目录应用程序的机密密钥。  

主要的过程包括以下步骤︰

2. 创建目录 Azure 应用程序。
2. 将角色分配给 AD 应用程序。
3. 开发客户端应用程序。


##<a name="prerequisites"></a>系统必备组件

- HDInsight 群集。 您可以创建一个使用[入门教程](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster)中的说明。 




## <a name="create-azure-directory-application"></a>创建目录 Azure 应用程序 
创建活动目录应用程序时，实际创建的应用程序和服务主体。 您可以执行应用程序的标识的应用程序。

目前，必须使用 Azure 的经典门户创建一个新的活动目录应用程序。 此功能将被添加到 Azure 的门户，在以后的版本中。 您还可以执行这些步骤到 Azure PowerShell 或 Azure CLI。 有关使用 PowerShell 或 CLI 与主体服务的详细信息，请参阅[身份验证服务主体与 Azure 资源管理器中](../resource-group-authenticate-service-principal.md)。

**创建目录 Azure 应用程序**

1.  登录到[Azure 的传统门户网站]( https://manage.windowsazure.com/)。
2.  从左窗格中选择**活动目录**。

    ![Azure 经典门户活动目录](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\active-directory.png)
    
3.  选择要用于创建新的应用程序的目录。 它应是现有联系人。
4.  从顶部列出现有应用程序中单击**应用程序**。
5.  从底部添加新的应用程序，请单击**添加**。
6.  输入**名称**，选择**Web 应用程序和/或 Web API**，，然后单击**下一步**。

    ![新 azure 的活动目录应用程序](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application.png)

7.  输入**登录 URL**和**ID 的应用程序的 URI**。 对于**符号上的 URL**，提供网站描述您的应用程序的 URI。 未验证的 web 站点存在。 对于应用程序 ID URI，可提供标识您的应用程序的 URI。 然后单击**完成**。
花费一些时间来创建应用程序。  创建应用程序后，门户将显示新的应用程序的快速 Glace 页。 不要关闭门户。 

    ![新 azure 的活动目录应用程序属性](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application-properties.png)

**若要获取应用程序客户端 ID 和密钥**

1.  从新创建 AD 应用程序页中，从顶部的菜单中单击**配置**。
2.  制作副本的**客户端 ID**。 您将需要它在.NET 应用程序。
3.  **键**下, 单击**选择持续时间**的下拉列表中，选择**1 年**或**2 年**。 保存配置之前，将不显示项的值。
4.  单击**保存**在页的底部。 出现的机密密钥时，将密钥的副本。 您将需要它在.NET 应用程序。

##<a name="assign-ad-application-to-role"></a>分配给角色的广告应用程序

您必须分配给[角色](../active-directory/role-based-access-built-in-roles.md)的应用程序，授权其执行操作的权限。 您可以在订购、 资源组或资源级别设置范围。 权限继承到较低级别的作用域 （例如，添加到资源组的读者角色的应用程序意味着它可以读取资源组，它所包含的任何资源）。 在本教程中，您将资源组级别设置范围。  由于 Azure 的传统门户网站不支持资源组，这一部分必须执行从 Azure 的门户。 

**若要将所有者角色添加到 AD 应用程序**

1.  登录到[Azure 的门户](https://portal.azure.com)。
2.  从左窗格中，单击**资源组**。
3.  单击包含您将在本教程后面运行配置单元查询 HDInsight 群集资源组。 如果有太多的资源组，您可以使用筛选器。
4.  从群集刀片式服务器，请单击**访问**。

    ![云和 thunderbolt 图标 = 快速入门](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)
5.  从**用户**刀片式服务器，请单击**添加**。
6.  请按照指令将**所有者**角色添加到 AD 应用最后一个过程中创建。 当成功完成时，您应看到列出具有所有者角色的用户刀片式服务器中的应用程序。


##<a name="develop-hdinsight-client-application"></a>开发 HDInsight 客户端应用程序

创建 C#.net 控制台应用程序后[提交 Hadoop 作业在 HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk)中的说明。 然后使用以下标记替换 GetTokenCloudCredentials 方法︰

    public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
    {
        var authFactory = new AuthenticationFactory();

        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };

        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

        var accessToken =
            authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never)
                .AccessToken;

        return new TokenCloudCredentials(accessToken);
    }

若要检索通过 PowerShell 的租户 ID:

    Get-AzureRmSubscription

或者，Azure CLI:

    azure account show --json

      
## <a name="see-also"></a>请参见

- [将 HDInsight 中的 Hadoop 作业提交](hdinsight-submit-hadoop-jobs-programmatically.md)
- [创建活动目录应用程序和服务主体使用门户](../resource-group-create-service-principal-portal.md)
- [身份验证服务主体与 Azure 资源管理器](../resource-group-authenticate-service-principal.md)
- [Azure 的基于角色的访问控制](../active-directory/role-based-access-control-configure.md)
