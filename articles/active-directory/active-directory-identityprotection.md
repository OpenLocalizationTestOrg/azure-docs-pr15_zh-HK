<properties
    pageTitle="Azure 的 Active Directory 身份保护 |Microsoft Azure"
    description="了解如何 Azure AD 身份信息保护使您能够限制攻击者的能力，利用破坏的身份或设备和安全标识或以前怀疑或已知受到破坏的设备。"
    services="active-directory"
    keywords="azure 的活动目录身份保护，云应用程序发现，管理应用程序、 安全性、 风险、 风险程度、 漏洞、 安全策略"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection"></a>Azure 的 Active Directory 身份保护 

Azure 的活动目录标识保护是风险事件和潜在的安全漏洞影响您的组织的身份到一个整合的视图为您提供的 Azure AD 特优 P2 版的功能。 Microsoft 已被保护的基于云的标识过去的十年，并使用 Azure AD 身份保护，Microsoft 将这些相同的保护系统推出的企业客户。 身份保护利用现有 Azure 广告的异常检测能力 （可通过 Azure 广告的反常活动报告），并引入了新的风险事件类型，可以实时检测异常。



##<a name="getting-started"></a>入门教程

当攻击者通过窃取用户的身份获得访问环境，绝大多数存在安全隐患的发生。 攻击者已经变得越来越有效地利用第三方破坏和使用复杂网络钓鱼攻击。 一旦攻击者获得访问甚至低特权的用户帐户，它是相对简单地访问重要的公司资源通过横向移动。 因此有必要以保护所有标识，并在身份被泄露，主动防止泄露的身份被滥用。 

发现被破坏的身份并不容易。 幸运的是，可以帮助标识保护︰ 标识保护使用自适应的机器学习算法和启发式技术来检测异常和风险可能表明身份已遭到破坏的事件。
 
使用此数据，身份保护生成报告和警报，您可以调查这些风险事件，并采取适当的补救或缓解措施。
 
但 Azure 活动目录标识保护了多个监控和报告工具。 基于风险事件，身份保护计算用户风险级别为每个用户，使您能够配置基于风险的策略，以自动保护您的组织的身份。  这些基于风险的策略，除了其他 Azure Active Directory 和 EMS，所提供的条件访问控制可以自动阻止或提供了自适应修正措施，包括密码重置和多因素身份验证的实施。  

####<a name="explore-identity-protections-capabilities"></a>身份保护功能研究 

**检测风险事件和风险的帐户︰**  

- 使用机器学习和启发式规则的检测 6 风险事件类型 

- 计算用户风险级别

- 提供自定义的建议，以提高整体的安全状态并予以突出显示安全漏洞



**研究风险事件︰**

- 发送有关风险事件的通知

- 研究使用相关的上下文信息的风险事件

- 提供基本的工作流来跟踪调查

- 提供便捷地补救操作，例如重新设置密码



**基于风险的条件访问策略︰**

- 通过阻止登录或要求多因素身份验证质询减轻危险的登录策略。

- 块或安全风险较大的用户帐户策略

- 若要要求用户注册的多因素身份验证的策略


## <a name="detection-and-risk"></a>检测和风险

### <a name="risk-events"></a>风险事件

风险事件被标记为可疑身份保护的事件并指示标识可能被破坏。 风险事件的完整列表，请参见[类型的检测到的 Azure 活动目录身份保护的风险事件](active-directory-identityprotection-risk-events-types.md)。 


### <a name="risk-level"></a>风险级别

风险事件的风险级别是严重性的指示 （高、 中或低） 的风险事件。 风险等级仅用于帮助标识保护用户确定他们必须采取的措施来减少组织的风险的优先级。 风险事件的严重程度表示信号强度作为标识被破坏，加上它通常会带来的噪音量预测值。 

- **高**︰ 高可靠性和高严重性风险事件。 这些事件是强指示器，用户的身份被泄露，应立即修正影响的任何用户帐户。

- **媒体**︰ 严重性等级为高，但较低的信心风险事件，反之亦然。 这些事件都可能存在风险，并应修正影响的任何用户帐户。

- **低**︰ 较低的信心和低严重性风险事件。 此事件可能不需要立即采取操作，但再加上其他风险事件，可能会提供明显表示身份遭到破坏。 


