<properties
    pageTitle="管理基于角色的访问控制与 REST API，"
    description="管理基于角色的访问控制与 REST API，"
    services="active-directory"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="multiple"
    ms.tgt_pltfrm="rest-api"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-role-based-access-control-with-the-rest-api"></a>管理基于角色的访问控制与 REST API，

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API，](role-based-access-control-manage-access-rest.md)

基于角色的访问控制 (RBAC) 在 Azure 门户和 Azure 资源管理器 API 可帮助您管理您的订阅和细粒度级别的资源访问。 使用此功能，您可以通过在特定的范围内，向其分配一些角色授予访问 Active Directory 用户、 组或服务主体。

## <a name="list-all-role-assignments"></a>列出的所有角色分派

列出在指定的范围和 subscopes 的所有角色分派。

向角色分配，您必须有权访问`Microsoft.Authorization/roleAssignments/read`范围内的操作。 所有内置角色授予对该操作的访问。 有关角色分配和 Azure 资源的管理访问权限的详细信息，请参阅[Azure Role-Based 访问控制](role-based-access-control-configure.md)。

### <a name="request"></a>请求

**GET**方法用于以下 URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

在 URI 中，进行以下的替代自定义您的请求︰

1. *{范围}*替换为您希望列出的角色分配的范围。 下面的示例演示如何指定不同级别的作用域︰

  - 订阅︰ /subscriptions/ {订阅 id}  
  - 资源组︰ /subscriptions/ {订阅 id} / resourceGroups/myresourcegroup1  
  - 资源︰ /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Api 版本}*替换 2015年-07-01。

3. 替换为要应用筛选的角色分配列表条件*{滤镜}* :

  - 列出指定仅指定的范围不包括在 subscopes 的角色分配给角色︰`atScope()`    
  - 列出指定给特定的用户、 组或应用程序角色︰`principalId%20eq%20'{objectId of user, group, or service principal}'`  
  - 列出特定用户，包括从组继承的角色分配 |`assignedTo('{objectId of user}')`

### <a name="response"></a>响应

状态代码︰ 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>获取有关角色分配的信息

获取有关单个角色分配角色分配标识符所指定的信息。

若要获取有关角色分配的信息，您必须访问`Microsoft.Authorization/roleAssignments/read`操作。 所有内置角色授予对该操作的访问。 有关角色分配和 Azure 资源的管理访问权限的详细信息，请参阅[Azure Role-Based 访问控制](role-based-access-control-configure.md)。

### <a name="request"></a>请求

**GET**方法用于以下 URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

在 URI 中，进行以下的替代自定义您的请求︰

1. *{范围}*替换为您希望列出的角色分配的范围。 下面的示例演示如何指定不同级别的作用域︰

  - 订阅︰ /subscriptions/ {订阅 id}  
  - 资源组︰ /subscriptions/ {订阅 id} / resourceGroups/myresourcegroup1  
  - 资源︰ /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. 角色分配的 GUID 标识符替换*{角色分配 id}* 。

3. *{Api 版本}*替换 2015年-07-01。

### <a name="response"></a>响应

状态代码︰ 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>创建角色分配

在指定范围内指定主体授予指定的角色创建角色分配。

若要创建角色分配，您必须访问`Microsoft.Authorization/roleAssignments/write`操作。 内置的角色，只有*所有者*和*用户访问管理员*授予对此操作的访问。 有关角色分配和 Azure 资源的管理访问权限的详细信息，请参阅[Azure Role-Based 访问控制](role-based-access-control-configure.md)。

### <a name="request"></a>请求

**将**方法用于以下 URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

在 URI 中，进行以下的替代自定义您的请求︰

1. *{范围}*替换为您要创建的角色分配的范围。 当您在某个父作用域创建角色分配时，所有子作用域都继承相同的角色分配。 下面的示例演示如何指定不同级别的作用域︰

  - 订阅︰ /subscriptions/ {订阅 id}  
  - 资源组︰ /subscriptions/ {订阅 id} / resourceGroups/myresourcegroup1   
  - 资源︰ /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. 替换为新的 GUID，成为新的角色分配的 GUID 标识符*{0} 角色分配 id}* 。

3. *{Api 版本}*替换 2015年-07-01。

