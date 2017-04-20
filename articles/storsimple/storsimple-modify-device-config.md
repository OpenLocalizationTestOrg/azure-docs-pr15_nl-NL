<properties 
   pageTitle="Wijzigen van de configuratie van het StorSimple-apparaat | Microsoft Azure" 
   description="Wordt beschreven hoe de service StorSimple Manager gebruiken om te configureren, een StorSimple apparaat die al is geïmplementeerd." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="09/29/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-modify-your-storsimple-device-configuration"></a>Gebruik van de StorSimple Manager-service voor het wijzigen van de configuratie van uw StorSimple-apparaten

## <a name="overview"></a>Overzicht 

De Azure klassieke portal pagina **configureren** bevat alle apparaatparameters die u kunt opnieuw configureren op een apparaat StorSimple dat wordt beheerd door een StorSimple Manager-service. Deze zelfstudie wordt uitgelegd hoe u kunt de pagina **configureren** de volgende apparaatniveau taken uitvoeren:

- Apparaatinstellingen wijzigen 
- Tijdinstellingen wijzigen 
- DNS-instellingen wijzigen 
- Netwerkinterfaces wijzigen
- Uitwisselen of opnieuw toewijzen van IP-adressen

## <a name="modify-device-settings"></a>Apparaatinstellingen wijzigen

De apparaatinstellingen bevatten de beschrijvende naam van het apparaat en de apparaatbeschrijving.

Een apparaat StorSimple die is gekoppeld aan de StorSimple Manager-service is een standaardnaam toegewezen. De standaardnaam geeft doorgaans het seriële getal van het apparaat. Zo geeft de naam van een standaard-apparaat waarmee 15 tekens bevatten, zoals 8600-SHX0991003G44HT, wordt het volgende:

- **8600** – geeft aan het model.
- **SHX** – wordt aangegeven dat de manufacturing-site.
- **0991003** - geeft aan een bepaald product.
- **G44HT**- de laatste 5 cijfers worden opgehoogd als u wilt maken van unieke seriële getallen. Dit mogelijk niet een opeenvolgende set.

U kunt de portal van Azure klassieke wijzigen van naam van het apparaat en hieraan een unieke, beschrijvende naam van uw keuze. De beschrijvende naam kunt tekens bevatten en kan maximaal 64 tekens lang zijn.

U kunt ook een apparaatbeschrijving opgeven. Een apparaatbeschrijving kunt meestal de eigenaar en de fysieke locatie van het apparaat identificeren. Het beschrijvingsveld moet minder dan 256 tekens bevatten.
 
## <a name="modify-time-settings"></a>Tijdinstellingen wijzigen

Uw apparaat moet tijd om te verifiëren bij uw provider van de cloud-opslag synchroniseren. Selecteer uw tijdzone in de vervolgkeuzelijst en Geef maximaal twee netwerk Time Protocol (NTP)-servers. De primaire NTP-server is vereist en wanneer u Windows PowerShell voor StorSimple gebruiken voor het configureren van uw apparaat is opgegeven. Als uw NTP-server kunt u de standaard Windows Server **time.windows.com** opgeven. U kunt de primaire NTP serverconfiguratie via de portal van Azure klassieke weergeven, maar moet u de Windows PowerShell-interface om deze te wijzigen.

De configuratie van de secundaire NTP-server is optioneel. U kunt de klassieke portal gebruiken voor het configureren van een secundaire NTP-server. 

Tijdens het configureren van de NTP-server, moet u ervoor zorgen dat uw netwerk kan het verkeer NTP voor het doorsturen van uw datacenter met Internet. Wanneer u een openbare NTP-server opgeeft, moet u ervoor zorgen dat uw netwerkfirewalls en andere beveiligingsapparaten zijn geconfigureerd voor het toestaan van NTP verkeer naar en vanuit het externe netwerk. Als bidirectionele NTP verkeer is niet toegestaan, moet u een interne NTP-server (een Windows-domeincontroller biedt deze functie). Als uw apparaat niet kunt tijd synchroniseren, het niet mogelijk om te communiceren met uw provider cloud-opslag.

