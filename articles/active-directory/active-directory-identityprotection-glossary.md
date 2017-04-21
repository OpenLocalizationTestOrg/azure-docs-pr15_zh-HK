<properties
    pageTitle="Azure 的 Active Directory 身份保护术语表 |Microsoft Azure"
    description="Azure 的 Active Directory 身份保护术语表"
    services="active-directory"
    keywords="azure 的活动目录身份保护，云应用程序发现，管理应用程序、 安全性、 风险、 风险程度、 漏洞、 安全策略、 词汇表"
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
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-identity-protection-glossary"></a>Azure 的 Active Directory 身份保护术语表 


### <a name="at-risk-user"></a>风险 （用户）  
具有一个或多个活动的风险事件的用户。 

### <a name="atypical-sign-in-location"></a>非典型登录的位置   
登录不是典型的特定用户、 类似用户或组织的地理位置。

### <a name="azure-ad-identity-protection"></a>Azure AD 身份保护    
提供了一个整合的视图到风险事件和潜在的安全漏洞影响组织的标识的 Azure Active Directory 安全模块。

### <a name="conditional-access"></a>条件接收  
为保护资源的访问策略。 条件访问规则存储在 Azure Active Directory 和 Azure 广告之前授予访问资源的评估。  规则示例包括限制访问权限的基于用户的位置，设备运行状况或用户身份验证方法。

### <a name="credentials"></a>凭据     
信息，包括身份和用于获取访问本地和网络资源的身份证。 凭据的示例包括用户名和密码、 智能卡和证书。

### <a name="event"></a>事件   
在 Azure Active Directory 中的活动记录。

### <a name="false-positive-risk-event"></a>误报 （风险事件） 
风险事件状态来表示风险事件的调查，被错误地标记为风险事件标识保护用户手动设置。

### <a name="identity"></a>标识    
个人或实体必须根据条件，例如密码或证书的身份验证通过验证。

### <a name="identity-risk-event"></a>标识风险事件     
AAD 身份保护，标记为异常，可能表明身份已遭到破坏的事件。

### <a name="ignored-risk-event"></a>忽略 （风险事件）    
身份保护，提示用户不采取补救措施的情况下被关闭，风险事件来手动设置风险事件状态。

### <a name="impossible-travel-from-atypical-locations"></a>来自非典型的位置不可能旅行   
风险事件触发时检测两个登录同一用户，其中至少一个是来自非典型符号中的位置，且号接之间的时间长度小于物理两地间旅行所花费的最短时间。  

###<a name="investigation"></a>调查    
审阅活动、 日志和其他相关的信息与相关的风险事件以确定是否有必要，补救或减轻步骤的过程理解如果和如何标识遭到破坏的情况，并了解如何使用已泄露的身份。

### <a name="leaked-credentials"></a>泄漏的凭据  

风险事件触发时发布公开在深色的网站通过我们的研究人员找到当前用户凭据 （用户名和密码）。

### <a name="mitigation"></a>缓解措施  
要限制或消除攻击者能够利用泄露的身份或设备，无需恢复到安全状态的标识或设备的操作。 缓解措施不能解决以前标识或设备相关联的风险事件。

### <a name="multi-factor-authentication"></a>多因素身份验证 
身份验证方法要求用户的两个或多个身份验证方法，其中可能包括的内容有，这样的证书;用户知道的如用户名、 密码或密码短语;物理属性，如指纹;和个人的特性，如个人签名。

### <a name="offline-detection"></a>离线检测   
检测到的异常情况和事件，如登录尝试的风险评估后的事实，已经发生的事件。

### <a name="policy-condition"></a>策略条件    
安全策略定义的实体 （组、 用户、 应用程序、 设备平台、 设备状态、 IP 地址范围、 客户端类型） 的策略中包含或排除它的一部分。

### <a name="policy-rule"></a>策略规则     
描述将触发策略，并且策略被触发时所执行的操作的情况下的安全策略的一部分。

### <a name="prevention"></a>预防措施  
操作以防止损坏的标识或设备通过滥用组织怀疑或知道受到破坏。 预防行动无法保护设备或标识，并不能解决以前的风险事件。

### <a name="privileged-user"></a>特权 （用户）
它的风险事件，同时在 Azure Active Directory，如全局管理员，有一个或多个资源的永久或临时管理权限用户记帐管理员、 服务管理员、 用户管理员和管理员密码。 

###<a name="real-time"></a>实时    
请参见实时检测。

