<properties
   pageTitle="将 Azure 订阅的所有权转移 |Microsoft Azure"
   description="如何将 Azure 订阅经常转移到另一个用户，而另一些常见问题 (FAQ) 过程"
   services=""
   documentationCenter=""
   authors="genlin"
   manager="stevenpo"
   editor=""
   tags="billing,top-support-issue"/>

<tags
   ms.service="billing"
   ms.workload="na"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/10/2016"
   ms.author="genli"/>

# <a name="transferring-ownership-of-an-azure-subscription"></a>将 Azure 订阅的所有权转移

是吗：

- 需要移交付费给其他人您 Azure 订阅的所有权？
- 要更改用于注册 Azure 的帐户吗？ 也许您使用您的 Microsoft 帐户但供使用您的工作或学校帐户改为？
- 要将 Azure 订购从一个目录移动到另一个？
- Azure 和 Office 365 中不同租户和想要整合？

现在你可以很容易地在 Microsoft Azure 帐户中心-即付即用、 MSDN，操作包或 BizSpark 订阅。  我们已经添加了传送给另一个用户订阅的能力。 换句话说，您现在可以更改任何即付即用、 MSDN、 行动包或 BizSpark 订购您所拥有的不管哪个国家您在操作帐户管理。 我们现在对于这些的订阅类型支持传输的 Azure 市场采购。

> [AZURE.NOTE] 若要更改您的订阅提供不同的服务，信息，请参阅[交换机 Azure 订阅为其他服务](billing-how-to-switch-azure-offer.md)。 如果您需要在此文章中的任何时候更多的帮助，请[与支持人员联系](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)以获取您的问题尽快解决。

## <a name="how-to-transfer-ownership-of-an-azure-subscription"></a>如何将 Azure 订阅的所有权转移

> [AZURE.VIDEO transfer-an-azure-subscription]

1.  登录<https://account.windowsazure.com/Subscriptions>。 您无需执行所有权转移帐户管理员。 有关如何找出是谁订阅的帐户管理员的详细信息，请参阅[常见问题](#faq)。

2.  选择要传输的订阅。

3.  单击**传送订阅**选项。

    ![Azure 帐户订阅选项卡](./media/billing-subscription-transfer/image1.png)

4.  按照提示指定收件人。

    ![转移订阅对话框](./media/billing-subscription-transfer/image2.PNG)

5.  收件人将自动获得接受链接的电子邮件。

    ![订阅传送给收件人的电子邮件](./media/billing-subscription-transfer/image3.png)

6.  收件人单击链接并按照说明进行操作，包括输入付款信息。

    ![第一个订阅传送网页](./media/billing-subscription-transfer/image4.png)

    ![第二个订阅传送网页](./media/billing-subscription-transfer/image5.png)

7. 成功 ！ 现在传送订阅。

<a id="faq"></a>
## <a name="frequently-asked-questions-faq"></a>常见问题 (FAQ)

