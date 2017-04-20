<properties
   pageTitle="Prestaties verkeer routeren methode configureren | Microsoft Azure"
   description="In dit artikel kunt u prestaties verkeer routeren methode in verkeer Manager configureren"
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

# <a name="configure-performance-traffic-routing-method"></a>Prestaties verkeer routeren methode configureren

Om te routeren verkeer naar cloudservices en websites (eindpunten) die in verschillende datacenters overal ter wereld (ook wel bekend als regio's bevinden zich), kunt u binnenkomende verkeer naar het eindpunt met de laagste latentie verwijzen vanuit de client. Meestal het datacenter met de laagste latentie overeenkomt met het dichtst in geografische afstand. De prestaties verkeer routeren methode kunt u distribueren op basis van het laagste latentie, maar kan geen rekening met realtime Accountwijzigingen in netwerkconfiguratie of laden. Zie [over verkeer Manager verkeer routering methoden](traffic-manager-routing-methods.md)voor meer informatie over de verschillende verkeer routeren methoden van Azure verkeer Manager.

## <a name="route-traffic-based-on-lowest-latency-across-a-set-of-endpoints"></a>Route-verkeer is toegestaan op basis van de laagste latentie van een set eindpunten:

1. Klik op het pictogram **Verkeer Manager** u opent het deelvenster verkeer Manager in de Azure klassieke portal, in het linkerdeelvenster. Als u uw profiel verkeer Manager nog niet hebt gemaakt, raadpleegt u [Verkeer Manager profielen beheren](traffic-manager-manage-profiles.md) voor de stappen om een eenvoudige verkeer Manager profiel te maken.
2. Klik in de Azure klassieke portal naar het deelvenster verkeer Manager en zoek het verkeer Manager-profiel waarin de instellingen die u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van een profiel. Hiermee wordt de instellingenpagina voor het profiel geopend.
3. Klik op de pagina voor uw profiel op **eindpunten** boven aan de pagina en controleer of de eindpunten van de service die u wilt opnemen in uw configuratie aanwezig zijn. Zie [Beheren eindpunten in verkeer Manager](traffic-manager-endpoints.md)toevoegen of verwijderen van de eindpunten uit uw profiel.
4. Klik op **configureren** Klik boven aan de configuratiepagina openen op de pagina voor uw profiel.
5. Voor **verkeer routeren methode instellingen**, controleert u of de methode verkeer rondsturen * *prestaties*. Als dit niet het geval is, klikt u op * *prestaties** in de vervolgkeuzelijst.
6. Controleer of de **Controle-instellingen** correct zijn geconfigureerd. Cmdlets voor controle zorgt ervoor dat eindpunten die offline zijn niet verkeer worden verzonden. Wilt u controleren eindpunten, moet u een pad en de bestandsnaam opgeven. Opmerking die doorgestuurd schuine streep "/" geldige invoer voor het relatieve pad en houdt in dat het bestand zich in de hoofdmap (standaard). Zie de [Over verkeer Manager controleren](traffic-manager-monitoring.md)voor meer informatie over het controleren.
7. Nadat u uw configuratiewijzigingen hebt voltooid, klikt u op **Opslaan** onder aan de pagina.
8. Test de wijzigingen in uw configuratie. Zie voor meer informatie [Verkeer Manager-instellingen testen](traffic-manager-testing-settings.md).
9. Als u uw profiel verkeer Manager hebt installatie en werken, bewerk de DNS-record op de gezaghebbende DNS-server de naam van uw bedrijf domein verwijzen naar de naam van het verkeer Manager-domein. Zie voor meer informatie over hoe u dit doet, [punt een bedrijfsdomein Internet naar een domein verkeer Manager](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Volgende stappen


[Een bedrijfsdomein Internet wijst u een domein verkeer Manager](traffic-manager-point-internet-domain.md)

[Methoden voor het doorsturen van verkeer Manager](traffic-manager-routing-methods.md)

[Failover-mailroutering methode configureren](traffic-manager-configure-failover-routing-method.md)

[Round robin routeren methode configureren](traffic-manager-configure-round-robin-routing-method.md)

[Probleemoplossing verkeer Manager niet beschikbaar is weergegeven staat](traffic-manager-troubleshooting-degraded.md)

[Verkeer Manager - uitschakelen, inschakelen of verwijderen van een profiel](disable-enable-or-delete-a-profile.md)

[Een eindpunt in- of uitschakelen voor Manager - verkeer](disable-or-enable-an-endpoint.md)

