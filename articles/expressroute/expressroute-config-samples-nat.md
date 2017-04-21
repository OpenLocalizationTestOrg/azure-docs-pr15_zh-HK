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

# <a name="router-configuration-samples-to-setup-and-manage-nat"></a>若要设置和管理 NAT 的路由器配置示例

此页提供为 Cisco ASA 和刺柏 SRX 系列路由器 NAT 配置示例。 这些都应是样本仅作为指导，不是能使用。 您可以使用您的供应商，以拿出相应的配置，为您的网络。 

>[AZURE.IMPORTANT] 在此页面中的示例都应是纯粹的指南。 您必须使用供应商的销售 / 技术小组和网络团队以拿出相应的配置以满足您的需要。 Microsoft 不支持在此页中列出的配置与相关的问题。 必须与您的设备供应商联系以解决支持问题。

下面示例路由器配置将应用到 Azure 公共和 Microsoft peerings。 不，您必须配置 NAT 的 Azure 专用对等。 有关更多详细信息查看[ExpressRoute peerings](expressroute-circuit-peerings.md)和[ExpressRoute NAT 的要求](expressroute-nat.md)。

**注意︰**用于连接到 internet 和 ExpressRoute，则必须使用单独的 NAT IP 池。 在互联网和 ExpressRoute 使用相同的 NAT IP 池将导致非对称路由和连接丢失。

## <a name="cisco-asa-firewalls"></a>Cisco ASA 防火墙

### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>PAT 配置为向 Microsoft 从客户网络通信的

    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>
    
    
    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>
    
    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2
    
    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>有关从 Microsoft 到客户网络通信 PAT 配置

#### <a name="interfaces-and-direction"></a>接口和方向︰
    Source Interface (where the traffic enters the ASA): inside
    Destination Interface (where the traffic exits the ASA): outside

#### <a name="configuration"></a>配置︰
NAT 池︰

    object network outbound-PAT
        host <NAT-IP>

目标服务器︰

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

对于客户 IP 地址的对象组

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>
    
    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

NAT 的命令︰

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a>刺柏 SRX 系列路由器 

### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1.创建群集的冗余以太网接口

    interfaces {
        reth0 {
            description "To Internal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "To Microsoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "To Microsoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a>2.创建两个安全区域

 - 内部网络的信任区域和外部面临边缘路由器的网络的不可信区域
 - 为区域指定合适的接口
 - 允许在接口上的服务


    安全区域 {{安全区域信任 {主机的入站的流量 {系统-服务 {ping;                 } {bgp; 协议                 }} {reth0.100; 接口             }} 安全区域不可信 {主机的入站的流量 {系统-服务 {ping;                 } {bgp; 协议                 }} {reth1.100; 接口             }          }      }  }


### <a name="3-create-security-policies-between-zones"></a>3.创建区域之间的安全策略
 
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a>4.配置 NAT 的策略
 - 创建两个 NAT 池。 一个将使用 NAT 通讯站向 Microsoft 和其他来自 Microsoft 给客户。
 - 创建各自的通信的规则来对 NAT

        security {
            nat {
                source {
                    pool SNAT-To-ExpressRoute {
                        routing-instance {
                            External-ExpressRoute;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    pool SNAT-From-ExpressRoute {
                        routing-instance {
                            Internal;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    rule-set Outbound_NAT {
                        from routing-instance Internal;
                        to routing-instance External-ExpressRoute;
                        rule SNAT-Out {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-To-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                    rule-set Inbound-NAT {
                        from routing-instance External-ExpressRoute;
                        to routing-instance Internal;
                        rule SNAT-In {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-From-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }


### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5.配置 BGP 公布在每个方向的选择性前缀

请参阅[路由配置示例](expressroute-config-samples-routing.md)页面中的示例。

### <a name="6-create-policies"></a>6.创建策略

    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a>下一步行动

查看更多详细信息的[ExpressRoute 的常见问题解答](expressroute-faqs.md)。
