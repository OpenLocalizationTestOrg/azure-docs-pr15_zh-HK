<properties
   pageTitle="管理 Azure Analysis Services |Microsoft Azure"
   description="了解如何管理在 Azure Analysis Services 服务器。"
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="manage-analysis-services"></a>管理 Analysis Services

在 Azure 创建一个 Analysis Services 服务器后，可能有一些需要立即或今后执行下一步的管理任务。 例如，运行过程与刷新数据，可以控制可以访问您的服务器上的模型或监视服务器的健康状态。 只能在 Azure 门户中，其他人在 SQL Server 管理 Studio (SSMS)，执行一些管理任务，可以在完成某些任务。

## <a name="azure-portal"></a>Azure 门户
[Azure 门户](http://portal.azure.com/)是其中您可以创建和删除服务器、 监视服务器资源、 更改大小，以及管理谁有权访问您的服务器。  如果遇到某些问题，您还可以提交支持请求。

![在 Azure 中获取服务器名称](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server 管理 Studio
连接到您的服务器在 Azure 中就如同连接到您自己的组织中的服务器实例。 从 SSMS，可以执行许多相同的任务，例如处理数据或创建处理脚本，管理角色，并使用 PowerShell。

![SQL Server 管理 Studio](./media/analysis-services-manage/aas-manage-ssms.png)

 更大的区别之一就是您用来连接到您的服务器的身份验证。 若要连接到 Azure Analysis Services 服务器，您需要选择**活动目录的密码身份验证**。

### <a name="to-connect-with-ssms"></a>与 SSMS 进行连接
1. 在连接之前，您需要获取服务器名称。 在**Azure 门户**> 服务器 >**概述** > **服务器名称**，复制的服务器名称。

    ![在 Azure 中获取服务器名称](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

2. 在 SSMS >**对象资源管理器中**，单击**连接** > **Analysis Services**。

3. 在**连接到服务器**对话框中，将在服务器名称，然后在粘贴在**身份验证中**，选择下列项之一︰

    **活动目录集成身份验证**要使用单一登录与 Active Directory Azure Active Directory 联合身份验证。

    **活动目录的密码身份验证**要使用的组织帐户。 例如，从非域连接时加入的计算机。

    注意︰ 如果看不到活动目录身份验证，您可能需要 SSMS 中[启用 Azure Active Directory 身份验证](#enable-azure-active-directory-authentication)。

    ![在 SSMS 中连接](./media/analysis-services-manage/aas-manage-connect-ssms.png)

由于使用 SSMS 来管理您的服务器在 Azure 是一样管理内部部署服务器，因此我们不打算进入细节。 所有您需要可以查找帮助在 MSDN 上的[Analysis Services 实例管理](https://msdn.microsoft.com/library/hh230806.aspx)。

## <a name="server-administrators"></a>服务器管理员
可用于**Analysis Services 管理员**控制刀片在 Azure 门户或 SSMS 中服务器管理服务器管理员。 Analysis Services 管理员是数据库服务器的通用数据库管理任务，如添加和移除数据库以及管理用户权限的管理员。 默认情况下，在 Azure 的门户网站中创建服务器的用户自动添加为 Analysis Services 管理员。

您还应该了解︰

-   Windows Live™ ID 不是 Azure Analysis Services 支持的标识类型。  
-   Analysis Services 管理员必须为有效的 Azure Active Directory 用户。
-   如果创建 Azure Analysis Services 服务器通过 Azure 资源管理器模板，Analysis Services 管理员将 JSON 数组应该作为管理员添加的用户。

Analysis Services 管理员可以不同于 Azure 资源管理员可以管理 Azure 预订的资源。 这将保持与现有 XMLA 和 TSML 兼容性管理在 Analysis Services 的行为并允许您隔离 Azure 资源管理与 Analysis Services 之间职责数据库管理。

要查看所有角色和访问您 Azure Analysis Services 的资源的类型，使用在控制刀片上的访问控制 (IAM)。

## <a name="database-users"></a>数据库用户
Azure Analysis Services 模型数据库用户必须将 Azure Active Directory 中。 通过组织的电子邮件地址或 UPN 必须指定为 model 数据库的用户名。 这是不同于内部模型数据库支持的 Windows 域用户名的用户。

通过使用[Azure Active Directory 中的角色分配](../active-directory/role-based-access-control-configure.md)或通过 SQL Server 管理 Studio 中使用[表格模型脚本语言](https://msdn.microsoft.com/library/mt614797.aspx)(TMSL)，您可以添加用户。

**TMSL 脚本示例**

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Users"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@contoso.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="enable-azure-active-directory-authentication"></a>启用 Azure Active Directory 身份验证
若要启用注册表中 SSMS Azure Active Directory 身份验证功能，创建名为 EnableAAD.reg，一个文本文件，然后复制并粘贴以下︰


```
Windows Registry Editor Version 5.00
[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\Microsoft Analysis Services\Settings]
"AS AAD Enabled"="True"
```

保存，然后运行该文件。



## <a name="next-steps"></a>下一步行动
如果您还没有到新服务器上部署了表格模型，现在是一个很好的时间。 若要了解详细信息，请参阅[部署到 Azure Analysis Services](analysis-services-deploy.md)。

如果您已经对服务器部署模型，您就可以连接到使用浏览器或客户端。 若要了解详细信息，请参阅[从 Azure Analysis Services 服务器获取数据](analysis-services-connect.md)。