###<a name="real-time-detection"></a>实时检测  
检测到的异常情况和事件，如登录尝试之前允许事件进行的风险评估。

### <a name="remediated-risk-event"></a>修正 （风险事件）     
由身份信息保护，表明风险事件已修正使用这种类型的风险事件的标准修正操作自动设置风险事件状态。 例如，重置用户密码，表明以前的密码被破坏的许多风险事件是自动修正。

### <a name="remediation"></a>补救措施     
安全标识或以前怀疑或已知受到破坏的设备操作。 补救行动将标识或设备恢复到安全状态中，并解决以前标识或设备相关联的风险事件。

### <a name="resolved-risk-event"></a>解决 （风险事件）   
关闭标识保护用户手动设置风险事件状态的情况下，指示用户利用外部身份保护，适当的补救行动，并应考虑风险事件。

###<a name="risk-event-status"></a>风险事件状态    
风险事件的属性，指示事件是否处于活动状态，以及关闭，关闭的原因。

###<a name="risk-event-type"></a>风险事件类型  
指示导致事件被认为是风险较大的异常类型的风险事件类别。

###<a name="risk-level-risk-event"></a>风险等级 （风险事件）  
他们需要减少组织的风险以帮助标识保护用户设置优先级的操作风险事件的严重程度的指示 （高、 中或低）。 

###<a name="risk-level-sign-in"></a>风险等级 （登录） 
指示 （高、 中或低），针对特定登录的其他人试图使用该用户的标识的可能性。

###<a name="risk-level-user-compromise"></a>风险级别 （用户泄露时） 
指示 （高、 中或低） 标识已遭到破坏的可能性。

### <a name="risk-level-vulnerability"></a>风险等级 （漏洞）  
他们需要减少组织的风险指示 （高、 中或低） 来帮助确定操作的优先级身份保护用户安全漏洞的严重程度。

### <a name="secure-identity"></a>保护 （身份）   
采取补救操作，例如更改密码或机器映像来还原为不折不扣的状态可能会泄露的身份。

### <a name="security-policy"></a>安全策略
策略规则和条件的集合。 一个策略可以应用于实体如用户、 组、 应用程序、 设备、 设备平台、 设备状态、 IP 地址范围和 Auth2.0 的客户端类型。 启用了策略后，计算时的策略中包含的实体颁发的资源的标记。

### <a name="sign-in-v"></a>(V) 在每次登录 
要到 Azure Active Directory 中标识进行身份验证。

### <a name="sign-in-n"></a>登录 (n) 
进程或操作标识在 Azure Active Directory 和捕获此操作的事件中进行身份验证。

###<a name="sign-in-from-anonymous-ip-address"></a>登录的匿名 IP 地址    
风险事件触发后成功的登录已被标识为匿名代理服务器 IP 地址的 IP 地址。

###<a name="sign-in-from-infected-device"></a>签入从受感染的设备 
风险事件触发时签到源于这次将由一个或多个设备损坏，积极尝试与 bot 服务器通信的 IP 地址。

###<a name="sign-in-from-ip-address-with-suspicious-activity"></a>签入发件人 IP 地址与可疑活动 
风险事件触发后成功的登录 IP 地址与从大量的失败的登录尝试跨多个用户帐户短的一段时间。

###<a name="sign-in-from-unfamiliar-location"></a>登录不熟悉的位置从 
风险事件，当用户成功登录从新位置 （IP、 纬度/经度和 ASN） 时触发。

###<a name="sign-in-risk"></a>登录的风险     
请参阅风险级别 （登录）

###<a name="sign-in-risk-policy"></a>登录以风险策略  
计算特定符号中的风险，并应用缓解根据预定义的条件和规则的条件访问策略。

###<a name="user-compromise-risk"></a>用户泄露风险     
请参阅风险级别 （用户泄露时）

###<a name="user-risk"></a>用户风险    
请参阅风险级别 （用户泄露时）。

###<a name="user-risk-policy"></a>用户风险策略     
条件接收策略，认为签入并应用基于预定义的条件和规则的缓解措施。

###<a name="users-flagged-for-risk"></a>用户标记为风险   
有哪些活动或补救风险事件的用户

###<a name="vulnerability"></a>漏洞    
配置或条件在 Azure Active Directory 这使得目录易受攻击或威胁。


## <a name="see-also"></a>请参见

- [Azure 的 Active Directory 身份保护](active-directory-identityprotection.md)
