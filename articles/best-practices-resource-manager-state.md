<properties
    pageTitle="在资源管理器模板中的处理状态 |Microsoft Azure"
    description="显示建议使用复杂对象使用 Azure 资源管理器模板和链接的模板共享状态数据的方法"
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="tomfitz"/>

# <a name="sharing-state-in-azure-resource-manager-templates"></a>共享模板 Azure 资源管理器中的状态

本主题说明为管理和共享模板内的状态的最佳做法。 参数和本主题中所示的变量是可以定义的对象的类型的示例可以方便地组织您的部署需求。 从这些示例中，您可以实现您自己的对象具有对您的环境有意义的属性值。

本主题是白皮书的属于更大。 若要阅读完整的纸张，下载[世界类资源管理器模板注意事项和经验证的做法](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf)。


## <a name="provide-standard-configuration-settings"></a>提供标准的配置设置

而不是提供总的灵活性和无数的变化提供了一个模板，常见模式是提供的已知的配置选择。 实际上，用户可以选择标准的 t 恤尺寸，例如沙箱中，小型、 中型和大型。 T 恤衫大小的其他例子有产品，如社区版或企业版。 在其他情况下，它可能是工作负载特定配置的一种技术 — 如图减少或没有 sql。

复杂对象时，可以创建包含的数据，有时也称为"属性包"集合的变量，并使用这些数据来驱动资源声明您的模板中。 这种方法为客户提供不同大小的预配置好的已知配置。 没有已知的配置模板的用户必须确定群集大小调整自己、 平台资源的限制因素和做数学计算来识别分区存储帐户和其他资源 （由于群集大小和资源约束） 生成。 除了为客户进行更好的体验，一些已知的配置是易于支持，可以帮助您提供更高的密度水平。

下面的示例演示如何定义变量，其中包含复杂对象用于表示数据的集合。 集合定义用于虚拟机大小、 网络设置、 操作系统设置和可用性设置的值。

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

请注意**tshirtSize**变量，连接到文本**tshirtSize**参数 （**小型**，**中型**，**大型**） 通过您提供的 t 恤尺寸。 使用此变量来检索关联的复杂对象变量，该 t 恤衫大小。

然后可以引用更高版本的模板中的这些变量。 引用命名变量和它们的属性的能力简化的模板语法，并可以轻松地理解上下文。 下面的示例定义资源部署程序之前所示设置值的对象。 例如，通过检索的值设置虚拟机大小`variables('tshirtSize').vmSize`在值的大小从`variables('tshirtSize').diskSize`。 此外，将模板链接的 URI 设置为的值`variables('tshirtSize').vmTemplate`。

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-to-a-template"></a>将状态传递给模板

您在转换为模板，通过直接在部署过程中提供的参数共享状态。

下表列出了常用的模板中的参数。

名称 | 值 | 说明
---- | ----- | -----------
位置    | 从 Azure 区域约束列表的字符串   | 在其中部署资源的位置。
storageAccountNamePrefix    | 字符串    | 虚拟机的磁盘的放置位置的存储帐户的唯一 DNS 名称
域名称  | 字符串    | 域名的公共可访问 jumpbox 虚拟机格式︰ **{域名}。 {位置}.cloudapp.com** ，例如︰ **mydomainname.westus.cloudapp.azure.com**
adminUsername   | 字符串    | 用户名为虚拟机的
adminPassword   | 字符串    | 虚拟机的的密码
tshirtSize  | 从约束列表中的字符串提供 t 恤衫大小   | 配置命名的比例单元大小。 例如，"小"，"中"，"大"
virtualNetworkName  | 字符串    | 使用者想要使用的虚拟网络名称。
enableJumpbox   | 从约束列表 （启用/禁用） 的字符串 | 确定是否启用环境 jumpbox 的参数。 值:"已启用"，"已禁用"

使用上一节中的**tshirtSize**参数定义如下︰

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a>将状态传递到链接的模板

连接到链接的模板时，您通常使用的静态混合并生成变量。

### <a name="static-variables"></a>静态变量

静态变量通常用于提供基本值，例如在整个模板中使用的 Url。

在下面的模板摘录，`templateBaseUrl`在 GitHub 中指定该模板的根位置。 下一步的行生成一个新变量`sharedTemplateUrl`的连接具有已知名称的共享的资源模板的基 URL。 线的下方的复杂对象变量用于存储 t 恤尺寸，其中基 URL 是连接到已知的配置模板位置，存储在`vmTemplate`属性。

