<properties
   pageTitle="从 Azure Analysis Services 获取数据 |Microsoft Azure"
   description="了解如何连接到并从 Azure Analysis Services 服务器获取数据。"
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

# <a name="get-data-from-azure-analysis-services"></a>从 Azure Analysis Services 中获取数据
在 Azure，创建一个服务器并部署到该表格模型后, 组织中的用户就可以连接并开始浏览的数据。

Azure Analysis Services 支持客户端连接使用[更新对象模型](#client-libraries);TOM、 AMO、 Adomd.Net 或 MSOLAP，通过 xmla 与服务器连接。 例如，电源 BI、 电源双桌面、 Excel 或任何第三方客户端应用程序支持的对象模型。

## <a name="server-name"></a>服务器名称
在 Azure 创建一个 Analysis Services 的服务器时，您可以指定一个唯一的名称和服务器所要创建的地区。 在连接中指定的服务器名称，则服务器命名方案︰
```
<protocol>://<region>/<servername>
```
 协议是字符串**asazure**，该地区也是 Uri 创建服务器所在的地区 (例如西美国，westus.asazure.windows.net) 和服务器名是服务器的区域中唯一的名称。

## <a name="get-the-server-name"></a>获取服务器名称
在连接之前，您需要获取服务器名称。 在**Azure 门户**> 服务器 >**概述** > **服务器名称**，复制整个服务器名称。 如果您的组织中的其他用户也连接到此服务器，您需要与他们共享该服务器名称。 在指定的服务器名称，必须使用完整路径。

![在 Azure 中获取服务器名称](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connect-in-power-bi-desktop"></a>连接在电源双桌面

> [AZURE.NOTE] 此功能是预览。

1. 在[电源双桌面](https://powerbi.microsoft.com/desktop/)，单击**获取数据** > **数据库** > **Azure Analysis Services**。

2. 在**服务器**将粘贴剪贴板中的服务器名称。

3. 在**数据库**中，如果您知道您想要连接到的角度来看的表格模型数据库的名称，将其粘贴在此处。 否则，可以将此字段保留为空。 您可以选择数据库或在下一个屏幕上的角度来看。

4. 保留默认**实时连接**选项选中，然后按**连接**。 如果系统提示您输入一个帐户时，请输入您组织的帐户。

5. 在**导航器**中，展开服务器，然后选择模型的角度来看，您想要连接，然后单击**连接**。 模型或角度上的单击一次显示该视图的所有对象。


## <a name="connect-in-power-bi"></a>在双电源连接
1. 创建电源双桌面文件服务器上有活动连接到您的模型。

2. 在[电源 BI](https://powerbi.microsoft.com)，单击**获取数据** > **文件**。 找到并选择您的文件。


## <a name="connect-in-excel"></a>在 Excel 中连接
通过使用 Excel 2016 中获取数据或电源在早期版本中的查询连接到 Excel 中的 Azure Analysis Services 服务器支持。 [MSOLAP.7 提供程序](https://aka.ms/msolap)是必需的。 不支持电源数据透视表中使用表向导导入连接。

1. 在 Excel 2016**数据**功能区，单击**获取外部数据** > **从其他源** > **从 Analysis Services**。

2. 在数据连接向导中，在**服务器名称**粘贴剪贴板中的服务器名称。 然后，在**登录凭据**中选择**使用以下的用户名和密码**，然后再键入组织的用户名称，例如nancy@adventureworks.com,和密码。

    ![在 Excel 登录连接](./media/analysis-services-connect/aas-connect-excel-logon.png)

4. 在**选择数据库和表**，数据库和模型或角度来看，选择，然后单击**完成**。

    ![在 Excel 选择模型中连接](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>连接字符串
当连接到 Azure Analysis Services 使用表格的对象模型，使用下面的连接字符串格式︰

###### <a name="integrated-azure-active-directory-authentication"></a>Azure Active Directory 集成的身份验证
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
如果可用的 Azure Active Directory 凭据缓存将选取集成身份验证。 如果不是，则显示了 Azure 登录窗口。

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure Active Directory 使用用户名和密码的身份验证
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="client-libraries"></a>客户端库
当从 Excel 或其他如 TOM、 AsCmd、 ADOMD.NET 的接口连接到 Azure Analysis Services，您可能需要安装最新的提供程序客户端库。 获取最新版本︰  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>



## <a name="next-steps"></a>下一步行动
[管理您的服务器](analysis-services-manage.md)
