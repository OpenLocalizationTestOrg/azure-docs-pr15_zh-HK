<properties
    pageTitle="Azure 的 Active Directory 条件访问常见问题解答 |Microsoft Azure"
    description="关于条件访问的常见问题 "
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-faq"></a>Azure 的 Active Directory 条件访问常见问题解答

## <a name="which-applications-work-with-conditional-access-policies"></a>哪些应用程序处理的条件访问策略？

**A:**请参阅主题，[支持条件模拟访问应用程序](active-directory-conditional-access-supported-apps.md)。

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>为实施 B2B 协作和来宾用户的条件访问策略？

**A:**对 B2B 协作用户强制策略。 但是，在某些情况下，用户可能不能满足策略要求，如果，例如，组织中不支持多因素身份验证。 

不为 SharePoint 来宾用户当前执行的策略。 访客关系是在 SharePoint 中进行维护的。 来宾用户帐户不会受到访问策略在身份验证服务器。 可以在 SharePoint 管理来宾访问权限。

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>SharePoint Online 策略也适用于为业务的 OneDrive 吗？

**A:**是的。
 
## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>为什么不能在客户端应用程序，如 Word 或 Outlook 设置策略？

**A:**条件访问策略设置用于访问服务的要求，并在身份验证对该服务执行的操作时强制。 直接对客户端应用程序; 未设置策略相反，它被应用到服务调用时。 例如，在 SharePoint 中设置的策略将应用于客户端调用 SharePoint 和 Exchange 上设置的策略将应用于 Outlook。


## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>条件访问策略不适用于服务帐户？

**A:**条件的访问策略将应用于所有用户帐户。 这包括用户帐户用作服务帐户。 在许多情况下，不能满足策略运行无人参与服务帐户。 此情况，例如，需要 MFA 时。 在这些情况下，可以从策略、 使用条件访问策略管理设置中排除服务帐户。 了解有关将策略应用于此用户。
