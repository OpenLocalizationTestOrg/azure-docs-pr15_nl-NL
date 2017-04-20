<properties
   pageTitle="Meerdere VIP's voor Azure belasting voor documentconversies | Microsoft Azure"
   description="Overzicht van meerdere VIP's op Azure taakverdeling"
   services="load-balancer"
   documentationCenter="na"
   authors="chkuhtz"
   manager="narayan"
   editor=""
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="chkuhtz"
/>

# <a name="multiple-vips-for-azure-load-balancer"></a>Meerdere VIP's voor Azure belasting voor documentconversies

Azure taakverdeling kunt u laden balance '-services op meerdere poorten, meerdere IP-adressen of beide. U kunt definities van de verdeling van interne en externe openbare laden balance '-loopt van een set VMs laden.

In dit artikel worden de grondbeginselen van deze mogelijkheid, belangrijke concepten getoond en beperkingen. Als u alleen laten zien services op één IP-adres wilt, kunt u vinden vereenvoudigd instructies voor [openbare](load-balancer-get-started-internet-portal.md) of [interne](load-balancer-get-started-ilb-arm-portal.md) verdeling configuraties laden. Voor het toevoegen van meerdere VIP's is incrementele aan een enkele VIP-configuratie. De concepten in dit artikel gebruikt, kunt u een eenvoudigere configuratie op elk gewenst moment uitbreiden.

Wanneer u een taakverdeling Azure definieert, zijn een frontend en een back-end-configuratie verbonden met regels. De status-test waarnaar wordt verwezen door de regel wordt gebruikt om te bepalen hoe nieuwe loopt worden verzonden naar een knooppunt in de backend-toepassingen. De frontend is gedefinieerd door een virtueel IP-(VIP), dat wil een 3-tupel bestaat uit een IP-adres (openbaar of interne), een transportprotocol (UDP of TCP) en een poortnummer zeggen. Een DIP is een IP-adres op een Azure virtuele NIC die zijn bijgevoegd bij een VM in de backend-groep.

De volgende tabel bevat enkele voorbeeld frontend configuraties:

| VIP | IP-adres | Protocol | poort |
|-----|------------|----------|------|
|1|65.52.0.1|TCP|80|
|2|65.52.0.1|TCP|_8080_|
|3|65.52.0.1|_UDP_|80|
|4|_65.52.0.2_|TCP|80|

De tabel bevat vier verschillende frontends. Frontends #1, 2 # en #3 zijn een enkel VIP met meerdere regels. Hetzelfde IP-adres wordt gebruikt, maar de poort of het protocol voor elke frontend anders is. Frontends #1 en #4 zijn in een voorbeeld van meerdere VIP's, waarin de hetzelfde frontend protocol en poort hele meerdere VIP's worden gebruikt.

Azure taakverdeling biedt flexibiliteit bij het definiëren van de regels van taakverdeling. Een regel wordt gedefinieerd hoe een adres en poort op de frontend is toegewezen aan het doeladres en poort op de backend. Al dan niet backend-poorten in de hele regels worden gebruikt, is afhankelijk van het type van de regel. Elk type regel heeft over de vereisten die invloed kunnen zijn op de host configuratie en test-ontwerp. Er zijn twee soorten regels:

1. De standaardregel met geen backend poort opnieuw gebruiken
2. De zwevende IP-regel waar de backend-poorten opnieuw worden gebruikt

Azure taakverdeling kunt u mengt u beide regeltypen van de configuratie voor de verdeling van dezelfde laden. De taakverdeling kunt ze gebruiken tegelijk voor een bepaald VM of een combinatie daarvan, zo lang maken als u de beperkingen van de regel naleven. Welk regeltype die u kiest, is afhankelijk van de vereisten van uw toepassing en de complexiteit van het ondersteunende die configuratie. U moet evalueren welke regeltypen meest geschikt zijn voor uw scenario.

We verkennen deze scenario's verder door te beginnen met het standaardgedrag.

## <a name="rule-type-1-no-backend-port-reuse"></a>Regeltype #1: geen backend poort opnieuw gebruiken