-   **如何知道谁是订阅的帐户管理员吗？**

    您可以确认谁是订阅的帐户管理员，如下所示︰

    1. 登录到[Azure 的门户](https://portal.azure.com)。
    2. 在集线器菜单选择**订阅**。
    3. 选择您要检查，的订阅，然后选择**设置**。
    4. 选择**属性**。 在**帐户管理**框中将显示订阅的帐户管理员。  

-   **订购转移是否导致任何服务停机时间？**

    没有对该服务没有任何影响。 这实际上取消订阅当前帐户管理员和收件人的帐户下创建一个新但将基础 Azure 服务相关联的新订阅。 订阅 ID 保持不变。

-   **如何使用这种机制来更改订阅的目录**-   
    Azure 订阅帐户管理所属的目录中创建。 因此，若要更改目录，只是转移订阅到目标目录中的用户帐户。 当该用户完成接受传输的步骤时，订阅将自动移动到目标目录中。

-   **如果我接管付费订阅来自另一个组织的所有权，它们将继续有权访问我的资源？**

    如果订阅被转移到另一个租户，与前一个租户的用户将丢失对订阅的访问。 即使用户不再服务管理或 Co 管理，他们可能仍然有权通过其他安全机制的订阅。 其中包括︰
    - 授予用户管理权限订阅资源的管理证书。 有关详细信息，请参阅[创建并上载 Azure 管理证书](https://msdn.microsoft.com/library/azure/gg551722.aspx)
    -   对服务的访问键，如存储。 有关详细信息，请参阅[查看、 复制和重新生成存储访问键](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
    -   对于服务类似 Azure 的虚拟机的远程访问凭据

    这不是完整列表。 收件人应考虑更新他们需要限制访问其资源与服务关联的任何机密。 大多数资源可以更新，如下所示︰

    1.   转到 Azure 的门户网站︰ [ *https://portal.azure.com*](https://portal.azure.com)

    2.    单击浏览所有的&gt;的所有资源

    3.    选择的资源。 这将打开资源刀片式服务器。

    4.    在资源刀片式服务器，单击**设置**。 您可以在此处查看和更新现有的机密信息。


-   **整个帐单收件人付薪周期如果我把电话转接预订计费周期的中间？**

    发件人负责传送完毕为止报告的任何使用情况付款。 收件人是负责使用报告从开始向上传输的时间。 可能在传输之前发生但随后报告的一些用法。 这将包括在收件人的清单。

-   **收件人是否有权使用和帐单历史记录？**

    这次泄露给收件人的唯一信息是最后一个清单 （或当前余额，如果订阅此前之前生成第一个清单）。 其余的用法和帐单历史记录不传输与此订阅。

-   **可以在传输过程中更改此项优惠活动**

    此项优惠活动必须保持不变。 若要更改您的服务，必须[与支持部门联系](http://go.microsoft.com/fwlink/?LinkID=619338)。

-   **可以将订阅转移到另一个国家中的用户帐户？**

    不，目前不支持此功能。 收件人的用户帐户必须在同一个国家。

-   **收件人可以使用不同的支付机制？**

    是的。 有以下限制︰ 现在帐单历史记录的订阅被分割成两个帐户。 但优点是您可以这样做，而不必[与支持部门联系](http://go.microsoft.com/fwlink/?LinkID=619338)。

-   **我转移的 Azure 订阅后是否会影响付款方法？**

    为了接受预订传输，必须提供信用卡或类似的支付方式支付订阅。 例如，如果 Bob 将移交给 Jane 订阅，Jane 接受转移，Jane 还必须提供她将用于支付订阅的付款方式。 传输完成后，Bob 将不再收取他转给 Jane 的订阅。

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>下一步行动之后接受订阅的所有权

1. 现在您将帐户管理员。 检查并更新服务管理员和共同的管理员。 通过转到设置管理在[Azure 的传统门户网站](https://manage.windowsazure.com)的管理员。 [了解更多](http://go.microsoft.com/fwlink/?LinkID=533293)。
2. 对于您的订购和服务，还可以使用基于角色的访问控制 (RBAC)。 [Azure 门户网站](https://portal.azure.com)[了解更多有关 RBAC](http://go.microsoft.com/fwlink/?LinkID=544802)访问
3. 更新与此订阅服务的凭据。 其中包括︰
    - 授予用户管理权限订阅资源的管理证书。 有关详细信息，请参阅[创建并上载管理 Azure 的证书](https://msdn.microsoft.com/library/azure/gg551722.aspx)
    -   对服务的访问键，如存储。 有关详细信息，请参阅[查看、 复制和重新生成存储访问键](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
    -   对于服务类似 Azure 的虚拟机的远程访问凭据
4. 更新此订阅， [Azure 帐户中心](https://account.windowsazure.com/Subscriptions)[了解更多](http://go.microsoft.com/fwlink/?LinkID=533292)的付费通知  
5.  如果您正在使用一个伙伴，考虑更新此订阅的合作伙伴 ID。 你可以在[Azure 帐户中心](https://account.windowsazure.com/Subscriptions)。

> [AZURE.NOTE] 如果您仍然有进一步问题，请[与支持人员联系](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)以获取您的问题尽快解决。
