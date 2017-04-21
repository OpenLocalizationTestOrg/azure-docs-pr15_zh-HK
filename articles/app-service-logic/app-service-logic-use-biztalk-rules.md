<properties
   pageTitle="了解并在您的逻辑应用程序中创建一个 BizTalk 规则 API 应用程序 |Microsoft Azure"
   description="本主题介绍 BizTalk 规则连接器的功能，并提供有关其用法的说明"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="04/20/2016"
   ms.author="andalmia"/>

#<a name="biztalk-rules"></a>BizTalk 规则

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

业务规则封装的策略和控制业务流程的决定。 这些策略可以在过程手册、 合同或协议，正式定义，或者可能存在知识或体现在员工的专业技能。 这些策略是动态的如有更改，通过时间在业务规划、 法规或其他原因造成的更改。

在传统编程语言中实施这些策略需要大量的时间和协调，并不会启用非程序员可以参与创建和维护业务策略。 BizTalk 业务规则使您能够迅速实施这些策略，并将业务流程的其余部分分隔开来。 这样不会影响其余的业务流程对业务策略所需的更改。

##<a name="why-rules"></a>为什么规则

3 在业务流程中使用 BizTalk 业务规则的主要原因有︰

* 将业务逻辑与应用程序代码中分离出来
- 允许业务分析师可以更好地控制业务逻辑管理
+ 对业务逻辑的更改会更快地进入生产

##<a name="rules-concepts"></a>规则的概念

##<a name="vocabulary"></a>词汇

_词汇_是定义在规则条件和操作中使用的计算对象的友好名称所组成的集合。 词汇定义使规则更易于阅读、 理解并共享的特定业务领域中的人。

词汇被设计业务语义和实施之间的桥梁。 例如，审核状态的数据绑定可能指向某些行在某些数据库中，表示为 SQL 查询中的列。 而不是在规则中插入这种复杂的表示形式，您可以改为创建词汇定义，与该数据绑定中，友好名称为"状态"。 随后您可以包括任意数量的规则中的"状态"和规则引擎可以从表中检索相应的数据。

##<a name="rule"></a>规则

业务规则是规定如何开展业务流程的声明性语句。 规则由一个条件和操作。 条件求值，并且如果其值为 true，将规则引擎启动一个或多个操作。
通过使用以下格式定义业务规则框架中的规则︰

_IF__条件__然后__操作_

请考虑以下示例︰

*如果量小于或等于可用资金*  
*然后，执行交易记录和打印收据*

此规则可确定是否将通过应用业务逻辑，在窗体中的交易记录金额和可用资金的形式中的两种货币价值比较的执行事务。
您可以使用业务规则来创建、 修改和部署业务规则。 或者，您可以以编程方式执行上述任务。

###<a name="conditions"></a>条件

条件是真/假 （布尔值） 的表达式的一个或多个谓词组成。
在我们的示例中，谓词小于或等于应用于的数量和可用资金。 这种情况将总是计算为真或假。
谓词可以组合使用逻辑运算符 AND，OR，并不构成逻辑表达式可能非常大，但将总是计算为真或假。

###<a name="actions"></a>操作

操作是正常工作的条件评估后果。 在满足规则条件时，启动相应的操作。
在我们的示例中，"筹办事务"和"打印收据"是何时、 何地只，条件 （在此情况下，"如果小于或等于可用基金"） 为真时执行的操作。
操作执行集合操作 XML 文档表示中业务规则框架。

##<a name="policy"></a>策略

策略是规则的逻辑分组。 您撰写策略、 将其保存、 对其进行测试，满意结果，使用它在生产环境中。

###<a name="policy-composition"></a>策略组合

您可以撰写规则门户中的策略。 一个策略可以包含任意大的一套规则，但通常您撰写适用于特定的业务域的应用程序将使用该策略的上下文中的规则策略。

###<a name="policy-testing"></a>测试策略
在生产环境中使用之前，您可以有效地执行测试运行策略。 规则门户允许您提供输入的一种策略，运行策略，并查看它的输出。 输出中包括日志、 执行规则、 条件评估和结果输出。

##<a name="sample-scenario---insurance-claims"></a>示例场景的保险索赔
让我们采取一个示例方案，并遍历它与我们为同一编写业务逻辑。

![Alt 文本][1]

