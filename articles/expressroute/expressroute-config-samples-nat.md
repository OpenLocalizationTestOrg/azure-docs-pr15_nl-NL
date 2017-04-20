<properties
   pageTitle="ExpressRoute klant router configuratie voorbeelden | Microsoft Azure"
   description="Deze pagina vindt voorbeelden van router configuratie voor Cisco-en Juniper."
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

# <a name="router-configuration-samples-to-setup-and-manage-nat"></a>Voorbeelden van de router configuratie instellen en beheren van NAT

Deze pagina vindt voorbeelden van NAT-configuratie voor Cisco ASA en Juniper SRX reeks routers. Deze zijn bedoeld om te worden voorbeelden voor instructies alleen en mag niet worden gebruikt als is. U kunt werken met de leverancier van uw om uit te komen van geschikte configuraties voor uw netwerk. 

>[AZURE.IMPORTANT] Voorbeelden in deze pagina zijn bedoeld voor zuiver voor instructies. U moet werken met de leverancier van uw van verkoop / technische team en uw team netwerken bovenkomen met de juiste configuraties naar aan uw wensen voldoet. Microsoft ondersteuning geen met betrekking tot configuraties op deze pagina wordt vermeld. U moet contact opnemen met de leverancier van uw apparaat voor ondersteuning.

Router configuratie met de onderstaande voorbeelden zijn van toepassing op openbare Azure en Microsoft peerings. U moet NAT niet configureren voor Azure privé peering. Lees [ExpressRoute peerings](expressroute-circuit-peerings.md) en [ExpressRoute NAT vereisten](expressroute-nat.md) voor meer informatie.

**Notitie:** Voor verbindingen met internet en het ExpressRoute, moet u afzonderlijke NAT IP van toepassingen gebruiken. Dezelfde NAT IP resourcegroep gebruiken via internet en ExpressRoute treden asymmetrische Routering en verbroken.

## <a name="cisco-asa-firewalls"></a>Cisco ASA firewalls

### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>PAT configuratie voor verkeer van customer network voor naar Microsoft

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

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>PAT configuratie voor verkeer van Microsoft tot customer network

#### <a name="interfaces-and-direction"></a>Interfaces en richting:
    Source Interface (where the traffic enters the ASA): inside
    Destination Interface (where the traffic exits the ASA): outside

#### <a name="configuration"></a>Configuratie:
NAT-toepassingen:

    object network outbound-PAT
        host <NAT-IP>

Doelserver:

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

Objectgroep voor klant IP-adressen

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>
    
    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

NAT-opdrachten:

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a>Juniper SRX reeks routers 

### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1. redundante Ethernet-interfaces voor het cluster maken

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


### <a name="2-create-two-security-zones"></a>2. twee beveiligingszones maken

 - Zone voor interne netwerk- en Untrust Zone voor externe netwerk die tegenover elkaar liggen rand Routers vertrouwen
 - Juiste interfaces toewijzen aan de zones
 - Services op de interfaces toestaan


    beveiliging {zones {-beveiligingszones vertrouwen {host inkomende-verkeer {systeem-services {ping;                  } {bgp; protocollen                  via interfaces zoals}} {reth0.100;              }}-beveiligingszones Untrust {host inkomende-verkeer {systeem-services {ping;                  } {bgp; protocollen                  via interfaces zoals}} {reth1.100;              }          }      }  }


### <a name="3-create-security-policies-between-zones"></a>3. beveiligingsbeleid voor apparaten tussen de zones maken
 
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


### <a name="4-configure-nat-policies"></a>4. NAT beleidsregels configureren
 - Maak twee NAT-toepassingen. Een wordt gebruikt voor NAT-verkeer naar Microsoft en andere van Microsoft naar de klant.
 - Regels NAT maken voor de desbetreffende verkeer

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


### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5. BGP om te kondigen selectief voorvoegsels voor eenheden in elke richting configureren

Verwijzen naar de voorbeelden in pagina [Routering configuratie voorbeelden](expressroute-config-samples-routing.md) .

### <a name="6-create-policies"></a>6. beleid maken

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

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor meer informatie.
