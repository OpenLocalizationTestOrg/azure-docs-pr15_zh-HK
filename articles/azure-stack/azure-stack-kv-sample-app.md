<properties
    pageTitle="允许应用程序与 revtrieve Azure 堆栈项保险存储机密信息 |Microsoft Azure"
    description="使用示例应用程序来使用 Azure 堆栈密钥存储库"
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

# <a name="run-the-sample-application-for-key-vault"></a>运行示例应用程序的密钥存储库 

在本指南中，您将使用一个示例应用程序从密钥存储库中检索机密信息和密码。

## <a name="download-the-samples-and-prepare"></a>下载示例和准备

从[Azure 密钥存储库客户端示例页面](https://www.microsoft.com/en-us/download/details.aspx?id=45343)下载 Azure 密钥存储库客户端示例。

将解压缩.zip 文件到您的本地计算机中的内容。

读取**README.md**文件 （这是一个文本文件），然后按照说明进行操作。

## <a name="run-sample-1--hellokeyvault"></a>运行示例 #1--HelloKeyVault
HelloKeyVault 是控制台应用程序演示了支持的密钥存储库的关键方案︰

  1. 创建/导入密钥 （HSM 或软件密钥）
  2. 加密内容密钥密码
  3. 包装内容使用密钥存储库的密钥的密钥
  4. 解包内容密钥
  5. 解密密码
  6. 设置一个秘密

该控制台应用程序应该运行进行任何更改，只是将按下列步骤更新 App.Config 中的适当配置设置︰

1. 保险存储 URL、 应用程序主体 ID 和密钥更新 HelloKeyVault\App.config 中的应用程序配置设置。 （可选） 可以使用**scripts\GetAppConfigSettings.ps1**生成的信息。
2. 更新 GetAppConfigSettings.ps1 中的必填字段变量的值。
3. 启动 Windows PowerShell 窗口。
4. 在 PowerShell 窗口内运行 GetAppConfigSettings.ps1 脚本。
5. 复制到 HelloKeyVault\App.config 文件的脚本的结果。


## <a name="next-steps"></a>下一步行动

[部署虚拟机使用密钥存储库密码](azure-stack-kv-deploy-vm-with-secret.md)

[部署虚拟机的证书密钥存储库](azure-stack-kv-push-secret-into-vm.md)