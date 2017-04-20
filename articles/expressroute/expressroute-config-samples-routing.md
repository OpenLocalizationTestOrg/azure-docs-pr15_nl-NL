<properties
   pageTitle="ExpressRoute klant router configuratie voorbeelden | Microsoft Azure"
   description="Deze pagina vindt router config voorbeelden van Cisco en Juniper."
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

# <a name="router-configuration-samples-to-setup-and-manage-routing"></a>Voorbeelden van de router configuratie instellen en beheren van Routering

Deze pagina vindt interface en hetzelfde routeren configuratie voorbeelden van Cisco IOS-XE en Juniper MX reeks routers. Deze zijn bedoeld om te worden voorbeelden voor instructies alleen en mag niet worden gebruikt als is. U kunt werken met de leverancier van uw om uit te komen van geschikte configuraties voor uw netwerk. 

>[AZURE.IMPORTANT] Voorbeelden in deze pagina zijn bedoeld voor zuiver voor instructies. U moet werken met de leverancier van uw van verkoop / technische team en uw team netwerken bovenkomen met de juiste configuraties naar aan uw wensen voldoet. Microsoft ondersteuning geen met betrekking tot configuraties op deze pagina wordt vermeld. U moet contact opnemen met de leverancier van uw apparaat voor ondersteuning.

Router configuratie met de onderstaande voorbeelden zijn van toepassing op alle peerings. [ExpressRoute peerings](expressroute-circuit-peerings.md) en [ExpressRoute routeren vereisten](expressroute-routing.md) voor meer informatie over de routering controleren.

## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS-XE op basis van routers

In de voorbeelden in deze sectie toepassen voor elk router met de IOS-XE OS-familie.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. interfaces en submappen interfaces configureren

U moet een sub-interface per peering in elke router die u verbinding met Microsoft maken. Een sub-interface kan worden geïdentificeerd met een VLAN-ID of een gestapeld paar VLAN-id's en een IP-adres.

#### <a name="dot1q-interface-definition"></a>Dot1Q interfacedefinitie

In dit voorbeeld bevat de interfacedefinitie onderliggend voor een onderliggend interface met een enkel VLAN-ID. De VLAN-ID is unieke per peering. De laatste decimale waarden van uw IPv4-adres is altijd een oneven getal.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

#### <a name="qinq-interface-definition"></a>QinQ interfacedefinitie

In dit voorbeeld bevat de interfacedefinitie onderliggend voor een onderliggend interface met een twee VLAN-id's. De outer VLAN-ID (s-tag), als gebruikt blijft dezelfde over alle peerings. De binnenste VLAN-ID (c-code) is unieke per peering. De laatste decimale waarden van uw IPv4-adres is altijd een oneven getal.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>
    
### <a name="2-setting-up-ebgp-sessions"></a>2. instellen eBGP sessies

U moet een BGP-sessie met Microsoft voor elke peering instellen. Het onderstaande voorbeeld kunt u voor het instellen van een BGP-sessie met Microsoft. Als het IPv4-adres dat u voor de sub-interface van uw gebruikt a.b.c.d, is het IP-adres van de neighbor BGP (Microsoft) a.b.c.d+1. De laatste decimale waarden van de BGP-neighbor IPv4-adres zijn altijd een even getal.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. instellen voorvoegsels voor eenheden aangekondigd via de sessie BGP

U kunt configureren dat uw router om te kondigen select voorvoegsels voor eenheden naar Microsoft. U kunt doen met het onderstaande voorbeeld.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. toewijzingen van de route van

U kunt route-kaarten en voorvoegsel lijsten op filter voorvoegsels doorgegeven uw netwerk. U kunt het onderstaande voorbeeld gebruiken voor het uitvoeren van de taak. Zorg ervoor dat er juiste voorvoegsel lijsten-instelling.

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


## <a name="juniper-mx-series-routers"></a>Juniper MX reeks routers 

In de voorbeelden in deze sectie toepassen voor een Juniper MX reeks router.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. interfaces en submappen interfaces configureren

#### <a name="dot1q-interface-definition"></a>Dot1Q interfacedefinitie

In dit voorbeeld bevat de interfacedefinitie onderliggend voor een onderliggend interface met een enkel VLAN-ID. De VLAN-ID is unieke per peering. De laatste decimale waarden van uw IPv4-adres is altijd een oneven getal.

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


#### <a name="qinq-interface-definition"></a>QinQ interfacedefinitie

In dit voorbeeld bevat de interfacedefinitie onderliggend voor een onderliggend interface met een twee VLAN-id's. De outer VLAN-ID (s-tag), als gebruikt blijft dezelfde over alle peerings. De binnenste VLAN-ID (c-code) is unieke per peering. De laatste decimale waarden van uw IPv4-adres is altijd een oneven getal.

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

### <a name="2-setting-up-ebgp-sessions"></a>2. instellen eBGP sessies

U moet een BGP-sessie met Microsoft voor elke peering instellen. Het onderstaande voorbeeld kunt u voor het instellen van een BGP-sessie met Microsoft. Als het IPv4-adres dat u voor de sub-interface van uw gebruikt a.b.c.d, is het IP-adres van de neighbor BGP (Microsoft) a.b.c.d+1. De laatste decimale waarden van de BGP-neighbor IPv4-adres zijn altijd een even getal.

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

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. instellen voorvoegsels voor eenheden aangekondigd via de sessie BGP

U kunt configureren dat uw router om te kondigen select voorvoegsels voor eenheden naar Microsoft. U kunt doen met het onderstaande voorbeeld.

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


### <a name="4-route-maps"></a>4. toewijzingen van de route van

U kunt route-kaarten en voorvoegsel lijsten op filter voorvoegsels doorgegeven uw netwerk. U kunt het onderstaande voorbeeld gebruiken voor het uitvoeren van de taak. Zorg ervoor dat er juiste voorvoegsel lijsten-instelling.

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

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor meer informatie.