对于请求体，提供了以下面的格式的值︰

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| 元素名称     | 必填 | 类型   | 说明 |
|------------------|----------|--------|-------------|
| roleDefinitionId | 是的      | 字符串 | 角色的标识符。 标识符的格式为︰`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId      | 是的      | 字符串 | 向其分配该角色的 Azure 广告主体 （用户、 组或服务主体） 的对象 Id。 |

### <a name="response"></a>响应

状态代码︰ 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>删除角色分配

删除在指定范围内的角色分配。

若要删除角色分配，您必须访问`Microsoft.Authorization/roleAssignments/delete`操作。 内置的角色，只有*所有者*和*用户访问管理员*授予对此操作的访问。 有关角色分配和 Azure 资源的管理访问权限的详细信息，请参阅[Azure Role-Based 访问控制](role-based-access-control-configure.md)。

### <a name="request"></a>请求

使用**DELETE**方法具有以下 URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

在 URI 中，进行以下的替代自定义您的请求︰

1. *{范围}*替换为您要创建的角色分配的范围。 下面的示例演示如何指定不同级别的作用域︰

  - 订阅︰ /subscriptions/ {订阅 id}  
  - 资源组︰ /subscriptions/ {订阅 id} / resourceGroups/myresourcegroup1  
  - 资源︰ /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{角色分配 id}*替换角色分配标识的 GUID。

3. *{Api 版本}*替换 2015年-07-01。

### <a name="response"></a>响应

状态代码︰ 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>列出所有角色

列出可用于在指定范围内分配的所有角色。

到列表中的角色，您必须有权访问`Microsoft.Authorization/roleDefinitions/read`范围内的操作。 所有内置角色授予对该操作的访问。 有关角色分配和 Azure 资源的管理访问权限的详细信息，请参阅[Azure Role-Based 访问控制](role-based-access-control-configure.md)。

### <a name="request"></a>请求

**GET**方法用于以下 URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

在 URI 中，进行以下的替代自定义您的请求︰

1. *{范围}*替换为您希望列出角色的作用域。 下面的示例演示如何指定不同级别的作用域︰

  - 订阅︰ /subscriptions/ {订阅 id}  
  - 资源组︰ /subscriptions/ {订阅 id} / resourceGroups/myresourcegroup1  
  - 资源 /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Api 版本}*替换 2015年-07-01。

3. 替换为要应用筛选的角色列表条件*{滤镜}* :

  - 列出可用于分配指定的作用域和任何子作用域的角色︰`atScopeAndBelow()`
  - 搜索为角色使用精确的显示名称: `roleName%20eq%20'{role-display-name}'`。 使用精确的显示名称角色的 URL 编码形式。 例如，`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>响应

状态代码︰ 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>获取角色信息

获取有关由角色定义标识符指定单个角色的信息。 若要获取有关使用其显示名称单个角色的信息，请参阅[列出所有的角色](role-based-access-control-manage-access-rest.md#list-all-roles)。

若要获取有关某个角色的信息，您必须访问`Microsoft.Authorization/roleDefinitions/read`操作。 所有内置角色授予对该操作的访问。 有关角色分配和 Azure 资源的管理访问权限的详细信息，请参阅[Azure Role-Based 访问控制](role-based-access-control-configure.md)。

### <a name="request"></a>请求

**GET**方法用于以下 URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

在 URI 中，进行以下的替代自定义您的请求︰

1. *{范围}*替换为您希望列出的角色分配的范围。 下面的示例演示如何指定不同级别的作用域︰

  - 订阅︰ /subscriptions/ {订阅 id}  
  - 资源组︰ /subscriptions/ {订阅 id} / resourceGroups/myresourcegroup1  
  - 资源︰ /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{角色定义 id}*替换为该角色定义的 GUID 标识符。

3. *{Api 版本}*替换 2015年-07-01。

### <a name="response"></a>响应

状态代码︰ 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>创建自定义角色
创建自定义角色。

若要创建自定义角色，您必须访问`Microsoft.Authorization/roleDefinitions/write`在所有操作`AssignableScopes`。 内置的角色，只有*所有者*和*用户访问管理员*授予对此操作的访问。 有关角色分配和 Azure 资源的管理访问权限的详细信息，请参阅[Azure Role-Based 访问控制](role-based-access-control-configure.md)。

### <a name="request"></a>请求

**将**方法用于以下 URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

在 URI 中，进行以下的替代自定义您的请求︰

1. 替换为自定义角色的第一个*AssignableScope* *{范围}* 。 下面的示例演示如何指定不同级别的范围。

  - 订阅︰ /subscriptions/ {订阅 id}  
  - 资源组︰ /subscriptions/ {订阅 id} / resourceGroups/myresourcegroup1  
  - 资源︰ /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. 替换为一个新的 GUID，成为新的自定义角色的 GUID 标识符*{0} 角色定义 id}* 。

