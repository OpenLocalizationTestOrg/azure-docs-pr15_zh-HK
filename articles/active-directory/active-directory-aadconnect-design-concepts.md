<properties
   pageTitle="Azure AD 连接︰ 设计概念 |Microsoft Azure"
   description="本主题详细介绍特定实现设计区域"
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.custom = "azure-ad-connect"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="09/13/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-design-concepts"></a>Azure AD 连接︰ 设计概念
本主题的目的是为了说明必须 Azure AD 连接的实现设计过程中考虑的领域。 本主题是深入查看某些区域上的，这些概念进行了简要描述以及其他主题。

## <a name="sourceanchor"></a>sourceAnchor
SourceAnchor 属性被定义为*不可变的对象的生存期内的特性*。 它唯一地标识对象为同一对象内部和 Azure 的广告中。 属性也被称为**immutableId** ，可互换使用的两个名称。

一词不可变的是"不能更改"，对此主题很重要。 由于设置了后不能更改此属性的值，务必要选择一种支持方案的设计。

在下列情况下使用该特性︰

- 时新的同步引擎服务器生成，或重建后灾难恢复方案中，此属性将链接在 Azure AD 对象内部使用现有对象。
- 如果您移动到同步的标识模型从云专用标识，然后此特性允许对象与"硬匹配"现有对象在 Azure AD 具有内部的对象。
- 如果使用联合身份验证，然后**范围内**与该属性用于声明中唯一地标识用户。

本主题只谈到 sourceAnchor 与用户相关。 相同的规则应用于所有对象类型，但它仅对用户是此问题通常是个问题。

### <a name="selecting-a-good-sourceanchor-attribute"></a>选择一个好的 sourceAnchor 特性
属性值必须遵循下列规则︰

- 将长度少于 60 个字符
    - 编码，计为 3 个字符字符不是 a 到 z、 A 到 Z 或 0-9
- 不能包含特殊字符︰ & #92; ！ # $ % & * + / = ? ^ & #96;{ } |~ < > （);: , [ ] " @ _
- 必须是全局唯一的
- 必须是字符串、 整数或二进制文件
- 不基于用户的名称，这些更改
- 应该不区分大小写并避免可能因大小写的值
- 应在创建对象时分配

如果选定的 sourceAnchor 不是字符串类型，则 Azure AD 连接 Base64Encode 属性值，以确保没有特殊字符出现。 如果您使用 ADFS 比另一个联合服务器，请确保服务器还可以 Base64Encode 属性。

SourceAnchor 属性是区分大小写。 值为"JohnDoe"不是"johndoe"相同。 但不是应具有两个不同对象仅在大小写差异。

如果您采用单目录林内部，然后应使用的属性是**objectGUID**。 这也是在 Azure AD 连接中使用快速设置时使用的属性以及使用的目录同步的特性。

如果您有多个林并不林和域之间移动用户，则**objectGUID**是使用甚至在这种情况下的良好特性。

如果目录林和域之间移动用户，然后必须找到一个属性，它不会更改，也可以移动用户移动的过程。 建议的方法是引入综合特性。 可能包含某些内容看起来像一个 GUID 将是合适的属性。 在对象创建过程中一个新的 GUID 创建和用户标记。 在同步引擎服务器中，可以创建基于**objectGUID**此值和更新中添加选定的属性，可以创建自定义同步规则。 当您移动对象时，请确保复制此值的内容。

另一个解决方案是挑选您知道不会更改现有属性。 常用的属性包括**雇员 id**。 如果您考虑属性包含字母，请确保没有该特性的值可以更改大小写 （大写和小写） 没有机会。 不应使用的坏属性包括这些属性使用的用户的名称。 在结婚或离婚，名称应更改，这不允许为此属性。 这也是为什么等**范围内**、**邮件**和**targetAddress**属性不是甚至可以在 Azure AD 连接安装向导中选择的一个原因。 这些属性还包含@-character,sourceAnchor 中不允许的。

### <a name="changing-the-sourceanchor-attribute"></a>更改 sourceAnchor 属性
在 Azure AD 中创建了对象并标识同步之后，sourceAnchor 属性值不能更改。

鉴于此，以下限制适用于 Azure AD 连接︰

- 只能在初始安装过程中设置 sourceAnchor 属性。 如果您重新运行安装向导，此选项是只读的。 如果您需要更改此设置，您必须卸载并重新安装。
- 如果您安装了另一个 Azure AD 连接服务器，则必须选择以前使用相同的 sourceAnchor 属性。 如果您之前已使用目录同步，并移动到 Azure AD 连接，则必须使用**objectGUID** ，因为它是由目录同步的特性。
- 如果 sourceAnchor 的值更改后到 Azure AD，然后 Azure AD 连接同步引发一个错误，并且不允许任何更改在回源目录中更改对象之前，此问题已得到解决和 sourceAnchor 已导出对象。

## <a name="azure-ad-sign-in"></a>Azure 广告登录
同时使用 Azure AD 集成您的内部目录，它很重要了解如何同步设置可以影响方式用户进行身份验证。 Azure AD 使用范围内 (UPN) 对用户进行身份验证。 但是，当您同步您的用户，则必须选择要谨慎使用范围内的值的属性。

### <a name="choosing-the-attribute-for-userprincipalname"></a>选择范围内的属性
选择提供的属性时应确保在 Azure 使用 UPN 值

- 属性值符合的 UPN 语法 (RFC 822)，它的格式应为username@domain
- 在值的后缀在 Azure 的广告匹配到一个自定义的验证域

在快速设置属性假定的选择是范围。 范围内属性不包含值如果您希望用户可以登录到 Azure，然后您必须选择**自定义安装**。

### <a name="custom-domain-state-and-upn"></a>自定义的域状态和 UPN
请务必确保已验证的域的 UPN 后缀。

约翰是 contoso.com 内的用户。 要约翰要使用内部部署 UPNjohn@contoso.com之后给您 Azure 的广告目录 contoso.onmicrosoft.com 同步用户登录到 Azure。 若要执行此操作，您需要添加并在 Azure 的广告，然后才可以开始同步用户自定义的域作为验证 contoso.com。 如果 UPN 后缀的约翰，例如 contoso.com，Azure AD 中不符已验证的域，Azure 广告对用 contoso.onmicrosoft.com 替换的 UPN 后缀。

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>内部部署非路由域和 UPN Azure 的广告
某些组织有非可路由的域，如 contoso.local 或简单的单个标签域 contoso 类似。 您将不能在 Azure AD 验证非可路由的域。 Azure AD 连接在 Azure AD 验证域同步。 创建 Azure 的广告目录时，它创建的 Azure 广告为例，contoso.onmicrosoft.com 将成为默认域可路由的域。 因此，有必要以验证任何其他可路由的域，在这种情况下，您不想同步到默认的 onmicrosoft.com 域的情况下。

阅读[您自定义的域将名称添加到 Azure Active Directory](active-directory-add-domain.md)上添加和验证域的详细信息。

如果您在非可路由的域环境中运行，得适当地提醒你继续明确设置检测到 azure AD 连接。 如果只在非可路由的域中运行，它很可能用户的 UPN 太具有不可路由的后缀。 例如，如果您正在运行在 contoso.local 下，Azure AD 连接推荐您使用自定义设置，而不是使用快速设置。 使用自定义设置，您将能够指定应用作 UPN 登录到 Azure 后到 Azure AD 同步用户的属性。

## <a name="next-steps"></a>下一步行动
了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