![风险级别](./media/active-directory-identityprotection/01.png "风险级别")

 

在既标识风险事件**实时**，或在后续处理已发生风险事件后置入 （离线）。 目前身份信息保护中的大多数风险事件进行离线计算，2 4 小时内示身份保护。 进行计算时在实时，实时风险事件将出现在标识保护控制台在 5-10 分钟内。

多个旧式客户端当前不支持实时风险事件检测和预防。 因此，登录从这些客户端无法检测到或实时阻止。


## <a name="investigation"></a>调查
通过标识保护您旅程通常从身份保护仪表板开始。 

![补救措施](./media/active-directory-identityprotection/1000.png "补救措施")

仪表板使您可以访问︰
 
- 如**用户标记为风险**、**风险事件**和**漏洞**报告
- 如您的**安全策略**、**通知**和**多因素身份验证注册**的配置设置
 

它通常是您的起始点进行调查，这是审阅活动、 日志和其他相关的信息与相关的风险事件来决定是否需要进行补救或减轻步骤的过程，以及如何标识遭到破坏的情况，并了解如何使用已泄露的身份。

您可以将您 Azure 的活动目录保护每个电子邮件发送的[通知](active-directory-identityprotection-notifications.md)的调查活动。

以下各节将为您提供更多详细信息，并与调查相关的步骤。  



## <a name="what-is-a-user-risk-level"></a>用户风险级别是什么？

用户风险等级并不表示 （高、 中或低） 的用户的标识已遭到破坏的可能性。 它是基于计算与用户身份相关联的用户的风险事件。 

风险事件的状态为**活动**或**已关闭**。 只有在处于**活动状态**的风险事件导致用户风险的计算。 

使用以下输入计算用户风险级别︰

- 活动风险事件影响用户
- 这些事件的风险等级 
- 是否已采取任何补救措施 

![用户风险](./media/active-directory-identityprotection/1001.png "用户风险")



可以使用用户的风险级别来创建条件的访问策略，以阻止危险用户登录，或强制他们能够安全地更改他们的密码。 


## <a name="closing-risk-events-manually"></a>手动关闭风险事件

在大多数情况下，您将采取补救操作，例如一个安全的密码重置为自动关闭风险事件。 然而，这总是不可行。  
这是，例如，这种情况，当︰

- 具有活动的风险事件的用户已被删除
- 调查显示合法用户已执行报告的风险事件

由于风险事件的**活动**分配到用户风险的计算，您可能需要手动通过手动关闭风险事件降低了风险等级。  
调查期间，可以选择执行下列任一操作来更改风险事件的状态︰

![操作](./media/active-directory-identityprotection/34.png "操作")

- **解决**-如果在研究后风险事件，拍摄外身份保护，适当的补救行动，您认为应考虑风险事件关闭，将该事件标记为已解决。 解决事件将设置为已关闭的风险事件的状态和风险事件将不再导致用户的风险。

- **标记为误报**-在某些情况下，可能风险事件的调查，并发现它被错误地标记为一种冒险。 您可以帮助减少这种次数将标记为误报的风险事件。 这将有助于机器学习算法，以提高分类的类似事件将来。 误报事件的状态设置为**已关闭**，他们不再将导致用户的风险。

- **忽略**-如果不采取任何补救措施，但想要从活动列表中移除的风险事件，您可以标记忽略风险事件和事件状态都将被关闭。 被忽略的事件并不参与用户的风险。 此选项只应在特殊情况下。 

- **重新激活**-手动关闭 （通过选择**解析**、**假阳性**或**忽略**） 的风险事件可以被重新激活，将事件状态设置为**活动状态**。 重新激活的风险事件导致用户风险级别计算。 不能重新激活 （例如，一个安全的密码重置） 通过修正已关闭的风险事件。 




**若要打开相关的配置对话框**︰

1. 在**Azure AD 身份保护**刀片式服务器，**调查**下的**风险事件**。

    ![手动密码重置](./media/active-directory-identityprotection/1002.png "手动密码重置")

2. 在**风险事件**列表中，单击风险。

    ![手动密码重置](./media/active-directory-identityprotection/1003.png "手动密码重置")