在非常简单的保险索赔方案中，Claimant 提交他保险索赔 （通过任何客户端如网站、 电话应用程序等）。 此报销申请请求获取发送给业务的索赔处理部门和基于处理的结果，声明可以是任一已批准，已拒绝或沿发送以进行进一步的手动处理。
我们的方案中的索赔处理部门将吸收系统的业务逻辑的一个。 考虑一下此计价单位，我们可以看到以下项目︰

![Alt 文本][2]

让我们现在使用业务规则来实现此业务逻辑。


##<a name="creation-of-rules-api-app"></a>创建的规则 Api 的应用程序


1. 登录到 Azure 门户
2. 选择新建-> 市场，然后搜索*BizTalk 规则*
3. 从结果列表中选择 BizTalk 规则。 打开 BizTalk 规则刀片式服务器
4. 选择*创建*按钮![Alt 文本][3]
1. 在打开新刀片，输入以下信息︰  
    1. 名称 – 授予的规则 API 应用程序名称
    1. 应用程序服务计划 – 选择或创建一个新的应用程序服务计划
    1. 选择您希望该应用程序驻留在定价层定价层 –
    1. 资源组 – 选择或创建资源组，该应用程序中的驻留位置
    2. 选择您想要使用的订阅的订阅
    1. 位置 – 选择您希望该应用程序部署的地理位置。
4.  选择*创建*。 在几分钟内将创建您 BizTalk 规则 API 的应用程序。

##<a name="vocabulary-creation"></a>词汇表创建
创建一个 BizTalk 规则 API 应用程序之后, 下一步是创建词汇。 预期结果是开发人员，是较为常见的角色来做此练习。 下面介绍了如何做到这一点︰


1. 启动您 BizTalk 规则 API 的应用程序从门户进入浏览-> API 的应用程序-><Your Rules API App>。 这样将使您进入规则 API 的应用程序的仪表板与下面类似︰

   ![Alt 文本][4]

2.选择"词汇定义"。 这将显示词汇创作屏幕 3.选择"添加"以开始添加新的词汇定义。
有两种类型的词汇定义当前支持 – 文本和 XML。

##<a name="literal-definition"></a>文本定义
1.  单击"添加"后, 一个新的"添加定义"叶片会打开。 输入以下值
  1.    名称 — 仅包含字母数字字符将不包含任何特殊字符。 这应该是唯一的现有词汇定义列表。
  2.    描述 – 可选字段。
  3.    定义类型 – 有 2 种类型支持。 在此示例中选择文本
  4.    这样用户就可以选择定义的数据类型数据类型 –。 目前支持 4 个数据类型︰ 我。  字符串-输入这些值必须用双引号 （"示例字符串"）  
    二。 布尔值--这可以是真或假  
    iii。    编号 – 可以是任何十进制数字  
    iv。 日期时间--这意味着定义类型为日期。 必须输入数据，使用这种格式 – mm/dd/yyyy︰ 分︰ 秒 AM\PM  
  5. 输入--这是您在其中输入定义的值。 此处输入的值必须符合选定的数据类型。 您可以输入一个值，以逗号或使用关键字*的*值的范围的值的一组。 例如，您可以在其中输入唯一值为 1;一组 1，2，3;或一系列 1 到 5。 请注意，仅支持范围的数字。
  6. 选择*确定*。

![Alt 文本][5]
##<a name="xml-definition"></a>XML 定义
如果以 XML 形式选择词汇类型，则需要指定以下输入  
  一。    架构 – 单击此会打开新刀片式服务器允许用户从已上载的架构或允许上载一个新的列表中选择。
b。    此输入获取 XPATH – 选择前面步骤中的架构后，才解除锁定。 单击这将显示选定并允许用户选择一个词汇定义需要创建的节点的架构。  
  c。    事实 – 此输入标识的数据对象将送入到规则引擎进行处理。 这是一个高级的属性，默认情况下设置为所选的 XPATH 的父。 事实将成为链和集合的情况下尤为重要。

![Alt 文本][6]

### <a name="add-bulk"></a>添加大容量
上述步骤已捕获创建词汇定义的体验。 一旦创建，创建的词汇定义将以列表形式显示。 有一些要求，以便能够从相同的架构，而不是每一次重复上面的步骤中生成多个定义。 这是添加大容量功能变得非常有用。
选择"批量添加"将进入一个新的叶片。 第一步是选择要创建多个定义的架构。 选择这将打开新的刀片，从而允许您从已上载的架构列表中选择，或允许上载一个新。
现在 XPATH 属性获取锁定。 如果选择此会打开架构查看器可以选择多个节点的位置。
创建的多个定义的名称将默认为选定的节点的名称。 这些可以始终创建后可修改。