这种方法的好处是，如果模板位置发生更改，您只需更改静态变量在一个位置，将其传递到整个链接的模板。

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a>生成的变量

除了静态变量，动态地生成多个变量。 本节将介绍一些常见类型的生成的变量。

#### <a name="tshirtsize"></a>tshirtSize

您熟悉此生成变量从上面的例子。

#### <a name="networksettings"></a>网络设置的接口

在容量、 功能或指定了作用域的端到端的解决方案模板中，链接的模板通常在网络上创建存在的资源。 一种简单的方法是使用一个复杂的对象存储网络设置并将它们传递到链接的模板。

如下所示进行通信的网络设置的一个示例。

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings

在链接的模板中创建的资源通常放置在可用性设置。 在以下示例中，指定可用性组名称和故障域和更新域进行计数来使用。

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

如果需要多个可用性集 （例如，一个用于主节点），另一个用于数据节点，您可以使用名称作为前缀，指定多个可用性设置，或者按照前面所示的用于创建特定的 t 恤尺寸变量的模型。

#### <a name="storagesettings"></a>storageSettings

与链接的模板通常共享存储详细信息。 在下面的示例中， *storageSettings*对象提供有关存储帐户和容器名称的详细信息。

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings

与链接的模板，可能需要在不同的已知的配置类型的各种节点类型传递操作系统设置。 复杂对象是存储和共享系统信息的简便方法，还便于更方便地支持多个操作系统选择以进行部署。

下面的示例显示*osSettings*的对象︰

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings

生成的变量中， *machineSettings*是一个复杂对象，包含用于创建虚拟机的核心变量的混合。 可变因素包括管理员用户名和密码、 虚拟机名称和操作系统图像引用前缀。

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

请注意， *osImageReference*从主模板中定义的*osSettings*变量中检索的值。 这意味着，您可以轻松地更改操作系统的虚拟机 — — 完全或根据模板使用者的偏好。

#### <a name="vmscripts"></a>vmScripts

*VmScripts*对象包含有关下载和执行上的虚拟机实例，包括外部和内部引用的脚本的详细信息。 外部引用包括基础结构。 内部引用包含的已安装的软件安装和配置。

使用*scriptsToDownload*属性列表的脚本下载到 VM。 此对象还包含对不同类型的操作的命令行参数的引用。 这些操作包括执行默认安装的每个节点之后的所有节点的都部署，运行安装，可能是特定于给定的模板的所有其他脚本。

此示例是从模板用于部署 MongoDB，要求仲裁服务器，以提供高可用性。 *ArbiterNodeInstallCommand*已添加到*vmScripts*安装仲裁服务器。

变量部分是在哪里找到定义要执行的脚本用适当值的特定文本的变量。

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>从模板返回状态

不仅可以传递数据转换为模板，您还可以共享数据返回到调用的模板。 在模板链接的**输出**部分中，您可以提供可由源模板的键/值对。

下面的示例演示如何通过在链接模板中生成的专用 IP 地址。

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

在主模板中，可以使用下面的语法使用该数据︰

    "[reference('master-node').outputs.masterip.value]"

您可以使用此表达式的输出部分或主模板的资源部分中。 不能在变量部分中使用表达式，因为它依赖于运行时状态。 若要从主模板返回此值，请使用︰

    "outputs": { 
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }
     
使用模板链接的输出部分以返回虚拟机的数据磁盘的示例，请参阅[创建多个虚拟机的数据磁盘](resource-group-create-multiple.md#creating-multiple-data-disks-for-a-virtual-machine)。

## <a name="define-authentication-settings-for-virtual-machine"></a>定义虚拟机的身份验证设置

相同的图案显示以前的配置设置用于指定虚拟机的身份验证设置。 您创建的身份验证类型传递参数。

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

添加不同的身份验证类型的变量，变量可以存储哪些类型用于此部署基于参数的值。

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

在定义的虚拟机，将**osProfile**设置为您所创建的变量。

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>下一步行动
- 若要了解详细信息模板的部分，请参阅[创作 Azure 资源管理器模板](resource-group-authoring-templates.md)
- 在模板中可用的函数，请参见[Azure 资源管理器模板函数](resource-group-template-functions.md)