3. *{Api 版本}*替换 2015年-07-01。

对于请求体，提供了以下面的格式的值︰

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| 元素名称 | 必填 | 类型 | 说明 |
|--------------|----------|------|-------------|
| 名称         | 是的 | 字符串   | 自定义角色的 GUID 标识符。    |
| properties.roleName               | 是的 | 字符串   | 显示自定义的角色的名称。 最大大小为 128 个字符。                        |
| properties.description            | 不  | 字符串   | 自定义角色的说明。 最大大小为 1024年个字符。                                               |
| properties.type                   | 是的 | 字符串   | 设置为"CustomRole"。                                         |
| properties.permissions.actions    | 是的 | String] | 指定授予自定义角色的操作的操作字符串的数组。             |
| properties.permissions.notActions | 不  | String] | 指定的操作来从自定义角色授予操作中排除操作字符串的数组。 |
| properties.assignableScopes       | 是的 | String] | 可以使用自定义角色的范围的数组。   |

### <a name="response"></a>响应

状态代码︰ 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>更新自定义角色

修改自定义角色。

若要修改自定义角色，您必须访问`Microsoft.Authorization/roleDefinitions/write`在所有操作`AssignableScopes`。 内置的角色，只有*所有者*和*用户访问管理员*授予对此操作的访问。 有关角色分配和 Azure 资源的管理访问权限的详细信息，请参阅[Azure Role-Based 访问控制](role-based-access-control-configure.md)。

### <a name="request"></a>请求

**将**方法用于以下 URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

在 URI 中，进行以下的替代自定义您的请求︰

1. 替换为自定义角色的第一个*AssignableScope* *{范围}* 。 下面的示例演示如何指定不同级别的作用域︰

  - 订阅︰ /subscriptions/ {订阅 id}  
  - 资源组︰ /subscriptions/ {订阅 id} / resourceGroups/myresourcegroup1  
  - 资源︰ /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. 自定义角色的 GUID 标识符替换*{角色定义 id}* 。

3. *{Api 版本}*替换 2015年-07-01。

对于请求体，提供了以下面的格式的值︰

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| 元素名称 | 必填 | 类型 | 说明 |
|--------------|----------|------|-------------|
| 名称         | 是的      | 字符串 | 自定义角色的 GUID 标识符。 |
| properties.roleName | 是的 | 字符串 | 显示已更新的自定义角色的名称。 |
| properties.description | 不 | 字符串 | 已更新的自定义角色的说明。 |
| properties.type | 是的 | 字符串 | 设置为"CustomRole"。 |
| properties.permissions.actions | 是的 | String] | 指定的已更新自定义角色授予访问权限的操作的操作字符串的数组。 |
| properties.permissions.notActions | 不 | String] | 指定要从已更新的自定义角色授予操作中排除的操作的操作字符串的数组。 |
| properties.assignableScopes | 是的 | String] | 数组可以在其中使用已更新的自定义角色的作用域。 |

### <a name="response"></a>响应

状态代码︰ 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>删除自定义角色

删除自定义角色。

若要删除一个自定义角色，您必须访问`Microsoft.Authorization/roleDefinitions/delete`在所有操作`AssignableScopes`。 内置的角色，只有*所有者*和*用户访问管理员*授予对此操作的访问。 有关角色分配和 Azure 资源的管理访问权限的详细信息，请参阅[Azure Role-Based 访问控制](role-based-access-control-configure.md)。

### <a name="request"></a>请求

使用**DELETE**方法具有以下 URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

在 URI 中，进行以下的替代自定义您的请求︰

1. *{范围}*替换为您要删除该角色定义的范围。 下面的示例演示如何指定不同级别的作用域︰

  - 订阅︰ /subscriptions/ {订阅 id}  
  - 资源组︰ /subscriptions/ {订阅 id} / resourceGroups/myresourcegroup1  
  - 资源︰ /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. 自定义角色的 GUID 角色定义 id 替换*{角色定义 id}* 。

3. *{Api 版本}*替换 2015年-07-01。

### <a name="response"></a>响应

状态代码︰ 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```


[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
