<properties
    pageTitle="部署虚拟机使用 Azure 堆栈密钥存储库中存储的密码 |Microsoft Azure"
    description="了解如何部署虚拟机使用 Azure 堆栈密钥存储库中存储的密码"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="deploy-a-vm-by-retrieving-the-password-stored-in-key-vault"></a>通过检索存储在密钥存储库中的密码部署虚拟机

当您需要在部署过程中作为参数进行传递 （如密码） 的安全值时，您可以为机密 Azure 堆栈的密钥存储库中存储该值并引用 Azure 资源管理器中的其他模板中的值。 因此，永远不会公开机密只参考机密包括到模板中。 您不需要手动输入的值将机密部署资源的每次。 您指定的用户或服务主体可以访问该密钥。

## <a name="reference-a-secret-with-static-id"></a>引用静态 ID 与一个秘密

引用在参数文件中，将值传递到模板中的秘密。 您通过将密钥存储库的资源标识符和机密的名称引用机密。 在此示例中，必须已存在的密钥存储库的机密。 静态值用于资源 id。

    "parameters": {
    "adminPassword": {
    "reference": {
    "keyVault": {
    "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
    },
    "secretName": "sqlAdminPassword"


>[AZURE.NOTE]接受秘密参数应为*securestring*。

## <a name="next-steps"></a>下一步行动
[部署与密钥存储库的示例应用程序](azure-stack-kv-sample-app.md)

[部署虚拟机的证书密钥存储库](azure-stack-kv-push-secret-into-vm.md)