![Alt 文本][7]

##<a name="policy-creation"></a>创建策略
一旦开发人员创建所需的词汇，预期结果是业务分析师会通过 Azure 门户创建一个业务策略。  
    1.在创建该规则的应用程序的情况下，没有单击该用户将转到策略创建页面策略镜头。  
    2.此页面将显示具有该特定规则的应用程序策略的列表。 分析师可以添加新的策略只需键入策略的名称，并两次按 tab 键。 多个策略可以驻留在单个规则 API 应用程序中。  
    3.选择创建的策略将使用户进入策略详细信息页面，在其中可以查看策略中的规则。  
    ![Alt 文本][8]
4。  选择"添加"以添加新规则。 这将为新的刀片。

##<a name="rule-creation"></a>创建规则
规则是的条件和操作的语句集合。 如果条件的计算结果为 true，则操作来执行。 在创建规则刀片式服务器，使唯一的规则 （用于该策略） 的名称和描述 （可选）。
用于创建复杂的条件语句的条件 （如果） 框。 以下是受支持的关键字︰  
1.  和 – 条件运算符  
2.  或--运算符的条件  
3.  does\_不\_存在  
4.  存在  
5.  假  
6.  是\_等于\_到  
7.  是\_大于\_比  
8.  是\_大于\_比\_等于\_到  
9.  是\_中  
10. 是\_较少\_比  
11. 是\_较少\_比\_等于\_到  
12. 是\_不\_中  
13. 是\_不\_等于\_到  
14. 求模  
15. 真  

Action(THEN) 框中可以包含多个语句，每行一个，创建要执行的操作。 以下是受支持的关键字︰  
1.  等于  
2.  假  
3.  真  
4.  暂停  
5.  求模  
6.  为空  
7.  更新  

条件和操作框提供了智能感知可帮助您快速编写一条规则。 这可以通过按 ctrl + 空格或刚开始键入触发。 关键字匹配键入的字符将自动关闭筛选和显示。 智能感知窗口将显示所有的关键字和词汇定义。
![Alt 文本][9]

##<a name="explicit-forward-chaining"></a>显式的正向链接
BizTalk 规则支持显式向前链接，如果用户想要重新评估在响应某些操作的规则，它们可触发此警报通过某些关键字。 以下是受支持的关键字︰  
   1.   更新<vocabulary definition>– 此关键字重新计算其条件中使用的指定的词汇定义的所有规则。  
   2.   暂停 — 此关键字将停止所有规则执行

##<a name="enabledisable-rules"></a>Enable\Disable 规则
可以启用或禁用每个策略中的规则。 默认情况下启用了所有规则。 禁用的规则不会在策略执行期间执行。 Enable\Disable 规则可以完成从规则刀片式服务器直接 – 命令都可用在命令栏顶部的刀片式服务器，或从该策略中，上下文菜单 （右键单击规则） 还提供了 enable\disable 选项。

##<a name="rule-priority"></a>规则优先级
按顺序执行策略的所有规则。 由发生策略的顺序决定的执行优先级。 只需拖动和放置规则，可以更改此优先级。

##<a name="test-policy"></a>测试策略
在测试策略刀片式服务器使用"测试策略"命令，可以测试您的策略。 在此刀片式服务器中，您可以看到列表策略中使用要求用户输入的词汇定义。 用户可以手动添加这些输入其测试方案的值。 此外，用户还可以选择导入测试输入的 XMLs。 在所有的输入后，可以运行测试，每个词汇定义的输出将显示在输出列以便于进行比较。 要查看业务分析员友好日志，请单击"查看日志"可以查看执行日志。 若要保存日志，"保存输出"选项可以用来存储所有测试独立分析的相关的数据。

## <a name="using-rules-in-logic-apps"></a>在应用程序逻辑中使用规则
一旦已编写和测试策略，就可供消耗。 通过从该门户的主页的左侧选择逻辑应用程序，您可以创建新的逻辑应用程序。 一旦已经创建了您的逻辑的应用程序，启动它，然后选择*触发器和操作*。 然后，您可以选择*从头开始创建*模板。 按照步骤将 BizTalk 规则 API 应用程序添加到应用程序逻辑。 完成此操作后，将会有一个选项，以选择哪些规则 API 的应用程序 （操作） 到目标。 操作包括要执行的策略的列表。 选择需要获得特定策略后，输入所需的策略。 为进一步的决策过程，用户可以使用规则 API 的应用程序向下游的输出。

