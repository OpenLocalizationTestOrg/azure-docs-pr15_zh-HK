<properties
   pageTitle="ExpressRoute 客户路由器配置示例 |Microsoft Azure"
   description="此页提供为 Cisco 和刺柏路由器路由器配置示例。"
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="router-configuration-samples-to-setup-and-manage-routing"></a>若要设置和管理路由的路由器配置示例

此页提供为 Cisco IOS XE 和刺柏 MX 系列路由器接口和路由的配置示例。 这些都应是样本仅作为指导，不是能使用。 您可以使用您的供应商，以拿出相应的配置，为您的网络。 

>[AZURE.IMPORTANT] 在此页面中的示例都应是纯粹的指南。 您必须使用供应商的销售 / 技术小组和网络团队以拿出相应的配置以满足您的需要。 Microsoft 不支持在此页中列出的配置与相关的问题。 必须与您的设备供应商联系以解决支持问题。

下面示例路由器配置将应用到所有的 peerings。 有关路由的详细查看[ExpressRoute peerings](expressroute-circuit-peerings.md)和[ExpressRoute 路由的要求](expressroute-routing.md)。

## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS XE 基于路由器

本节中的示例适用于运行的操作系统 IOS XE 系列所有路由器。

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1.配置接口和子接口

您将需要每个对等连接到每个路由器中的一个子界面。 子接口可以使用 VLAN ID 或堆积的对 VLAN Id 和 IP 地址来标识。

#### <a name="dot1q-interface-definition"></a>Dot1Q 接口定义

本示例提供了单一的 VLAN id 的子接口的子接口定义 VLAN ID 是唯一的每个对等的。 IPv4 地址的最后一个八位字节将始终为奇数。

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

#### <a name="qinq-interface-definition"></a>QinQ 接口定义

此示例提供两个 VLAN Id 与子接口的子接口定义。 外部 VLAN ID （s-标记），如果使用在所有 peerings 之间保持不变。 内部的 VLAN ID （c 标记） 是唯一每个对等的。 IPv4 地址的最后一个八位字节将始终为奇数。

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>
    
### <a name="2-setting-up-ebgp-sessions"></a>2.eBGP 会话设置

您必须安装与 Microsoft 的每个对等的 BGP 会话。 下面的示例中，您可以安装与 Microsoft 的 BGP 会话。 如果您使用您的子接口的 IPv4 地址为 a.b.c.d，BGP 邻居 (Microsoft) 的 IP 地址将是 a.b.c.d+1。 BGP 邻居的 IPv4 地址的最后一个八位字节将始终为偶数。

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3.设置前缀要公布对 BGP 会话

您可以配置您的路由器公布到 Microsoft 选择前缀。 您可以使用下面的示例这样做。

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4.工艺映射

可以使用路由映射和前缀列表筛选器前缀传播到您的网络。 下面的示例可用于完成此任务。 确保您有适当的前缀列表设置。

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>刺柏 MX 系列路由器 

本节中的示例适用于任何刺柏 MX 系列路由器。

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1.配置接口和子接口

#### <a name="dot1q-interface-definition"></a>Dot1Q 接口定义

本示例提供了单一的 VLAN id 的子接口的子接口定义 VLAN ID 是唯一的每个对等的。 IPv4 地址的最后一个八位字节将始终为奇数。

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


#### <a name="qinq-interface-definition"></a>QinQ 接口定义

此示例提供两个 VLAN Id 与子接口的子接口定义。 外部 VLAN ID （s-标记），如果使用在所有 peerings 之间保持不变。 内部的 VLAN ID （c 标记） 是唯一每个对等的。 IPv4 地址的最后一个八位字节将始终为奇数。

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2.eBGP 会话设置

您必须安装与 Microsoft 的每个对等的 BGP 会话。 下面的示例中，您可以安装与 Microsoft 的 BGP 会话。 如果您使用您的子接口的 IPv4 地址为 a.b.c.d，BGP 邻居 (Microsoft) 的 IP 地址将是 a.b.c.d+1。 BGP 邻居的 IPv4 地址的最后一个八位字节将始终为偶数。

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3.设置前缀要公布对 BGP 会话

您可以配置您的路由器公布到 Microsoft 选择前缀。 您可以使用下面的示例这样做。

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4.工艺映射

可以使用路由映射和前缀列表筛选器前缀传播到您的网络。 下面的示例可用于完成此任务。 确保您有适当的前缀列表设置。

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>下一步行动

查看更多详细信息的[ExpressRoute 的常见问题解答](expressroute-faqs.md)。