Een overzicht van openbare NTP-servers, gaat u naar de [NTP Servers Web](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Wat gebeurt er als het apparaat wordt geïmplementeerd in een andere tijdzone?

Als het apparaat dat is geïmplementeerd in een andere tijdzone, wordt de tijdzone van het apparaat wordt gewijzigd. Gezien het feit dat het back-beleid de tijdzone apparaat gebruikt, wordt het back-beleid wordt automatisch aangepast overeenkomstig de nieuwe tijdzone. Zonder tussenkomst is vereist.

## <a name="modify-dns-settings"></a>DNS-instellingen wijzigen

Een DNS-server wordt gebruikt als uw apparaat probeert te communiceren met uw provider van de cloud-opslag. Beschikbaarheid moet u zowel de primaire en secundaire DNS-servers configureren tijdens de eerste apparaat-implementatie. Als u wilt configureren, het primaire DNS-server, moet u de Windows PowerShell-interface gebruiken op uw apparaat StorSimple.

Als u wilt wijzigen van de secundaire DNS-server, kunt u de Azure klassieke portal.



## <a name="modify-network-interfaces"></a>Netwerkinterfaces wijzigen

Het apparaat heeft zes apparaat netwerkinterfaces, waarvan vier 1 GbE zijn en twee van de plaats waar 10 GbE zijn. Deze interfaces zijn gelabeld als gegevens 0 – gegevens 5. GEGEVENS 0, DATA 1, gegevens 4 en 5 van de gegevens zijn 1 GbE, gegevens 2 en 3 van de gegevens 10 GbE netwerkinterfaces zijn.

**Netwerk Interface-instellingen** configureren voor elk van de interfaces moet worden gebruikt. Om ervoor te zorgen beschikbaarheid, is het raadzaam dat u ten minste twee iSCSI-interfaces en twee cloud ingeschakelde interfaces op uw apparaat hebben. We raden maar is niet vereist dat niet-gebruikte interfaces worden uitgeschakeld.

Als u een van de netwerkinterfaces configureert, moet u een virtuele IP-(VIP) configureren.

GEGEVENS 0 is standaard ingeschakeld cloud. Tijdens het configureren van gegevens 0, moet u ook twee vaste IP-adressen, één voor elke controller configureren. Deze vaste IP-adressen voor apparaten rechtstreeks toegang tot kunnen worden gebruikt en zijn handig tijdens de installatie van updates op het apparaat, of wanneer u toegang hebt tot de controllers voor probleemoplossing.

In StorSimple 8000 reeks Update 1, de routering meetwaarde gegevens 0 is ingesteld op aan de laagste; Als uw apparaat wordt uitgevoerd StorSimple 8000 reeks Update 1, wordt er daarom alle verkeer in de cloud worden gerouteerd via gegevens 0. Maak een notitie hiervan als er meer dan één cloud ingeschakelde netwerkinterface op uw apparaat StorSimple.

>[AZURE.NOTE] De vaste IP-adressen voor de controller worden gebruikt voor het onderhoud van de updates bij het apparaat. De vaste IP-adressen moet dus geschikt en verbinding maken met Internet.

Voor elke netwerk-interface, worden de volgende parameters weergegeven:

- **Snelheid** – niet als u een gebruiker worden geconfigureerd parameter. GEGEVENS 0, DATA 1, gegevens 4 en 5 van de gegevens zijn altijd 1 GbE, gegevens 2 en 3 van de gegevens 10 GbE-interfaces zijn.

     >[AZURE.NOTE] Snelheid en duplex zijn altijd automatisch-gebruikt. Jumboframes worden niet ondersteund.
 
- De **gebruikersinterface staat** – een interface kan worden ingeschakeld of uitgeschakeld. Als ingeschakeld, wordt het apparaat wordt geprobeerd gebruik van de interface. Het is raadzaam dat alleen die interfaces die zijn verbonden met het netwerk en gebruikt is ingeschakeld. Interfaces waarmee u niet werkt uitschakelen.

- **Type interface** – deze parameter, kunt u isoleren iSCSI-verkeer vanuit de cloud opslag-verkeer is toegestaan. Deze parameter is een van de volgende opties:

    - **Cloud ingeschakeld** : wanneer ingeschakeld, het apparaat wordt deze interface gebruiken om te communiceren met de cloud.
    - **iSCSI ingeschakeld** – wanneer ingeschakeld, het apparaat wordt deze interface gebruiken om te communiceren met de host iSCSI.

    Het is raadzaam dat u iSCSI-verkeer vanuit de cloud opslagverkeer isoleren. Bedenk ook dat als uw host zich binnen hetzelfde subnet als uw apparaat, hoeft u niet een gateway; toewijzen echter als uw host zich in een ander subnet dan uw apparaat, moet u een gateway toewijzen.

- **IP-adres** : dit is IPv4 of IPv6 of beide. IPv4 zowel IPv6-adres gezinnen worden ondersteund voor de apparaat netwerk-interfaces. Wanneer u met IPv4, geeft u een 32-bits IP-adres (*xxx.xxx.xxx.xxx*) in stip decimale notatie met punten. Wanneer u IPv6, simpelweg op een voorvoegsel 4 cijfers en een 128-bit adres wordt automatisch gegenereerd voor uw netwerk-interface van het apparaat op basis van die voorvoegsel.

- **Subnet** – Hiermee verwijst naar het subnetmasker en via de Windows PowerShell-interface is geconfigureerd.

- **Gateway** -dit is de standaard-gateway die moet worden gebruikt door deze interface wanneer wordt geprobeerd te communiceren met knooppunten die niet in de ruimte dezelfde IP-adres (subnet). De standaard-gateway moet zich in dezelfde adresruimte (subnet) als de interface IP-adres, afhankelijk van het subnetmasker.

- **Vaste IP-adres** : dit veld is alleen beschikbaar als u de gegevens 0 configureren interface. Voor bewerkingen zoals updates of het oplossen van het apparaat, moet u mogelijk sluit rechtstreeks aan op de controller apparaat. Het vaste IP-adres kan worden gebruikt voor toegang tot het actieve zowel de passieve controller op uw apparaat.

U kunt Controller 0 en 1 Controller opnieuw configureren via het Azure klassieke portal.

>[AZURE.NOTE] 
>
>- Controleer of de snelheid van de gebruikersinterface en duplex op de schakeloptie waarmee de apparaatinterface van elk is aangesloten op om ervoor te zorgen werking. Schakeloptie interfaces moet ofwel Onderhandel over met of worden geconfigureerd voor Gigabit Ethernet (1000 Mbps) en volledige duplex. Interfaces trager of halve dubbelzijdig treden prestatieproblemen.
>
>- Als u wilt minimaliseren onderbrekingen en downtime, is het raadzaam portfast op elk van de schakeloptie-poorten die de iSCSI-netwerk-interface van uw apparaat verbinden in te schakelen. Dit zorgt ervoor dat dat netwerkconnectiviteit sneller bij failover kan worden gemaakt.
 
## <a name="swap-or-reassign-ips"></a>Uitwisselen of opnieuw toewijzen van IP-adressen

Op dit moment als een VIP wordt gebruikt wordt in elke netwerkinterface op de controller uit (door hetzelfde apparaat of een ander apparaat in het netwerk) worden toegewezen, klikt u vervolgens de controller, mislukt via. U moet dus, volgt u de juiste procedure als u bij het wisselen van VIP's voor het netwerk-interface van het apparaat, omdat u een dubbele IP-situatie wilt maken.

De volgende stappen om te wisselen of opnieuw toewijzen van de VIP's naar een of meer van de netwerkinterfaces uitvoeren:

#### <a name="to-reassign-ips"></a>IP-adressen toewijzen

1. Schakel het IP-adres voor beide interfaces.

2. Nadat de IP-adressen zijn uitgeschakeld, moet u de nieuwe IP-adressen toewijzen aan de desbetreffende interfaces.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [configureren van MPIO voor uw apparaat StorSimple](storsimple-configure-mpio-windows-server.md).

- Informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
     
