<properties
    pageTitle="自定义属性映射 |Microsoft Azure"
    description="了解在 Azure Active Directory 的 SaaS 应用程序的哪些属性映射是如何修改它们来满足您的业务需求。"
    services="active-directory"
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
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="customizing-attribute-mappings"></a>自定义属性映射


Microsoft Azure AD 用户供应到销售队伍，Google 应用程序和其他第三方 SaaS 应用程序提供支持。 如果您有用户提供的第三方的 SaaS 应用程序已启用，Azure 管理门户控制其属性值的配置称为"属性映射"窗体中。

没有预配置的一组间 Azure AD 用户对象和每个 SaaS 应用程序的用户对象的属性映射。 一些应用程序来管理对象组或联系人的其他类型。 <br> 
根据您的业务需要，您可以自定义默认属性映射。 这意味着，您可以更改或删除现有属性映射或创建新的属性映射。

在 Azure 广告门户中，可以通过在 SaaS 应用程序的工具栏中单击属性来访问此功能。

> [AZURE.NOTE] **属性**链接才可用，如果您有用户提供 SaaS 应用程序启用。 


![销售队伍][1] 


当工具栏，当前对于 SaaS 应用程序配置的映射的列表中单击属性。

下面的屏幕快照演示此示例︰



![销售队伍][2]  


在上面的示例中，您可以看到，队伍中的托管对象的**名字字段**属性填入链接的**givenName**值 Azure 的 AD 对象。

如果您希望自定义属性映射或如果您想要还原自定义设置恢复为默认的配置时，，你可以通过单击位于应用程序的底部工具栏上的相关的按钮。


![销售队伍][3]  


有一个 SaaS 应用程序正常运行所需的属性映射。 在属性表中，相关的属性映射具有**是****必需**的属性的值。 如果需要一个属性映射，则您不能删除它。 在这种情况下，**删除**的功能将不可用。

若要修改现有属性映射，只需选择该映射，然后单击**编辑**。 这将打开对话框页面，使您可以修改所选的属性映射。


![编辑属性映射][4]  



## <a name="understanding-attribute-mapping-types"></a>了解属性映射类型


与属性映射，您可以控制如何将属性填入第三方 SaaS 应用程序。 有四个不同的映射类型支持︰

- **直接**--目标属性填充了链接的对象的属性的值在 Azure 的广告。


- **常量**--目标属性填入您指定的特定字符串。


- **表达式**的目标属性是根据类似脚本的表达式的结果来填充的。 有关更多详细信息，请参阅[书写表达式的 Azure Active Directory 中的属性映射](active-directory-saas-writing-expressions-for-attribute-mappings.md)。


- **无**-目标属性左不被修改。 但是，如果目标属性是以往任何时候都是空的它将随您指定的默认值。



除了这四种基本属性映射类型，自定义属性映射支持**默认**值分配的概念。 分配默认值可以确保，如果没有任何一个值在 Azure 的广告，也不在目标对象上用值填充目标属性。

Microsoft Azure 广告提供了一个非常有效的同步过程实现。 在初始化环境中，只需要更新对象期间处理的同步周期。 更新属性映射的同步周期的性能有一定影响。 这是因为属性映射配置的更新要求所有托管的对象进行重新求值。 正因为如此，它是建议的最佳做法，以保持最少属性映射到连续的更改的次数。


##<a name="related-articles"></a>相关的文章

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
- [自动化用户资源调配/撤消对 SaaS 应用程序](active-directory-saas-app-provisioning.md)
- [编写属性映射的表达式](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [作用域为用户提供的筛选器](active-directory-saas-scoping-filters.md)
- [如何使用 SCIM 以使用户和应用程序从 Azure 的 Active Directory 组的自动资源调配](active-directory-scim-provisioning.md)
- [帐户设置通知](active-directory-saas-account-provisioning-notifications.md)
- [有关如何集成 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)


<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
