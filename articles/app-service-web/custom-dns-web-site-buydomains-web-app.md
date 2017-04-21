<properties
    pageTitle="如何购买 Azure 应用程序服务 Web 应用程序中的自定义域名"
    description="了解如何购买在 Azure 应用程序服务 web 应用程序自定义的域名。"
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="buy-and-configure-a-custom-domain-name-in-azure-app-service"></a>购买并在 Azure 应用程序服务中配置自定义域名

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

创建 web 应用程序时，Azure 将其赋给一个 azurewebsites.net 的子域中。 例如，如果您的 web 应用程序名为**contoso**，URL 是**contoso.azurewebsites.net**。 Azure 还会指定一个虚拟 IP 地址。

用于生产的 web 应用程序，您可能希望用户看到自定义域名。 本文解释如何购买和使用[应用程序服务 Web 应用程序](http://go.microsoft.com/fwlink/?LinkId=529714)配置自定义的域。 

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## <a name="overview"></a>概述

如果您没有为您的 web 应用程序的域名，您可以轻松地购买[Azure 门户](https://portal.azure.com/)上的其中一个。 在采购过程中您可以选择有 WWW 和根域的 DNS 记录将自动映射到您的 web 应用程序。 您还可以管理 Azure 门户内您域的权利。


使用以下步骤来购买域名，并将分配给您的 web 应用程序。

1. 在浏览器中，打开[Azure 门户](https://portal.azure.com/)。

2. 在**Web 应用程序**选项卡，单击您的 web 应用程序的名称，选择**设置**，然后选择**自定义的域**

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

3. 在**自定义的域**刀片式服务器，请单击**购买域**。

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

4. 在**购买域**刀片式服务器，使用文本框中键入域名称要购买，并按 enter 键。 建议可用的域将显示下方的文本框中。 选择您想要买哪些的域。 您可以选择同时购买多个域。 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

5. 单击**联系人信息**，并填充域的联系人信息窗体。

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-3.png)

    > [AZURE.NOTE] 请务必填写尽可能精确越好，尤其是电子邮件地址的所有必填字段。 在购买无"隐私保护"域，您可能会要求域生效之前验证您的电子邮件。 在某些情况下，不正确的数据有关的联系信息将导致购买域失败。 

6. 现在，您可以选择，

    a）"自动更新"域每年
    
    b） 自愿加入"隐私保护"包括购买价格中免费的 (除了 Tld 的注册表执行人员不支持的隐私。 例如:。 co.in，。 co.uk 等。)  
    
    c）"分配默认主机名"WWW 和根域添加到当前 Web 应用程序。 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.5.png)
  
    > [AZURE.NOTE] 选项 C 配置 DNS 绑定和自动为您的主机名称绑定。  这样一来，可一旦采购完毕 (baring DNS 传播延迟，在一些情况下) 使用自定义的域访问您的 Web 应用程序。 在的情况下，您的 Web 应用程序是 Azure 流量管理器后，您看不到选项，将根域中，A 记录不工作与流量管理器。 您可以始终指定域/子-domains 购买通过一个 Web 应用程序向另一个 Web 应用程序，反之亦然。 第 8 步的更多详细信息，请参阅。 
    
7. 单击**选择****购买域**刀片式服务器，则会出现购买信息**购买确认**刀片式服务器上。 如果您接受的法律条款，并单击**购买**，将提交您的订单，您可以监视**通知**采购流程。 域采购可能需要几分钟才能完成。 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-4.png)

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-5.png)

8. 是否成功订购一个域，您可以管理域并将分配给您的 web 应用程序。 单击**"..."**在您的域的右侧。 然后，您可以**取消采购**或**管理域**。 单击**管理域**，然后我们可以将**子域**绑定到我们**管理域**刀片式服务器上的 web 应用程序。 如果您想要将**子域**绑定到不同的 Web 应用程序再执行此从各自的 Web 应用程序的上下文中的步骤。 在这里您只需选择流量管理器分配到通信管理器终结点的域 （如果 Web 应用程序后 TM） 选择命名从下拉菜单。 这样，域/子域将自动分配给该流量管理器终结点后面的所有 Web 应用程序。 

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-6.png)

    > [AZURE.NOTE] 您可以"取消采购"在 5 日内以全额退款。 后 5 天内，您将不能对"取消采购"，相反，您将看到一个选项"删除"域。 删除域会导致释放它从您的订阅而不退款，并将成为可用的域。 

完成配置后，您的 web 应用程序的**主机名绑定**部分中将列出自定义域名。

此时，您应该能够在浏览器中输入自定义域名并查看，它成功地将带您到您的 web 应用程序。
 
## <a name="what-happens-to-the-custom-domain-you-bought"></a>怎么样您购买自定义域

自定义您买了**自定义的域和 SSL**刀片式服务器中的域与 Azure 的订阅。 Azure 的资源，为此自定义的域从您第一次购买的域应用程序服务应用程序是独立的不同。 这意味着︰

- 在 Azure 的门户，可以使用自定义域，您购买了多个应用程序服务应用程序，而不只是第一次购买用于自定义的域的应用程序。 
- 您可以管理转到**自定义的域和 SSL**刀片式服务器的*任何*应用程序服务应用程序中的订阅购买 Azure 的预订中的所有自定义域。
- 您可以向该自定义的域中的一个子域从相同的 Azure 订阅分配任何应用程序服务的应用程序。
- 如果您决定要删除的应用程序服务的应用程序，您可以选择不删除自定义如果您想继续使用它的其他应用程序绑定到的域。

## <a name="if-you-cant-see-the-custom-domain-you-bought"></a>如果看不到自定义的域所购买

如果您购买了从自定义域中**自定义的域和 SSL**刀片式服务器，但看不到在**受管理域**下的自定义域，请验证以下操作︰

- 创建自定义的域可能无法完成。 检查进度的顶部的 Azure 门户通知铃声。
- 创建自定义的域可能由于某种原因失败。 检查进度的顶部的 Azure 门户通知铃声。
- 自定义的域可能已成功，但不是可能尚未刷新刀片式服务器。 请尝试重新打开**自定义的域和 SSL**刀片式服务器。
- 在某些时候自定义的域可能已删除。 通过单击**设置**中检查审核日志 > 从您的应用程序的主刀片式服务器的**审核日志中**。 
- 在所需**自定义的域和 SSL**刀片式服务器可能属于不同的 Azure 订阅中创建的应用程序。 切换到另一个应用程序在不同的订阅，并检查其**自定义的域和 SSL**的刀片式服务器。  
  在门户，您将无法查看或管理其他 Azure 订阅比应用程序中创建自定义的域。 但是，如果您单击**高级管理**中的域**管理域**刀片式服务器，您将被重定向到域提供商的网站，在其中您可以  [手动](web-sites-custom-domain-name.md)配置您自定义的域像自定义任何外部域 
  应用程序中其他 Azure 订阅创建的。 


