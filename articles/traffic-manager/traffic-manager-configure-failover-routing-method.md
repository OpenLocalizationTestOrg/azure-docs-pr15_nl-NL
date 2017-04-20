<properties
   pageTitle="Verkeer Manager failover-verkeer routeren methode configureren | Microsoft Azure"
   description="In dit artikel kunt u failover-verkeer routeren methode in verkeer Manager configureren"
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

# <a name="configure-failover-routing-method"></a>Failover-mailroutering methode configureren

Vaak wil een organisatie betrouwbaarheid voor de services. Dit gebeurt doordat back-services geval hun primaire service uitvalt. Een algemene patroon voor service failover is een reeks van identieke services en het verzenden van verkeer naar een primaire service, terwijl een geconfigureerde lijst met een of meer van de back-services. U kunt dit type back-up configureren met Azure cloudservices en websites door de onderstaande procedures.

Houd er rekening mee dat Azure Websites al vindt u failover verkeer routering methode functionaliteit voor websites binnen een datacenter (ook wel bekend als een regio), ongeacht de website-modus. Verkeer Manager kunt u opgeven failover-verkeer routeren methode voor websites in verschillende datacenters.

## <a name="to-configure-failover-traffic-routing-method"></a>Failover-verkeer routeren methode configureren:

1. Klik op het pictogram **Verkeer Manager** u opent het deelvenster verkeer Manager in de Azure klassieke portal, in het linkerdeelvenster. Als u uw profiel verkeer Manager nog niet hebt gemaakt, raadpleegt u [Verkeer Manager profielen beheren](traffic-manager-manage-profiles.md) voor stapsgewijze instructies om een eenvoudige verkeer Manager-profiel te maken.
2. Naar het deelvenster verkeer Manager in de portal van Azure klassieke en zoek het verkeer Manager-profiel waarin de instellingen die u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van een profiel. Hiermee wordt de instellingenpagina voor het profiel geopend.
3. Klik op de profielpagina op **eindpunten** boven aan de pagina en verifiëren dat de beide cloud services en websites (eindpunten) die u wilt opnemen in uw configuratie aanwezig zijn. Zie voor de stappen voor het toevoegen of verwijderen van de eindpunten, [Beheren eindpunten in verkeer Manager](traffic-manager-endpoints.md).
4. Klik op **configureren** Klik boven aan de configuratiepagina openen op uw profielpagina.
5. Controleer of dat de methode verkeer rondsturen **overname is**voor **verkeer routeren methode-instellingen**. Als dit niet het geval is, klikt u op **Failover** in de vervolgkeuzelijst.
6. Pas de failover voor uw eindpunten voor **De lijst prioriteit Failover**. Wanneer u de methode **Failover** verkeer rondsturen selecteert, wordt de volgorde van de geselecteerde eindpunten het belang is. Het primaire eindpunt bevindt zich bovenaan. Gebruik de pijl omhoog en pijl-omlaag om de volgorde naar wens. Zie [Set-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880)voor informatie over het instellen van de prioriteit failover met Windows PowerShell.
7. Controleer of de **Controle-instellingen** correct zijn geconfigureerd. Cmdlets voor controle zorgt ervoor dat eindpunten die offline zijn niet verkeer worden verzonden. Wilt u controleren eindpunten, moet u een pad en de bestandsnaam opgeven. Opmerking die doorgestuurd schuine streep "/" geldige invoer voor het relatieve pad en houdt in dat het bestand zich in de hoofdmap (standaard). Zie voor meer informatie over het controleren van [Verkeer Manager controle](traffic-manager-monitoring.md).
8. Nadat u uw configuratiewijzigingen hebt voltooid, klikt u op **Opslaan** onder aan de pagina.
9. Test de wijzigingen in uw configuratie. Zie [Verkeer Manager-instellingen testen](traffic-manager-testing-settings.md) voor meer informatie.
10. Als u uw profiel verkeer Manager hebt installatie en werken, bewerk de DNS-record op de gezaghebbende DNS-server de naam van uw bedrijf domein verwijzen naar de naam van het verkeer Manager-domein. Zie voor meer informatie over hoe u dit doet, [punt een bedrijfsdomein Internet naar een domein verkeer Manager](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Volgende stappen

[Een bedrijfsdomein Internet wijst u een domein verkeer Manager](traffic-manager-point-internet-domain.md)

[Methoden voor het doorsturen van verkeer Manager](traffic-manager-routing-methods.md)

[Round robin routeren methode configureren](traffic-manager-configure-round-robin-routing-method.md)

[Prestaties routeren methode configureren](traffic-manager-configure-performance-routing-method.md)

[Probleemoplossing verkeer Manager niet beschikbaar is weergegeven staat](traffic-manager-troubleshooting-degraded.md)

[Verkeer Manager - uitschakelen, inschakelen of verwijderen van een profiel](disable-enable-or-delete-a-profile.md)

[Een eindpunt in- of uitschakelen voor Manager - verkeer](disable-or-enable-an-endpoint.md)

