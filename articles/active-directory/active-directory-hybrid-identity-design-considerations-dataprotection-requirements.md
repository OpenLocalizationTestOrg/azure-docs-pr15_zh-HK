<properties
    pageTitle="Azure Active Directory 混合标识设计考虑事项--确定数据保护要求 |Microsoft Azure"
    description="当规划混合身份解决方案，标识为您的业务和哪个选项可以最好地满足这些要求的数据保护要求。"
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

#<a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>增强数据安全性通过强标识解决方案的计划

若要保护数据的第一步是确定谁可以访问该数据，您需要有一个标识，此过程的一部分，可以集成的解决方案与您的系统以提供身份验证和授权功能。 身份验证和授权往往互相混淆和理解他们的角色。 实际上它们是完全不同，如下图所示︰

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)
 
**移动设备管理生命周期阶段**

规划混合身份解决方案时必须了解您的业务和哪个选项可以最好地满足这些要求的数据保护要求。
 
>[AZURE.NOTE]
一旦您完成规划数据安全性，评审[确定多因素身份验证的要求](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)以确保您的选择相关的多因素身份验证要求而不受本部分中所做的决定。

## <a name="determine-data-protection-requirements"></a>确定数据保护需求
在移动时代，大多数公司都有一个共同目标︰ 使他们的用户都能高效工作他们在本地或远程的移动设备随时随地以提高生产效率。 这可能是一个共同目标，而有这种要求的公司也将必须减轻为了保护公司数据的安全和维护用户的隐私的威胁的数量有关的问题。 每个公司; 这方面可能有不同的要求将改变根据哪些行业作为公司不同的法规遵从性规则将导致不同的设计决策。 

但是，有一些，应研究和验证，而不考虑工业，将在下一节中介绍的安全方面。

## <a name="data-protection-paths"></a>数据保护路径

![](./media/hybrid-id-design-considerations/data-protection-paths.png)
 
**数据保护路径**

在上面的图中，标识组件将之前访问数据时要验证的第一个。 但是，该数据可以在不同的状态期间被访问的时间。 在此关系图上的每个数字表示的数据可以位于在某些时候中时间的路径。 这些数字说明如下︰

1. 在设备级别的数据保护。
2. 在传输过程中的数据保护。
3. 同时部署其余的数据保护。
4. 同时在云环境中的其余部分的数据保护。

虽然技术控制将使 IT 部门能够在这些阶段中的每一个上保护数据本身不会直接提供混合身份解决方案，有必要混合身份解决方案是能够利用内部和云标识管理资源来标识之前该用户授予对数据的访问。 当规划混合身份解决方案确保，根据您的组织要求回答以下问题︰

## <a name="data-protection-at-rest"></a>在其余的数据保护
无论数据所处的位置 （设备、 云或内部） 的其余部分，务必要执行一项评估以了解组织需要在这方面。 此区域中，请确保正在询问以下问题︰

- 贵公司是否需要保护静态数据的？
 - 如果是的话，是能与您当前的内部部署基础结构集成混合身份解决方案？
 - 如果是的话，是能与您的工作负载在云环境中集成混合身份解决方案？
- 是云的身份管理能够保护用户的凭据和其他数据存储在云环境中？

## <a name="data-protection-in-transit"></a>在传输过程中的数据保护
必须保护设备和数据中心之间或设备和云之间的传输中的数据。 但是，在传输过程中不一定意味着与云服务; 外部组件的通信进程而在内部移动，另外，例如两个虚拟网络。 此区域中，请确保正在询问以下问题︰

- 贵公司是否需要来保护传输中的数据？
 - 如果是的话，是否能如 SSL/TLS 的安全控件相结合混合身份解决方案？
- 云的身份管理是否保留签名的通信量和目录存储 （内部和数据中心之间） 内？


## <a name="compliance"></a>法规遵从性
管理法规、 法律和法规遵从性要求会因您的公司所属行业。 高管控行业中的公司必须满足法规遵从性问题与相关的标识管理问题。 如萨班斯-奥克斯利法案 (SOX)、 健康保险便携性和责任法案 (HIPAA)、 金融服务现代化法案法案 (GLB) 以及支付卡行业数据安全标准 (PCI DSS) 的规定是非常严格有关标识和访问权限。 您的公司将采用混合身份解决方案必须具有将满足一个或多个这些法规所要求的核心功能。 此区域中，请确保正在询问以下问题︰

- 是混合身份解决方案符合管理法规的要求，为您的业务？
- 没有混合身份解决方案提供了内置功能，会使您的公司能够符合管理法规要求？ 
 
>[AZURE.NOTE]
请确保每个回答的记录笔记并了解答案背后的基本原理。 [定义数据保护战略](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)超出可用选项以及每个选项的优点/缺点。  通过无回答的问题会选择哪个选项可以最好地满足您的业务需要。

## <a name="next-steps"></a>下一步行动
 [确定内容管理需求](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)


## <a name="see-also"></a>请参见
[设计考虑事项概述](active-directory-hybrid-identity-design-considerations-overview.md)
