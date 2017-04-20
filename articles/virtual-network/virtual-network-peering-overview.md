
<properties
   pageTitle="Azure virtuele netwerk peering | Microsoft Azure"
   description="Meer informatie over VNet peering in Azure wordt aangegeven."
   services="virtual-network"
   documentationCenter="na"
   authors="NarayanAnnamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="narayan" />

# <a name="vnet-peering"></a>VNet peering

VNet peering is een methode die met elkaar twee virtuele netwerken (VNets) in de dezelfde regio via het backbonenetwerk Azure verbindt. Nadat de peered, wordt de twee virtuele netwerken weergegeven als een handtekening voor alle connectivity doeleinden. Ze nog steeds worden beheerd als afzonderlijke resources, maar virtuele machines in deze virtuele netwerken met elkaar kunnen communiceren via privé IP-adressen.

Het verkeer tussen virtuele machines in de peered virtuele netwerken is gerouteerd via de Azure infrastructuur, net zoals verkeer tussen VMs in hetzelfde virtuele netwerk is doorgestuurd. Enkele van de voordelen van het gebruik van VNet peering zijn:

- Een lage latentie, hoge bandbreedte verbinding tussen bronnen in verschillende virtuele netwerken.
- De mogelijkheid om het gebruik van bronnen zoals netwerkapparaten en VPN gateways als overdracht punten in een peered VNet.
- De mogelijkheid om verbinding maken met een virtueel netwerk waarin het model Azure resourcemanager met een virtueel netwerk dat het klassieke implementatie-objectmodel gebruikt en volledige connectiviteit tussen bronnen in deze virtuele netwerken inschakelen.

Vereisten en belangrijke aspecten van VNet peering:

- De twee virtuele netwerken die zijn peered moet in dezelfde Azure regio.
- Het virtuele netwerken die zijn peered moeten niet-overlappende IP-adres spaties hebben.
- Tussen twee virtuele netwerken VNet peering is en er is geen afgeleide overgankelijke relatie. Bijvoorbeeld als virtueel netwerk A is peered met virtuele netwerk B en als virtueel netwerk B is peered met virtuele netwerk C, deze niet wordt omgezet naar virtuele netwerk een wordt peered met virtuele netwerk C.
- Peering kunnen tussen virtuele netwerken in twee verschillende abonnementen worden gemaakt terwijl lang een gebruiker met bevoegdheden van beide abonnementen geautoriseerd de peering en de abonnementen gekoppeld aan de dezelfde Active Directory-tenant zijn. 
- Peering tussen virtual netwerk in resource manager model en implementatiemodel klassieke is vereist dat de VNets in hetzelfde abonnement worden moet.
- Een virtueel netwerk dat het resourcemanager implementatie-objectmodel gebruikt met een andere virtuele netwerk dat gebruikmaakt van dit model kan worden peered of met een virtueel netwerk die gebruikmaakt van het implementatiemodel klassieke. Virtuele netwerken die gebruikmaken van het implementatiemodel klassieke niet kunnen echter worden peered aan elkaar.
- Hoewel de communicatie tussen virtuele machines in peered virtuele netwerken geen beperkingen extra bandbreedte heeft, bandbreedte initiaal op basis van grootte VM nog steeds is van toepassing.