![Afbeelding van MultiVIP](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

In dit scenario zijn de frontend VIP's als volgt geconfigureerd:

| VIP | IP-adres | Protocol | poort |
|-----|------------|----------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

De DIP is het doel van de binnenkomende stroom. In de groep backend beschrijft elke VM de gewenste service op een unieke poort op een DIP. Deze service is gekoppeld aan de frontend tot en met de regeldefinitie van een.

We moet u twee regels definiëren:

| Regel | Frontend toewijzen | Naar backend-groep |
|------|--------------|-----------------|
| 1 | ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 | ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

Het volledige toewijzen in Azure taakverdeling is nu als volgt:

| Regel | VIP IP-adres | Protocol | poort | Bestemming | poort |
|------|----------------|----------|------|-----|------|
|![regel](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|DIP IP-adres|80|
|![regel](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|DIP IP-adres|81|

Elke regel, moet een stroom met een unieke combinatie van IP-adres en doelpoort opleveren. Variërende de doelpoort van de stroom, kunnen meerdere regels loopt leveren aan de dezelfde DIP op verschillende poorten.

Servicestatus sondes worden altijd doorgestuurd naar de DIP van een VM. U moet u zorgen dat uw test de status van de VM weerspiegelt.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Regeltype #2: backend poort opnieuw gebruiken met behulp van zwevende IP-

Azure taakverdeling biedt de flexibiliteit om opnieuw gebruiken de poort frontend over meerdere VIP's ongeacht het regeltype gebruikt. Daarnaast kunnen voorbeelden van de toepassing liever of dezelfde poort moet worden gebruikt door meerdere toepassingsexemplaren op een enkele VM in de groep backend vereisen. Algemene voorbeelden van poort hergebruik zijn cluster beschikbaarheid, virtuele toestellen en meerdere TLS eindpunten zonder opnieuw codering weergeeft.

Als u hergebruiken van de backend-poort over meerdere regels wilt, moet u zwevende IP-inschakelen in de regeldefinitie.

Zwevende IP is een deel van wat is het bekend als directe Server terug (DSA). DSA bestaat uit twee delen: de topologie van een stroom en een IP-toewijzing kleurenschema. Op een niveau platform werkt Azure taakverdeling altijd in een topologie van de stroom DSA ongeacht of zwevend IP- of niet is ingeschakeld. Dit betekent dat het uitgaande deel van een stroom altijd juist is herschreven om rechtstreeks flow terug naar de oorsprong.

Met het standaardtype regel beschrijft Azure een traditionele taakverdeling IP-adres schema voor gebruiksgemak. Het kleurenschema van het IP-adres toewijzing toe te staan dat voor meer flexibiliteit, zoals hieronder wordt uitgelegd inschakelen, zwevende IP worden gewijzigd.

In het volgende diagram ziet u deze configuratie:

![Afbeelding van MultiVIP](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

In dit scenario heeft elke VM in de groep backend drie netwerkinterfaces:

* DIP: een virtuele NIC die is gekoppeld aan de VM (resource van Azure NIC)
* VIP1: een loopback-interface binnen Gast OS die is geconfigureerd met IP-adres van VIP1
* VIP2: een loopback-interface binnen Gast OS die is geconfigureerd met IP-adres van VIP2

>[AZURE.IMPORTANT] De configuratie van de logische interfaces wordt uitgevoerd binnen de Gast OS. Deze configuratie wordt niet uitgevoerd of beheerd door Azure. Zonder deze configuratie wordt werkt de regels niet. Servicestatus test definities gebruikt u de DIP van de VM in plaats van de logische VIP. Uw service dient daarom test antwoorden op een DIP poort die de status van de service aangeboden na te denken over de logische VIP.

Stel dat u dezelfde frontend configuratie zoals in het vorige scenario:

| VIP | IP-adres | Protocol | poort |
|-----|------------|----------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

We moet u twee regels definiëren:

| Regel | Frontend toewijzen | Naar backend-groep |
|------|--------------|-----------------|
| 1 | ![regel](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 (in VM1 en VM2) |
| 2 | ![regel](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 (in VM1 en VM2) |

De volgende tabel ziet u de volledige toewijzing in de taakverdeling:

| Regel | VIP IP-adres | Protocol | poort | Bestemming | poort |
|------|----------------|----------|------|-------------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|komt overeen VIP (65.52.0.1)|komt overeen VIP (80)|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|komt overeen VIP (65.52.0.2)|komt overeen VIP (80)|

De bestemming van de binnenkomende stroom is de VIP-adres op de loopback-interface in VM. Elke regel, moet een stroom met een unieke combinatie van IP-adres en doelpoort opleveren. Door het doeladres van de stroom variërende, is de poort hergebruik mogelijk op de dezelfde VM. Uw service wordt blootgesteld aan de taakverdeling door deze naar het IP-adres en de poort van de desbetreffende loopback-interface van de VIP binden.

Zoals u ziet dat dit voorbeeld wordt de doelpoort niet gewijzigd. Zelfs als dit een zwevende IP-scenario, ondersteunt Azure taakverdeling ook definiëren van een regel naar de backend-doelpoort herschrijven en zodat u deze verschillen van de doelpoort frontend.

Het type zwevende IP-regel is het fundament van verschillende laden verdeling configuratie patronen. Een voorbeeld die momenteel beschikbaar is, is de [SQL-AlwaysOn met meerdere Listeners](../virtual-machines/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) -configuratie. Na verloop van tijd documenteren we meer van deze scenario's.

## <a name="limitations"></a>Beperkingen

* Meerdere VIP configuraties worden alleen ondersteund met IaaS VMs.
* Met de zwevende IP-regel, moet uw toepassing de DIP gebruiken voor uitgaande gegevensstromen. Als uw toepassing aan het VIP-adres voor de loopback-interface in de Gast OS hebt geconfigureerd koppelt, klikt u vervolgens SNAT is niet beschikbaar voor de uitgaande stroom herschrijven en de stroom uitvalt.
* Openbare IP-adressen hebben een effect op facturering. Zie [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses/) voor meer informatie
* Abonnementen toepassen. Zie voor meer informatie [Service limieten](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.
