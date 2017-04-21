<properties 
   pageTitle="在 Azure 应用程序服务创建贸易伙伴协议 |Microsoft Azure" 
   description="创建贸易伙伴协议" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
    ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="08/23/2016"
   ms.author="rajram"/>

# <a name="creating-a-trading-partner-agreement"></a>创建一个贸易伙伴协议   

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

贸易伙伴是实体参与 B2B 的 （企业对企业） 通信。 当两个伙伴建立了关系时，这被称为一种*协议*。 定义的协议基于通信这两个合作伙伴希望获得和协议或特定的传输。 包括各种 B2B 协议和 Azure 应用程序服务所支持的传输︰

- AS2 （适用性说明 2）
- EDIFACT （用于管理、 商务和运输 (UN/EDIFACT) 联合国/电子数据交换）
- X12 (ASC X12)

### <a name="biztalk-api-apps-that-support-b2b-scenarios"></a>BizTalk API 的应用程序支持 B2B 方案
以下 API 应用程序启用这些功能使用 Azure 门户中丰富而直观的体验︰


## <a name="biztalk-trading-partner-management-tpm"></a>贸易合作伙伴管理 (TPM) 的 BizTalk
- 创建和管理的合作伙伴、 配置文件和标识
- 存储和管理的 EDI 架构
- 存储和管理证书 （使用 AS2 协议中）
- AS2 协议的创建和管理
- 创建和管理 EDIFACT 协议 （包括批处理发送端）
- 创建和管理 X12 的协议 （包括批处理发送端）

![][1]


## <a name="as2-connector"></a>AS2 连接器
- 在相关的 TPM API 的应用程序实例中定义执行 AS2 协议
- 有关故障排除的曲面 AS2 处理/跟踪信息


## <a name="biztalk-edifact"></a>BizTalk EDIFACT
- 在相关的 TPM API 的应用程序实例中定义执行 EDIFACT 协议
- 有关故障排除的曲面 EDIFACT 处理/跟踪信息
- 在 EDIFACT 协议中定义相关的 TPM API 的应用程序实例中提供状态管理的批处理 （开始和停止）


## <a name="biztalk-x12"></a>BizTalk X12
- 执行 X12 协议相关的 TPM API 的应用程序实例中定义 
- 面，X12 处理/跟踪的疑难解答信息
- 在 X12 提供状态管理的批处理 （开始和停止） 协议中的相关的 TPM API 的应用程序实例

如前面所述，AS2，X 12 和 EDIFACT API 的应用程序要求对函数的 TPM API 的应用程序按预期的方式。


## <a name="getting-started"></a>入门教程
若要创建贸易伙伴协议︰

1. 创建**BizTalk 贸易合作伙伴管理**连接器的实例。 这就要求对函数空 SQL 数据库。 之前开始一定要有一个空白数据库可用并可供使用。
2. 上载的架构和证书按照协议的要求。 通过执行此操作浏览 TPM 实例创建和单步执行到架构和/或证书部分
3. 浏览到 TPM 实例创建并单步执行**合作伙伴**部分
4. 创建所需的合作伙伴。 编辑相应的配置文件并添加所需的标识
5. 现在使用的**协议**部分创建协议。 当您创建了协议时，必须选择要使用的协议。 其余的配置选项基于所选的协议。

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png
 
