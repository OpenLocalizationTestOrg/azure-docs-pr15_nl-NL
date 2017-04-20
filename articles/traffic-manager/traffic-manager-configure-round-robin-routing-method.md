<properties
   pageTitle="Het configureren van verkeer Manager afronden robin verkeer routeren methode | Microsoft Azure"
   description="In dit artikel kunt u round robin van taakverdeling voor de eindpunten van uw Manager verkeer configureren."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-round-robin-routing-method"></a>Methode configureren Round Robin

Een patroon algemene verkeer routeren methode is een reeks identieke eindpunten, waaronder cloudservices en websites, bieden en verkeer naar elk label in een manier round robin verzenden. De onderstaande stappen wordt uitgelegd verkeer Manager configureren om te kunnen uitvoeren van dit type verkeer routeren methode. Zie voor meer informatie over de methoden voor het doorsturen van verschillende verkeer, [over verkeer Manager verkeer routeren methoden](traffic-manager-routing-methods.md).

>[AZURE.NOTE] Azure Websites biedt al round robin van taakverdeling functionaliteit voor websites binnen een datacenter (ook wel bekend als een regio). Verkeer Manager kunt u opgeven round robin verkeer routeren methode voor websites in verschillende datacenters.

## <a name="routing-traffic-equally-round-robin-across-a-set-of-endpoints"></a>Routering verkeer gelijkmatig (ronde robin) voor een reeks eindpunten:

1. Klik op het pictogram **Verkeer Manager** u opent het deelvenster verkeer Manager in de Azure klassieke portal, in het linkerdeelvenster. Als u uw profiel verkeer Manager nog niet hebt gemaakt, raadpleegt u [Verkeer Manager profielen beheren](traffic-manager-manage-profiles.md) voor stapsgewijze instructies om een eenvoudige verkeer Manager-profiel te maken.
2. Klik in de Azure klassieke portal naar het deelvenster verkeer Manager en zoek het verkeer Manager-profiel waarin de instellingen die u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van een profiel. Hiermee wordt de instellingenpagina voor het profiel geopend.
3. Klik op de pagina voor uw profiel op **eindpunten** boven aan de pagina en controleer of de eindpunten van de service die u wilt opnemen in uw configuratie aanwezig zijn. Zie voor de stappen voor het toevoegen of verwijderen van de eindpunten, [Beheren eindpunten in verkeer Manager](traffic-manager-endpoints.md).
4. Klik op **configureren** Klik boven aan de configuratiepagina openen op uw profielpagina.
5. Voor **verkeer routeren methode instellingen**, controleert u of de methode verkeer rondsturen **Round Robin**. Als dit niet het geval is, klikt u **Round Robin** in de vervolgkeuzelijst worden weergegeven.
6. Controleer of de **Controle-instellingen** correct zijn geconfigureerd. Cmdlets voor controle zorgt ervoor dat eindpunten die offline zijn niet verkeer worden verzonden. Wilt u controleren eindpunten, moet u een pad en de bestandsnaam opgeven. Opmerking die doorgestuurd schuine streep "/" geldige invoer voor het relatieve pad en houdt in dat het bestand zich in de hoofdmap (standaard). Zie de [Over verkeer Manager controleren](traffic-manager-monitoring.md)voor meer informatie over het controleren.
7. Nadat u uw configuratiewijzigingen hebt voltooid, klikt u op **Opslaan** onder aan de pagina.
8. Test de wijzigingen in uw configuratie. Zie voor meer informatie [Verkeer Manager-instellingen testen](traffic-manager-testing-settings.md).
9. Als u uw profiel verkeer Manager hebt installatie en werken, bewerk de DNS-record op de gezaghebbende DNS-server de naam van uw bedrijf domein verwijzen naar de naam van het verkeer Manager-domein. Zie voor meer informatie over hoe u dit doet, [punt een bedrijfsdomein Internet naar een domein verkeer Manager](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Volgende stappen


[Een bedrijfsdomein Internet wijst u een domein verkeer Manager](traffic-manager-point-internet-domain.md)

[Methoden voor het doorsturen van verkeer Manager](traffic-manager-routing-methods.md)

[Failover-mailroutering methode configureren](traffic-manager-configure-failover-routing-method.md)

[Prestaties routeren methode configureren](traffic-manager-configure-performance-routing-method.md)

[Probleemoplossing verkeer Manager niet beschikbaar is weergegeven staat](traffic-manager-troubleshooting-degraded.md)

[Verkeer Manager - uitschakelen, inschakelen of verwijderen van een profiel](disable-enable-or-delete-a-profile.md)

[Een eindpunt in- of uitschakelen voor Manager - verkeer](disable-or-enable-an-endpoint.md)

