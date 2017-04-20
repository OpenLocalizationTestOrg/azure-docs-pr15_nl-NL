<properties
    pageTitle="Wat moet u doen in het geval van een Azure service-onderbreking die invloed hebben op Azure virtuele netwerken | Microsoft Azure"
    description="Lees wat u moet doen in het geval van een Azure service-onderbreking die invloed hebben op Azure virtuele netwerken."
    services="virtual-network"
    documentationCenter=""
    authors="NarayanAnnamalai"
    manager="jefco"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="virtual-network"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="narayan;aglick"/>

#<a name="virtual-network--business-continuity"></a>Virtual Network – bedrijfscontinuïteit

##<a name="overview"></a>Overzicht

Een virtueel netwerk (VNet) is een logische weergave van uw netwerk in de cloud. Dit kunt u uw eigen persoonlijke IP-adresruimte problemen definiëren en het netwerk segmenteren in subnetten. VNets fungeert als vertrouwen grens voor het hosten van uw resources berekeningscluster zoals Azure virtuele Machines en Cloudservices (web/werknemer rollen). Een VNet kunt rechtstreekse privé IP-communicatie tussen de resources die in deze worden gehost. Een virtueel netwerk kan ook worden gekoppeld aan een on-premises netwerk via een van de hybride opties zoals een VPN-Gateway of ExpressRoute.
 
Een VNet is in het bereik van een gebied gemaakt. U kunt VNets maken met dezelfde adresruimte in twee verschillende regio's (dat wil zeggen ons Oost en ons West, maar niet kan ze met elkaar verbinden rechtstreeks). 

##<a name="business-continuity"></a>Bedrijfscontinuïteit

Er zijn verschillende manieren dat uw toepassing kan worden onderbroken. Een bepaald gebied kan worden volledig afgekapt vanwege een natuurlijke noodgevallen of een gedeeltelijke noodgevallen vanwege een fout op meerdere apparaten/services. De invloed op de service VNet is er anders in elk van deze situaties.

**V: wat moet u doen in het geval van een storing naar een hele regio? dat wil zeggen als een gebied volledig bereikt vanwege een natuurlijke noodgevallen is? Wat gebeurt er met de virtuele netwerken die worden gehost in de regio?**

A: het virtuele netwerk- en de resources in de desbetreffende regio blijft tijdens het tijdstip waarop de service-onderbreking niet toegankelijk.

![Eenvoudige virtuele netwerkdiagram](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**V: wat kan ik doen hetzelfde Virtual netwerk in een andere regio opnieuw maken?**

A: virtual Network (VNet) is vrij lightweight bron. U kunt aanroepen Azure-API's als u wilt maken van een VNet met dezelfde adresruimte in een andere regio. Opnieuw maken dezelfde omgeving waarin in de desbetreffende regio bevatte, moet u API-gesprekken uw Cloudservices (web/werknemer rollen) en virtuele Machines die u had opnieuw te implementeren. U moet ook te draaien van een Gateway VPN-verbinding maken met uw on-premises netwerk als er on-premises implementatie-connectiviteit (bijvoorbeeld in een hybride implementatie).

De instructies voor het maken van een VNet zijn gevonden [hier](./virtual-networks-create-vnet-arm-pportal.md). 

**V: kan een replica van een VNet in een bepaald gebied niet opnieuw worden gemaakt in een andere regio tijd vooraf?**

A: Ja, kunt u twee VNets de dezelfde privé IP-adresruimte en middelen gebruiken in twee verschillende regio's tijd vooraf maken. Als een klant internet die tegenover elkaar liggen services in de VNet host is, kunnen hebben ze verkeer Manager instellen voor geografische-route-verkeer naar het gebied dat actief is. Echter een klant kan geen verbinding maken twee VNets met dezelfde adresruimte naar hun on-premises netwerk zoals treedt er een routeren problemen. Op het moment van een noodgevallen en verlies van een VNet in één regio, een klant verbinding kunt maken de andere VNet in de regio beschikbaar met identieke adresruimte naar hun on-premises netwerk.

De instructies voor het maken van een VNet zijn gevonden [hier](./virtual-networks-create-vnet-arm-pportal.md).
