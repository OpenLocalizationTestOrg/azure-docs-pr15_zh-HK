<properties
    pageTitle="连接到 Azure 使用 CLI 堆栈 |Microsoft Azure"
    description="了解如何使用跨平台命令行界面 (CLI) 来管理和部署 Azure 堆栈上的资源"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="helaw"/>

# <a name="install-and-configure-azure-stack-cli"></a>安装和配置 Azure 堆栈 CLI

在本文中，我们引导您使用 Azure 的命令行界面 (CLI) 来管理 Azure 堆栈在 Linux 和 Mac 客户端平台上的资源的过程。  

## <a name="install-azure-stack-cli"></a>安装 CLI 的 Azure 堆栈

如果你在 Mac 或 Linux 上，可以通过使用下面的命令来获取 CLI:
  
    `npm install -g azure-cli@0.10.4`.


## <a name="connect-to-azure-stack"></a>连接到 Azure 堆栈
在以下步骤中，您可以配置 Azure CLI 连接到 Azure 堆栈。 然后在每次登录，并检索订购信息。

1.  通过执行此 PowerShell 检索活动目录资源 id 的值︰
        
          (Invoke-RestMethod -Uri https://api.azurestack.local/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]

2.  使用以下 CLI 命令添加 Azure 堆栈环境中，并确保更新*-活动目录资源 id*数据与前面步骤中检索 URL:

           azure account env add AzureStack --resource-manager-endpoint-url "https://api.azurestack.local" --management-endpoint-url "https://api.azurestack.local" --active-directory-endpoint-url  "https://login.windows.net" --portal-url "https://portal.azurestack.local" --gallery-endpoint-url "https://portal.azurestack.local" --active-directory-resource-id "https://azurestack.local-api/" --active-directory-graph-resource-id "https://graph.windows.net/"

3.  通过使用下面的命令 （替换*的用户名*与您的用户名） 登录︰

        azure login -e AzureStack -u “<username>”

    >[AZURE.NOTE]如果您收到了证书验证问题，请通过运行命令禁用证书验证`set        NODE_TLS_REJECT_UNAUTHORIZED=0`。

4.  通过使用以下命令设置 Azure 的配置模式到 Azure 资源管理器中︰

        azure config mode arm

5.  检索订阅列表。

        azure account list     

## <a name="next-steps"></a>下一步行动

[部署使用 Azure CLI 的模板](azure-stack-deploy-template-command-line.md)

[使用 PowerShell 连接](azure-stack-connect-powershell.md)

[管理用户权限](azure-stack-manage-permissions.md)
