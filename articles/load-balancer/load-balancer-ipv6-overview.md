<properties
    pageTitle="Overzicht van IPv6 voor Azure taakverdeling | Microsoft Azure"
    description="Informatie over IPv6-ondersteuning voor Azure taakverdeling en VMs verdeeld."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="IPv6, azure taakverdeling, dubbele stapel, openbare IP-, systeemeigen ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Overzicht van IPv6 voor Azure taakverdeling

Internetgerichte netwerktaakverdelers kunnen worden geïmplementeerd met een IPv6-adres. Naast het IPv4-connectiviteit Hierdoor kunnen de volgende mogelijkheden:

* Oorspronkelijke end-to-end IPv6-connectiviteit tussen openbare Internet-clients en Azure virtuele Machines (VMs) tot en met de taakverdeling.
* Systeemeigen end-to-end IPv6 uitgaande verbinding tussen VMs en clients van openbare Internet IPv6-ondersteuning.

De volgende afbeelding ziet u de functionaliteit voor IPv6 voor taakverdeling Azure.

![Azure taakverdeling met IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Zodra geïmplementeerd, kan een IPv4- of Internet IPv6 ingeschakelde client van de verdeling van de Azure internetgerichte belasting communiceren met de openbare IPv4 of IPv6-adressen (of hostnamen). De taakverdeling stuurt de IPv6-pakketten naar de persoonlijke IPv6-adressen van de VMs met NAT (Translation). De client IPv6-Internet communiceren niet rechtstreeks met de IPv6-adres van de VMs.

## <a name="features"></a>Functies

Systeemeigen IPv6-ondersteuning voor VMs geïmplementeerd via Azure resourcemanager bevat:

1. IPv6-services taakverdeling voor IPv6-clients op Internet
2. Systeemeigen IPv6 en IPv4 eindpunten op VMs ("twee gestapeld")
3. Binnenkomende en uitgaande door de geactiveerde systeemeigen IPv6-verbindingen
4. Ondersteunde protocollen zoals TCP, UDP en HTTP (S) inschakelen een uitgebreide reeks bureaubladservices

## <a name="benefits"></a>Voordelen

Deze functionaliteit kunt de volgende belangrijke voordelen:

* Vereisen dat nieuwe toepassingen toegankelijk wordt gemaakt voor alleen IPv6-clients zijn vergaderen
* Inschakelen mobile en Internet van dingen (IOT) ontwikkelaars kunt twee gestapeld (IPv4 + IPv6) Azure virtuele Machines gebruiken voor de groeiende mobile & IOT markten

## <a name="details-and-limitations"></a>Details en beperkingen

Meer informatie

* De DNS-Azure-service zowel IPv4-A en IPv6 AAAA naamrecords bevat en reageert met beide records voor de taakverdeling. De client kiest welk adres (IPv4 of IPv6) om te communiceren met.
* Wanneer een VM een verbinding met een openbaar Internet IPv6-verbonden apparaat gestart, is van de VM bron IPv6-adres netwerkadres omgezet (NAT) naar de openbare IPv6-adres van de taakverdeling.
* VMs waarop het besturingssysteem Linux moeten worden geconfigureerd een IPv6 IP-adres via DHCP ontvangen. Veel van de afbeeldingen Linux in de galerie met Azure zijn al geconfigureerd voor ondersteuning biedt voor IPv6 ongewijzigd. Zie voor meer informatie [DHCPv6 configureren voor Linux VMs](load-balancer-ipv6-for-linux.md)
* Als u besluit om een test systeemstatus gebruiken met uw taakverdeling, een IPv4-test maken en dit product gebruiken met de IPv4 en IPv6-eindpunten. Als de service op uw VM uitvalt, kan de IPv4 en IPv6 eindpunten uit draaiing worden gehaald.

Beperkingen

* U kunt geen IPv6 laden taakverdeling regels toevoegen in de portal van Azure. De regels kunnen alleen worden gemaakt door de sjabloon, CLI, PowerShell.
* U kunt bestaande VMs voor het gebruik van IPv6-adressen voor mogelijk niet bijwerken. U moet nieuwe VMs implementeren.
* Een enkel IPv6-adres kan worden toegewezen aan een interface één netwerk in elke VM.
* De openbare IPv6-adressen worden niet toegewezen aan een VM. Ze kunnen alleen worden toegewezen aan een taakverdeling.
* U kunt het omgekeerde DNS-zoekopdracht niet configureren voor uw openbare IPv6-adressen.
* De VMs met de IPv6-adressen mogen niet leden van een Azure-Cloudservice. Ze kunnen worden verbonden met een Azure virtuele netwerk (VNet) en met elkaar communiceren via hun IPv4-adressen.
* Privé IPv6-adressen kunnen worden geïmplementeerd op afzonderlijke VMs in een resourcegroep, maar kunnen niet worden geïmplementeerd in een resourcegroep via schaal Sets.
* Azure VMs kunnen geen verbinding maken via IPv6 andere VMs, andere Azure services of on-premises implementatie-apparaten. Ze kunnen alleen met de Azure taakverdeling communiceren via IPv6. Ze kunnen echter communiceren met de volgende andere informatiebronnen met IPv4.
* Beveiliging van de beveiligingsgroep netwerk (NSG) voor IPv4 wordt ondersteund in twee-stack (IPv4 + IPv6)-implementaties. NSGs worden niet toegepast op de IPv6-eindpunten.
* Het eindpunt IPv6 op de VM is niet rechtstreeks blootgesteld aan internet. Het is achter een taakverdeling. Alleen de poorten in de regels van de verdeling van laden zijn toegankelijk via IPv6.
* Wijzigen van de parameter IdleTimeout voor IPv6 is **momenteel niet ondersteund**. De standaardinstelling is vier minuten.

## <a name="next-steps"></a>Volgende stappen

Leer hoe u een taakverdeling met IPv6 implementeren.

* [Beschikbaarheid van IPv6 per regio](https://go.microsoft.com/fwlink/?linkid=828357)
* [Een taakverdeling met IPv6 met een sjabloon implementeren](load-balancer-ipv6-internet-template.md)
* [Een taakverdeling met IPv6 via Azure PowerShell implementeren](load-balancer-ipv6-internet-ps.md)
* [Een taakverdeling met IPv6 met Azure CLI implementeren](load-balancer-ipv6-internet-cli.md)