2. 在风险刀片式服务器，右键单击用户。

    ![手动密码重置](./media/active-directory-identityprotection/1004.png "手动密码重置")



### <a name="closing-all-risk-events-for-a-user-manually"></a>手动关闭用户的所有风险事件

而不是手动逐个关闭用户的风险事件，Azure 活动目录标识保护还提供了一个方法来关闭用户通过一次单击所有事件。


![操作](./media/active-directory-identityprotection/2222.png "操作")

单击**清除所有事件**时，所有事件都已都关闭，受影响的用户不再处于危险。



## <a name="remediating-user-risk-events"></a>修正用户风险事件

补救措施是要确保安全标识或以前怀疑或已知受到破坏的设备操作。 补救行动将标识或设备恢复到安全状态中，并解决以前标识或设备相关联的风险事件。

要修正用户风险事件，您可以︰

- 执行一个安全的密码重置手动修正用户风险事件 

- 配置用户风险安全策略，以缓解或自动纠正用户风险事件

- 重新映像感染威胁的设备  


### <a name="manual-secure-password-reset"></a>手动安全密码重置

安全的密码重置是对于很多风险事件，有效补救措施，在执行时，自动关闭这些风险事件并重新计算用户的风险级别。 身份保护仪表板可用于启动危险用户重置密码。 

相关的对话框提供了两种不同的方法来重新设置密码︰

**重置密码**的选择**要求用户重置密码**允许用户自我恢复如果用户已注册的多因素身份验证。 在用户下一次登录，将需要成功地解决多因素身份验证质询，然后，被迫更改密码用户。 如果用户帐户不是已注册的多因素身份验证，则该选项不可用。

**临时密码**-选择**生成一个临时密码**，立即使现有的密码，并创建一个新的临时密码的用户。 将新的临时密码发送到备选电子邮件地址的用户或用户的经理。 因为密码是临时的则将提示用户更改登录密码。


![策略](./media/active-directory-identityprotection/1005.png "策略")


**若要打开相关的配置对话框**︰

1. 在**Azure AD 身份保护**刀片式服务器，请单击**用户标记为风险**。

    ![手动密码重置](./media/active-directory-identityprotection/1006.png "手动密码重置")


2. 从用户列表中，选择具有至少一个风险事件的用户。

    ![手动密码重置](./media/active-directory-identityprotection/1007.png "手动密码重置")


2. 在用户刀片式服务器，单击**重设密码**。

    ![手动密码重置](./media/active-directory-identityprotection/1008.png "手动密码重置")





## <a name="user-risk-security-policy"></a>用户风险安全策略

用户风险安全策略是评估对特定用户的风险级别并将根据预定义的条件和规则的修正和缓解操作应用的条件访问策略。


![用户 ridk 策略](./media/active-directory-identityprotection/1009.png "用户 ridk 策略")


Azure AD 身份保护可帮助您管理减轻和补救措施的通过使您能够为风险标记的用户︰

- 设置用户和组策略应用于︰ 

    ![用户 ridk 策略](./media/active-directory-identityprotection/1010.png "用户 ridk 策略")


- 设置用户风险级别阈值 （低、 中或高） 触发该策略︰ 

    ![用户 ridk 策略](./media/active-directory-identityprotection/1011.png "用户 ridk 策略")


- 设置策略会触发时要执行的控件︰

    ![用户 ridk 策略](./media/active-directory-identityprotection/1012.png "用户 ridk 策略")


- 切换您的策略的状态︰

    ![用户 ridk 策略](./media/active-directory-identityprotection/403.png "MFA 注册")


- 审查和评估之前激活该更改的影响︰

    ![用户 ridk 策略](./media/active-directory-identityprotection/1013.png "用户 ridk 策略")


选择**高**阈值减少策略触发并对用户的影响减到最小次数。
但是，它不包括**低**和**中等**用户标记为从策略，可能不安全的身份或设备以前怀疑或已知受到破坏的风险。

当设置该策略，

- 排除用户都有可能产生大量的假阳性 （开发人员、 安全分析师）

- 排除在其中启用策略是不可行的区域设置中的用户 （如帮助台无访问权限）

- **高**阈值在初始策略的过程中推广的使用，或者如果必须最小化最终用户所看到的挑战。