## <a name="using-rules-via-apis"></a>使用通过 Api 的规则
也可以使用一组丰富的 Api 调用规则 API 的应用程序。 此方法的用户并不限于仅使用逻辑应用程序，可以通过调用其他任何应用程序中使用规则。 可以通过"API 定义"镜头，在规则操控板上单击查看精确的 REST Api。

![Alt 文本][10]

下面是如何在 C 中使用此 API 的示例#

            // Constructing the JSON message to use in API call to Rules API App

            // xmlInstance is the XML message instance to be passed as input to our Policy
            string xmlInstance = "<ns0:Patient xmlns:ns0=\"http://tempuri.org/XMLSchema.xsd\">  <ns0:Name>Name_0</ns0:Name>  <ns0:Email>Email_0</ns0:Email>  <ns0:PatientID>PatientID_0</ns0:PatientID>  <ns0:Age>10.4</ns0:Age>  <ns0:Claim>    <ns0:ClaimDate>2012-05-31T13:20:00.000-05:00</ns0:ClaimDate>    <ns0:ClaimID>10</ns0:ClaimID>    <ns0:TreatmentID>12</ns0:TreatmentID>    <ns0:ClaimAmount>10000.0</ns0:ClaimAmount>    <ns0:ClaimStatus>ClaimStatus_0</ns0:ClaimStatus>    <ns0:ClaimStatusReason>ClaimStatusReason_0</ns0:ClaimStatusReason>  </ns0:Claim></ns0:Patient>";

            JObject input = new JObject();

            // The JSON object is to be of form {"<XMLSchemName>_<RootNodeName>":"<XML Instance String>".
            // In the below case, we are using XML Schema - "insruanceclaimsschema" and the root node is "Patient".
            // This is CASE SENSITIVE.
            input.Add("insuranceclaimschema_Patient", xmlInstance);
            string stringContent = JsonConvert.SerializeObject(input);


            // Making REST call to Rules API App
            HttpClient httpClient = new HttpClient();

            // The url is the Host URL of the Rules API App
            httpClient.BaseAddress = new Uri("https://rulesservice77492755b7b54c3f9e1df8ba0b065dc6.azurewebsites.net/");
            HttpContent httpContent = new StringContent(stringContent);
            httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json");

            // Invoking API "Execute" on policy "InsruranceClaimPolicy" and getting response JSON object. The url can be gotten from the API Definition Lens
            var postReponse = httpClient.PostAsync("api/Policies/InsuranceClaimPolicy?comp=Execute", httpContent).Result;

请注意，上面的规则 API 应用程序安全设置设置为"Anon 提供公用"。 这可以从 API 应用程序中使用设置-所有的设置-> 应用程序设置-> 访问级别。

![Alt 文本][11]

## <a name="editing-vocabulary-and-policy"></a>编辑策略和词汇
使用业务规则的主要优势之一是，对业务逻辑的更改可以推送到生产得更快。 对词汇和策略所做的任何更改将立即应用在生产环境中。 用户只需浏览到相应的词汇定义或策略并进行更改以使其生效。

<!--Image references-->
[1]: ./media/app-service-logic-use-biztalk-rules/InsuranceScenario.PNG
[2]: ./media/app-service-logic-use-biztalk-rules/InsuranceBusinessLogic.png
[3]: ./media/app-service-logic-use-biztalk-rules/CreateRuleApiApp.png
[4]: ./media/app-service-logic-use-biztalk-rules/RulesDashboard.png
[5]: ./media/app-service-logic-use-biztalk-rules/LiteralVocab.PNG
[6]: ./media/app-service-logic-use-biztalk-rules/XMLVocab.PNG
[7]: ./media/app-service-logic-use-biztalk-rules/BulkAdd.PNG
[8]: ./media/app-service-logic-use-biztalk-rules/PolicyList.PNG
[9]: ./media/app-service-logic-use-biztalk-rules/RuleCreate.PNG
[10]: ./media/app-service-logic-use-biztalk-rules/APIDef.PNG
[11]: ./media/app-service-logic-use-biztalk-rules/PublicAnon.PNG