![Eenvoudige VNet peering](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Connectiviteit
Nadat er zijn twee virtuele netwerken peered, een virtuele machine (web/werknemer rol) in het virtuele netwerk kunt rechtstreeks verbinding maken met andere virtuele machines in het peered virtuele netwerk. Deze twee netwerken hebt volledige IP-niveau connectivity.

De netwerklatentie voor een retour tussen twee virtuele machines in peered virtuele netwerken is hetzelfde als voor een retour binnen een lokale virtuele netwerk. De netwerkdoorvoer is gebaseerd op de bandbreedte dat toegestaan voor de virtuele machine proportionele om het formaat. Er is geen extra beperkingen gelden voor de bandbreedte.

Het verkeer tussen de virtuele machines in peered virtuele netwerken is gerouteerd via de Azure back-enddatabase-infrastructuur en niet via een gateway.

Virtuele machines in een virtueel netwerk hebben toegang tot de interne taakverdeling (ILB) eindpunten in het peered virtuele netwerk. Beveiligingsgroepen netwerk (NSGs) kunnen worden toegepast in een virtueel netwerk toegang tot andere virtuele netwerken of subnetten blokkeren desgewenst.

Als gebruikers peering configureert, kunnen ze openen of sluiten van de regels NSG tussen de virtuele netwerken. Als de gebruiker te openen volledige connectiviteit tussen peered virtuele netwerken (dit is de standaardoptie), kunt ze NSGs op specifieke subnetten of virtuele machines blokkeren of specifieke toegang weigeren.

Azure-voorwaarde interne DNS-naamresolutie voor virtuele machines werkt niet in peered virtuele netwerken. Virtuele machines hebben interne DNS-namen die omgezet alleen binnen het lokale virtuele netwerk worden. Gebruikers kunnen echter virtuele machines die worden uitgevoerd in peered virtuele netwerken als de DNS-servers voor een virtueel netwerk configureren.

## <a name="service-chaining"></a>Service koppelen
Zoals in het diagram verderop in dit artikel, kunnen gebruikers de gebruiker gedefinieerde routetabellen met naar virtuele machines in peered virtuele netwerken als de "volgende hop" IP-adres verwijzingen configureren. Hiermee worden gebruikers om te bereiken service koppelen, waarmee ze kunnen-verkeer van het ene virtuele netwerk op een virtuele toestel waarop wordt uitgevoerd in een netwerk met een peered virtuele door gebruiker gedefinieerde routetabellen.

Gebruikers kunnen ook effectief hub-en-spaak type omgevingen waar de hub onderdelen van de infrastructuur zoals een virtuele toestel netwerk kunt hosten maken. Alle spaak virtuele netwerken kunnen klikt u vervolgens op hetzelfde niveau met, evenals een subset van verkeer naar apparaten die worden uitgevoerd in de hub virtueel netwerk. Kortom, het volgende hop IP-adres op het 'door gebruiker gedefinieerde tabel' VNet peering kunt naar het IP-adres van een virtuele machine in het peered virtuele netwerk.

## <a name="gateways-and-on-premises-connectivity"></a>Gateways en on-premises-connectiviteit
Elke virtueel netwerk, ongeacht of deze is peered met een ander virtuele netwerk, kan nog steeds een eigen gateway hebt en deze gebruiken om te verbinden met on-premises implementatie. Gebruikers kunnen ook [VNet-naar-VNet verbindingen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) configureren met behulp van de gateways, hoewel de virtuele netwerken zijn peered.

Wanneer beide opties voor virtueel netwerk interconnectiviteit zijn geconfigureerd, wordt het verkeer tussen de virtuele netwerken doorloopt tot en met de peering configuratie (dat wil zeggen tot en met de Azure backbone).

Wanneer u virtuele netwerken zijn peered, kunnen gebruikers ook configureren de gateway in het peered virtuele netwerk als een overdracht die wijst naar de on-premises implementatie. Het virtuele netwerk waarin een externe gateway wordt gebruikt, kan niet in dit geval een eigen gateway hebben. Een virtueel netwerk kunt slechts één gateway hebben. Dit zijn een lokale gateway of een externe gateway (in het peered virtuele netwerk), zoals wordt weergegeven in de volgende afbeelding.

Gateway-overdracht niet wordt ondersteund in de peering relatie tussen virtuele netwerken met het model resourcemanager en de gebruikers van het implementatiemodel klassieke. Beide virtuele netwerken in de relatie peering moeten het implementatiemodel resourcemanager voor de overdracht van een gateway gebruiken om te werken.

Wanneer de virtuele netwerken die één Azure ExpressRoute verbinding delen zijn peered, wordt het verkeer ertussen gaat via de peering relatie (dat wil zeggen via het backbonenetwerk Azure). Gebruikers kunnen nog steeds lokale gateways gebruiken in elke virtuele netwerk verbinding maken met de on-premises implementatie circuitlijnen. U kunt ook kunnen een gedeelde gateway gebruiken en de overdracht voor on-premises implementatie-connectiviteit configureren.

![VNet peering overdracht](./media/virtual-networks-peering-overview/figure02.png)

## <a name="provisioning"></a>Inrichten
VNet peering is een bevoegdheden bewerking. Dit is een afzonderlijke functie onder de naamruimte VirtualNetworks. Een gebruiker kan specifieke machtigen voor het Autoriseer peering worden opgegeven. Een gebruiker die alleen-lezen-schrijven toegang tot het virtuele netwerk heeft krijgt automatisch deze rechten.

Een peering bewerking op een andere VNet kan worden geïnitieerd door een gebruiker aan wie u een beheerder is of een gebruiker met bevoegdheden van de peering mogelijkheid. Als er een overeenkomende aanvraag voor peering aan de andere kant en overige voorwaarden is voldaan, de peering wordt tot stand worden gebracht.

Raadpleeg de artikelen in de sectie 'Volgende stappen' voor meer informatie over het tot stand brengen VNet peering tussen twee virtuele netwerken.

## <a name="limits"></a>Limieten
Er zijn beperkingen ten aanzien van het aantal peerings dat is toegestaan voor één virtuele netwerk. Raadpleeg [Azure netwerken limieten](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.

## <a name="pricing"></a>Prijzen
VNet peering is gratis gedurende de periode controleren. Nadat deze is uitgebracht, wordt er een nominale boete op ingress- en egress verkeer die gebruikmaakt van de peering. Raadpleeg de [prijzen van de pagina](https://azure.microsoft.com/pricing/details/virtual-network)voor meer informatie.


## <a name="next-steps"></a>Volgende stappen
- [Peering tussen virtuele netwerken instellen](virtual-networks-create-vnetpeering-arm-portal.md).
- Meer informatie over [NSGs](virtual-networks-nsg.md).
- Meer informatie over [de gebruiker gedefinieerde routes en IP-doorsturen](virtual-networks-udr-overview.md).