- 如果您的组织需要更高的安全性，请使用**低**阈值。 选择**低**阈值引入了附加的用户登录的挑战，但更高的安全性。

对于大多数组织建议的默认设置是配置可用性与安全性之间的平衡**中**阈值的规则。

相关的用户体验的概述，请参阅︰

- [Compromised 帐户恢复流动](active-directory-identityprotection-flows.md#compromised-account-recovery)。  

- [Compromised 帐户阻止流动](active-directory-identityprotection-flows.md#compromised-account-blocked)。  


**若要打开相关的配置对话框**︰

1. 在**Azure AD 身份保护**刀片式服务器，在**配置**部分中，单击**用户风险策略**。

    ![用户 ridk 策略](./media/active-directory-identityprotection/1009.png "用户 ridk 策略")






## <a name="mitigating-user-risk-events"></a>降低用户风险事件
管理员可以设置登录根据风险级别时阻止用户到用户风险安全策略。 

阻止登录︰
 
- 可防止新用户风险事件，受影响用户的生成

- 使管理员可以手动修正风险事件影响用户的身份并将其恢复到安全状态



## <a name="what-is-a-sign-in-risk-level"></a>登录的风险级别是什么？

登录的风险级别是可能性的指示 （高、 中或低），针对特定登录的其他人试图使用该用户的标识进行身份验证。 签到风险级别在登录的时间计算的并认为风险事件和标记检测中实时为该特定登录的。 

## <a name="mitigating-sign-in-risk-events"></a>缓解风险登录事件 
一项缓解措施是限制攻击者能够利用泄露的身份或设备，无需恢复到安全状态的标识或设备的操作。 缓解措施不能解决以前标识或设备相关联的登录的风险事件。

可以使用在 Azure AD 身份保护条件访问自动缓解登录的风险事件。 使用这些策略，请考虑用户或登录来阻止危险号接或要求用户进行多因素身份验证的风险级别。 这些操作可能会阻止攻击者利用窃取的身份严重的损害，并可能会给出一些时间来确保安全标识。 


## <a name="sign-in-risk-security-policy"></a>签入风险的安全策略

登录以风险策略是评估特定的签入的风险并将缓解根据预定义的条件和规则应用的条件访问策略。

![登录风险策略](./media/active-directory-identityprotection/1014.png "登录风险策略")


Azure AD 身份保护，有助于通过使您能够管理缓解危险的登录︰

- 设置用户和组策略应用于︰ 

    ![登录风险策略](./media/active-directory-identityprotection/1015.png "登录风险策略")


- 设置登录风险级别阈值 （低、 中或高） 触发该策略︰ 

    ![登录风险策略](./media/active-directory-identityprotection/1016.png "登录风险策略")


- 设置策略会触发时要执行的控件︰  

    ![登录风险策略](./media/active-directory-identityprotection/1017.png "登录风险策略")
    

- 切换您的策略的状态︰

    ![MFA 注册](./media/active-directory-identityprotection/403.png "MFA 注册")

- 审查和评估之前激活该更改的影响︰ 

    ![登录风险策略](./media/active-directory-identityprotection/1018.png "登录风险策略")


### <a name="what-you-need-to-know"></a>您需要知道

您可以配置登录风险安全策略要求进行多因素身份验证︰

![登录风险策略](./media/active-directory-identityprotection/1017.png "登录风险策略")

但是，出于安全考虑，此设置仅适用于已注册的多因素身份验证的用户。 如果尚未登记的多因素身份验证的用户，则满足条件要求进行多因素身份验证，用户将被阻止。 

作为最佳实践，您需要多因素身份验证对于危险号单元，您应该︰

1. 使受影响的用户的[多因素身份验证注册策略](#multi-factor-authentication-registration-policy)。
2. 在非危险会话中执行的 MFA 注册需要登录到受影响的用户

完成这些步骤，可确保多因素身份验证是必需为危险的符号。 


### <a name="best-practices"></a>最佳做法

 
选择**高**阈值减少策略触发并对用户的影响减到最小次数。  
 
但是，它不包括**低**和**中等**号单元标记为风险的策略，不可能阻止攻击利用已泄露的身份。 

当设置该策略， 

- 排除不能 / 不能具有多因素身份验证的用户

- 排除在其中启用策略是不可行的区域设置中的用户 （如帮助台无访问权限）

- 排除用户都有可能产生大量的假阳性 （开发人员、 安全分析师）

- **高**阈值在初始策略的过程中推广的使用，或者如果必须最小化最终用户所看到的挑战。

- 如果您的组织需要更高的安全性，请使用**低**阈值。 选择**低**阈值引入了附加的用户登录的挑战，但更高的安全性。

对于大多数组织建议的默认设置是配置可用性与安全性之间的平衡**中**阈值的规则。

 
登录以风险策略是︰

- 应用于所有的浏览器通信和使用现代的身份验证登录。
- 不应用于使用旧安全协议通过禁用在联合 IDP，如 ADFS 的 WS 信任终结点的应用程序。

身份保护控制台中的**风险事件**页面列出所有事件︰

- 此策略应用到
- 您可以查看活动和确定操作是否合适 

相关的用户体验的概述，请参阅︰

- [危险的符号中恢复](active-directory-identityprotection-flows.md#risky-sign-in-recovery) 

- [危险登录被阻止](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  

- [在危险符号中的多因素身份验证注册](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)  





**若要打开相关的配置对话框**︰

1. 在**Azure AD 身份保护**刀片式服务器，在**配置**部分中，单击**签入风险策略**。

    ![用户 ridk 策略](./media/active-directory-identityprotection/1014.png "用户 ridk 策略")





## <a name="multi-factor-authentication-registration-policy"></a>多因素身份验证注册策略

Azure 的多因素身份验证是验证您的身份的一种方法，需要使用多个用户名和密码。 提供第二层的用户登录和交易的安全。  
我们建议需要 Azure 的多因素身份验证的用户登录，因为它︰

- 提供了一系列简单的验证选项与强身份验证

- 准备组织保护和恢复从帐户做出妥协的过程中的关键作用

![用户 ridk 策略](./media/active-directory-identityprotection/1019.png "用户 ridk 策略")



有关详细信息，请参阅[Azure 多因素身份验证是什么？](../multi-factor-authentication/multi-factor-authentication.md)


Azure AD 身份保护可帮助您通过配置的策略，使您能够管理推出的多因素身份验证注册︰ 



- 设置用户和组策略应用于︰ 

    ![MFA 策略](./media/active-directory-identityprotection/1020.png "MFA 策略")



- 设置策略会触发时要执行的控件︰  

    ![MFA 策略](./media/active-directory-identityprotection/1021.png "MFA 策略")


- 切换您的策略的状态︰

    ![MFA 策略](./media/active-directory-identityprotection/403.png "MFA 策略")

- 查看当前的注册状态︰ 

    ![MFA 策略](./media/active-directory-identityprotection/1022.png "MFA 策略")


相关的用户体验的概述，请参阅︰

- [多因素身份验证注册流程](active-directory-identityprotection-flows.md#multi-factor-authentication-registration)。  

- [多因素身份验证期间危险符号中的注册](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)。  





**若要打开相关的配置对话框**︰

1. 在**Azure AD 身份保护**刀片式服务器，在**配置**部分中，单击**多因素身份验证注册**。

    ![MFA 策略](./media/active-directory-identityprotection/1019.png "MFA 策略")





## <a name="next-steps"></a>下一步行动

 - [频道 9: Azure AD 和标识显示︰ 标识保护预览](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
 - [检测到的 Azure 活动目录身份保护的风险事件的类型](active-directory-identityprotection-risk-events-types.md)
 - [通过 Azure 活动目录标识保护检测到的漏洞](active-directory-identityprotection-vulnerabilities.md)
 - [Azure 的活动目录标识保护通知](active-directory-identityprotection-notifications.md)
 - [Azure 的活动目录标识保护行动手册](active-directory-identityprotection-playbook.md)
 - [Azure 的活动目录标识保护术语表](active-directory-identityprotection-glossary.md)

 - [Azure AD 身份保护登录体验](active-directory-identityprotection-flows.md)

 - [启用 Azure Active Directory 身份保护](active-directory-identityprotection-enable.md)
 - [活动目录标识保护 azure-如何解锁用户](active-directory-identityprotection-unblock-howto.md)

 - [开始使用 Azure 活动目录身份保护和 Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)


