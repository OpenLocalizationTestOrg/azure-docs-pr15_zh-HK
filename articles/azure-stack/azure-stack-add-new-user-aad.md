<properties
    pageTitle="在 Azure Active Directory 中添加新的 Azure 堆栈租户帐户 |Microsoft Azure"
    description="在部署 Microsoft Azure 堆栈 POC 之后, 将需要创建至少一个租户的用户帐户，以便您可以查看该租户门户。"
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>在 Azure Active Directory 中添加新的 Azure 堆栈租户帐户

[部署 Azure 堆栈 POC](azure-stack-run-powershell-script.md)之后, 您将需要一个租户的用户帐户以便您可以了解租户门户和测试您的服务和计划。 可以通过[使用 Azure 门户](#create-an-azure-stack-tenant-account-using-the-azure-portal)或通过[使用 PowerShell](#create-an-azure-stack-tenant-account-using-powershell)创建租户帐户。

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>创建使用 Azure 门户 Azure 堆栈租户帐户

您必须先使用 Azure 门户 Azure 订阅。

1. 登录到[Azure](http://manage.windowsazure.com)。

2.  在 Microsoft Azure 左侧的导航栏中，单击**活动目录**。

3.  在目录列表中，单击您想要使用 Azure 堆栈的目录或创建一个新。

4.  在此目录页上，单击**用户**。

5.  单击**添加用户**。

6.  在**添加用户**向导中的**用户类型**列表中，选择**您的组织中的新用户**。

7.  在**用户名**框中，键入用户的名称。

8.  在**@**框中，选择相应的项。

9.  单击下箭头。

10.  在向导的**用户配置文件**页中，键入**名字**、**姓氏**和**显示名称**。

11. 在**角色**列表中，选择**用户**。

12. 单击下箭头。

13. 在**获得临时密码**页上，单击**创建**。

14. 将复制**新的密码**。

15. 使用新帐户登录到 Microsoft Azure。 更改密码提示时。

16. 登录到`https://portal.azurestack.local`与新的帐户，以查看该租户门户。

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>创建使用 PowerShell Azure 堆栈租户帐户

如果您没有订阅了 Azure，不能使用 Azure 的门户添加一个租户的用户帐户。 在这种情况下，可以改为使用 Azure 活动目录模块用于 Windows PowerShell。

> [AZURE.NOTE] 如果使用的 Microsoft 帐户 (Live ID) 来部署 Azure 堆栈 PoC，不能使用 AAD PowerShell 创建组织帐户。 

1.  安装[Microsoft 联机服务注册助理面向 IT 专业人员的一项](https://www.microsoft.com/en-us/download/details.aspx?id=41950)。

2.  安装[Azure 活动目录模块用于 Windows PowerShell （64-位版本）](http://go.microsoft.com/fwlink/p/?linkid=236297)并将其打开。

3.  运行以下 cmdlet:




    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack PoC
   
            $msolcred = get-credential
    
    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".
    
            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId
    
    ```

4.  使用新帐户登录到 Microsoft Azure。 更改密码提示时。

5.  登录到`https://portal.azurestack.local`与新的帐户，以查看该租户门户。



