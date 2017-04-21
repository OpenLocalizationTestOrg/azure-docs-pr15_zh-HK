<properties
    pageTitle="Azure Active Directory 混合标识设计考虑事项-确定混合身份管理任务 |Microsoft Azure"
    description="有条件的访问控制，Azure Active Directory 检查挑选进行用户身份验证时，才允许访问该应用程序的特定条件。 一旦满足了这些条件，是经过身份验证的用户，并允许应用程序访问。"
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

# <a name="plan-for-hybrid-identity-lifecycle"></a>对于混合身份生命周期的计划 

标识是一个企业的移动性和应用程序访问策略的基础。 是否要签署的您的移动设备或 SaaS 应用程序，您的身份信息是接触到的一切的关键。 在其最高级别上，标识管理解决方案包括统一和您的身份存储库包括自动化和集中化的资源调配资源的进程之间的同步。 标识解决方案应跨内部和云是一个集中式的身份，还使用某种形式的联合身份验证维护集中的身份验证和安全地共享和与外部用户和企业合作。 介于操作系统和应用程序中，人的资源或加入，组织。 可以改变组织结构，以适应资源调配策略和过程。

还有一点需要具有标识解决方案，旨在让您的用户通过向客户提供自助服务的体验，以使它们保持工作效率。 标识解决方案是更可靠的级别如果它允许单一登录的用户通过他们需要在所有访问管理员的所有资源可以用于管理用户凭据使用标准化的过程。 某些级别的管理可以减少或消除了，具体取决于配置管理解决方案的广度。 此外，您可以安全地分发管理功能，手动或自动，在各种组织之间。 例如，域管理员可以提供的人员和资源域中。 该用户可以执行的管理和资源调配任务，但未被授权执行配置任务，例如创建工作流。


## <a name="determine-hybrid-identity-management-tasks"></a>确定混合身份管理任务
分发您的组织中的管理任务可以提高准确性和有效性的管理并提高组织的工作负荷的平衡。 下面是定义一个可靠的身份管理系统的透视。

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)


若要定义混合身份管理任务，您必须了解采用混合身份组织的一些基本特征。 请务必了解用于标识源的当前存储库。 了解这些核心元素，您将具有的基本要求，并根据需要提出更细致的问题，都会导致标识解决方案更好的设计决策。  

定义这些需求，同时确保至少回答以下问题

- 资源调配选项︰ 
 - 一个功能强大的客户访问管理和资源调配系统是否支持混合身份解决方案？
 - 如何将用户、 组和密码管理？
 - 可以标识生命周期管理作出响应？ 
      - 密码更新帐户暂停会有多长时间？
      
- 许可证管理︰ 
 - 混合身份解决方案处理许可证管理吗？
     - 如果是，提供了哪些功能？
- 该解决方案是否处理基于组许可证的管理？ 
      — 如果是，是否可能为它分配的安全组？ 
       — 如果是，将云目录自动分配许可证给组的所有成员？ 
        -如果用户随后添加，或从组中删除，将许可证自动分配或删除相应地会发生什么？ 

- 与其他第三方身份提供程序集成︰
- 可此混合解决方案集成在一起以实现单一登录的第三方身份提供程序？
- 是否可以统一到一个有凝聚力的标识系统的所有不同的标识提供程序？
- 如果是的话，如何和他们以及可以使用哪些功能？

## <a name="synchronization-management"></a>同步管理
同步标识管理器，以便能够使所有标识提供程序，并使其目标之一。 保持数据同步基于权威主标识提供程序。 在混合标识方案，与同步的管理模型，可以将管理本地服务器中的所有用户和设备身份和同步帐户和 （可选） 密码的云。 用户输入同一个密码在内部作为他或就在云中，并在登录、 密码验证标识解决方案。 此模型使用目录同步工具。
 
![](./media/hybrid-id-design-considerations/Directory_synchronization.png)正确设计的混合标识解决方案的同步确保回答以下问题: • 同步解决方案可用于混合身份解决方案是什么？
• 单一签入功能是什么？
• 为 B2B 和 B2C 之间的联合身份验证的选项是什么？

## <a name="next-steps"></a>下一步行动
[确定混合身份管理采用策略](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)


## <a name="see-also"></a>请参见
[设计考虑事项概述](active-directory-hybrid-identity-design-considerations-overview.md)

