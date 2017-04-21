
<properties
    pageTitle="与企业集成包一起使用的证书 |Microsoft Azure"
    description="学习如何与企业集成包和应用程序逻辑中使用证书"
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="msftman"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="deonhe"/>

# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>了解有关证书和企业集成包

## <a name="overview"></a>概述
企业集成使用证书来保护 B2B 通信的安全。 企业集成应用程序中，您可以使用两种类型的证书︰

- 公钥证书，必须从证书颁发机构 (CA) 购买。
- 个人证书，您可以发布您自己。 这些证书有时被称为自签名的证书。


## <a name="what-are-certificates"></a>什么是证书？
证书是用于验证电子通信中的参与者的身份和，还确保电子通信的数字文档。

## <a name="why-use-certificates"></a>为什么要使用的证书？
有时 B2B 通信必须保持机密。 企业集成使用证书来保护这些通信两种方式︰

- 通过加密邮件的内容
- 通过邮件添加数字签名  

## <a name="how-do-you-upload-certificates"></a>您如何上载证书？

### <a name="public-certificates"></a>公钥证书
在 B2B 功能与应用程序逻辑中使用*公共证书*，首先需要将证书传到集成帐户。 若要使用*自签名的证书*，相反，您必须首先将其上载到[Azure 密钥存储库](../key-vault/key-vault-get-started.md "了解密钥存储库")。

上载证书后，可帮助您保护 B2B 消息，当您创建了[协议](./app-service-logic-enterprise-integration-agreements.md)中定义其属性。  

这里是后登录到 Azure 门户集成考虑您上载您的公钥证书的详细的步骤︰

1. 选择**浏览**。  
    ![选择浏览](./media/app-service-logic-enterprise-integration-overview/overview-1.png)  

2. 在筛选器的搜索框中输入**集成**，然后从结果列表中选择**集成帐户**。     
    ![选择集成帐户](./media/app-service-logic-enterprise-integration-overview/overview-2.png)

3. 选择要向其添加证书的集成帐户。  
    ![选择要向其添加证书的集成帐户](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4.  选择**证书**麻将牌。  
    ![选择证书](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)

5. 在打开**证书**刀片式服务器，选择**添加**按钮。
    ![选择添加按钮](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. 输入您的证书**名称**，然后选择证书类型。 （在此示例中，我们使用的公用证书类型）。然后，选择**证书**文本框右侧的文件夹图标。

7. 当打开文件选择器时，查找并选择要上载到您的集成帐户的证书文件。

8. 选择证书，然后在文件选择器中选择**确定**。 此验证和上载到集成帐户的证书。

8. 最后，重新打开**证书添加**刀片式服务器，选择**确定**按钮。  
    ![选择确定按钮](./media/app-service-logic-enterprise-integration-certificates/certificate-3.png)  

9. 在约一分钟，您会看到指示证书上载已完成的通知。

10. 选择**证书**麻将牌。 您应该看到新添加的证书。  
    ![新的证书，请参阅](./media/app-service-logic-enterprise-integration-certificates/certificate-4.png)  

### <a name="private-certificates"></a>个人证书
通过执行以下步骤，可以将专用证书上载到集成帐户︰  

1. [上载您的私钥对密钥存储库](../key-vault/key-vault-get-started.md "了解有关密钥存储库")。  

    > [AZURE.TIP] 您必须授权 Azure 应用程序服务以在密钥存储库上执行操作的应用程序逻辑功能。 您可以通过使用以下 PowerShell 命令逻辑应用程序服务主体授予访问︰`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  

2. 创建一个私有证书。  

3. 将私有证书上传到集成帐户。

已执行上述步骤后，您可以使用私有证书创建协议。

后登录到 Azure 门户集成考虑您上载您的私有证书的详细的步骤如下︰  

1. 选择**浏览**。  
    ![将您个人的证书上传到集成帐户](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    

2. 在筛选器的搜索框中输入**集成**，然后从结果列表中选择**集成帐户**。     
    ![选择集成帐户](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  

3. 选择要向其添加证书的集成帐户。  
    ![选择要向其添加证书的集成帐户](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4. 选择**证书**麻将牌。  
    ![选择证书](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)  

5. 在打开**证书**刀片式服务器，选择**添加**按钮。
    ![选择添加按钮](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. 输入您的证书**名称**，然后选择证书类型。 （在此示例中，我们使用的公用证书类型）。然后，选择**证书**文本框右侧的文件夹图标。

7. 当打开文件选择器时，查找并选择要上载到您的集成帐户的证书文件。

8. 选择该证书后，请在文件选择器中选择**确定**。 此操作验证证书并将其上载到您的集成帐户。

9. 最后，重新打开**证书添加**刀片式服务器，选择**确定**按钮。  
    ![添加证书](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-1.png)  

10. 在约一分钟，您会看到指示证书上载已完成的通知。

11. 选择**证书**麻将牌。 您应该看到新添加的证书。
    ![新的证书，请参阅](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-2.png)  

您上载证书后，就可帮助您保护您的 B2B 消息[协议](./app-service-logic-enterprise-integration-agreements.md)中定义它们的属性时。  

## <a name="next-steps"></a>下一步行动
- [创建使用 B2B 功能逻辑应用程序](./app-service-logic-enterprise-integration-b2b.md)  
- [创建 B2B 协议](./app-service-logic-enterprise-integration-agreements.md)  
- [了解更多有关密钥存储库](../key-vault/key-vault-get-started.md "了解关键的存储库")  
