<properties 
    pageTitle="麻将牌库中的相关和链接的资源" 
    description="了解相关和链接资源的 Azure 预览门户平铺样式库中显示。" 
    services="azure-portal" 
    documentationCenter="" 
    authors="adamabdelhamed" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-portal" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/16/2015" 
    ms.author="adamab"/>

# <a name="related-and-linked-resources-in-the-tile-gallery"></a>麻将牌库中的相关和链接的资源

平铺样式库可以查找特定的资源图块并将它们拖动到您当前的刀片式服务器。 使用平铺样式库，您可以创建跨越资源的管理视图。 对于任何指定的资源，相关的资源包括所有共享资源，同一个资源组的资源和链接到或从该资源的所有资源。

## <a name="linked-resources-in-azure-resource-manager"></a>链接的资源在 Azure 资源管理器

链接是 Azure 资源管理器的功能。  它使您能够声明资源之间的关系，即使它们不能驻留在同一个资源组中。 链接的资源、 收费，不会影响和基于角色的访问不会影响运行时任何影响。  它是只可用于表示关系，以便这些工具像拼贴库可提供丰富的管理经验的机制。  您的工具可以检查的链接使用 API 的链接并提供自定义关系的管理以及经验。 

## <a name="how-do-i-link-my-resources"></a>如何将我的资源的链接？

创建资源通过门户或通过部署到 Azure PowerShell 或 Azure CLI 的模板时，链接会自动创建一些相关的资源。 通过使用[链接资源的 REST API](https://msdn.microsoft.com/library/azure/mt238499.aspx)或通过声明模板中的关系，您可以以编程方式链接资源。 有关使用链接的资源的完整讨论，请参阅[链接资源 Azure 资源管理器中](../resource-group-link-resources.md)。

## <a name="next-steps"></a>下一步行动

- 如果您需要对编写 Azure 资源管理器模板的介绍，请参阅[创作模板](../resource-group-authoring-templates.md)。
- 进行深入研究，详细讨论创建资源之间的链接，请参阅[链接资源在 Azure 资源管理器](../resource-group-link-resources.md)。
- 要了解更多有关使用资源组通过预览门户，请参见[使用 Azure 预览门户管理 Azure 的资源](resource-group-portal.md)。
