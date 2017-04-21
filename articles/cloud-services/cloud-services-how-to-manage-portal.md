<properties 
    pageTitle="公共云服务管理任务 |Microsoft Azure" 
    description="了解如何管理 Azure 门户中的云服务。 这些示例使用 Azure 的门户。" 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/02/2016"
    ms.author="adegeo"/>


# <a name="how-to-manage-cloud-services"></a>如何管理云服务

> [AZURE.SELECTOR]
- [Azure 门户](cloud-services-how-to-manage-portal.md)
- [Azure 的传统门户网站](cloud-services-how-to-manage.md)

云服务管理 Azure 门户的**云服务 （经典）**区域中。 本文介绍了一些常用的操作，您将管理云服务的同时进行。 其中包括更新、 删除、 扩展和提升分阶段的部署到生产环境。

有关如何扩展您的云服务的详细信息可[在此处](cloud-services-how-to-scale-portal.md)。

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>如何︰ 更新云服务角色或部署

如果您需要更新您的云服务的应用程序代码，请在云服务刀片式服务器上使用**更新**。 您可以更新单个角色或所有角色。 若要更新，您可以上载新的服务包或服务配置文件。

1. 在[Azure 的门户][]中，选择您要更新的云服务。 此步骤将打开云服务实例刀片式服务器。

2. 在刀片式服务器，请单击**更新**按钮。

    ![更新按钮](./media/cloud-services-how-to-manage-portal/update-button.png)

3. 新的服务包文件 (.cspkg) 和服务配置文件 (.cscfg) 来更新部署。

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **（可选）**更新部署标签和存储帐户。 

5. 如果任何角色都只有一个角色实例，选择**部署，即使一个或多个角色中包含的单个实例**启用才能继续升级。 

    Azure 只能保证 99.95%的服务可用性云服务更新的过程中每个角色是否至少两个角色实例 （虚拟机）。 两个角色实例，与一台虚拟机将处理客户端请求，而其他更新。

6. 检查**启动部署**包的上载完毕后，将应用了更新。

7. 单击**确定**开始更新服务。



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>如何︰ 换部署宣传分阶段的部署到生产环境

当您决定部署新的发行版的云服务，舞台和云服务暂存环境中测试新的发行版。 使用**交换**切换两个部署解决和提升到生产主机的新发行的 Url。 

您可以交换**云服务**页或控制板中的部署。

1. 在[Azure 的门户][]中，选择您要更新的云服务。 此步骤将打开云服务实例刀片式服务器。

2. 在刀片式服务器，单击**交换**按钮。

    ![云服务交换](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. 打开下面的确认提示。

    ![云服务交换](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. 验证部署信息后，单击**确定**可供换用的部署。

    部署交换速度很快因为仅更改了虚拟 IP 地址 (Vip) 的部署。

    为了节省计算成本，您可以验证生产部署工作正常后删除临时部署。

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>如何︰ 将资源链接到云服务

Azure 门户不会像当前的 Azure 经典门户一起链接资源。 相反，将更多的资源部署到云服务正在使用相同的资源组。

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>如何︰ 删除部署和云服务

您可以删除云服务之前，您必须删除每个现有的部署。

为了节省计算成本，您可以验证生产部署工作正常后删除临时部署。 您已停止的部署的角色实例的计算成本计费。

使用以下过程删除部署或云服务。 

1. 在[Azure 的门户][]中，选择要删除的云服务。 此步骤将打开云服务实例刀片式服务器。

2. 在刀片式服务器，请单击**删除**按钮。

    ![云服务交换](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. 您可以删除整个云服务通过**云服务和其部署**检查或选择**生产部署**或**临时部署**。

    ![云服务交换](./media/cloud-services-how-to-manage-portal/delete-blade.png) 

4. 单击底部的**删除**按钮。

5. 若要删除云服务，请单击**删除云服务**。 然后，看到确认提示时，单击**是**。

> [AZURE.NOTE]
> 当云服务被删除，并且详细的监视配置时，您必须手动删除数据从您的存储帐户。 有关在何处查找度量表的信息，请参阅[这](cloud-services-how-to-monitor.md)篇文章。

[Azure 门户]: https://portal.azure.com

## <a name="next-steps"></a>下一步行动

* [云服务的常规配置](cloud-services-how-to-configure-portal.md)。
* 了解如何[部署云服务](cloud-services-how-to-create-deploy-portal.md)。
* 配置[自定义的域的名称](cloud-services-custom-domain-name-portal.md)。
* 配置[ssl 证书](cloud-services-configure-ssl-certificate-portal.md)。