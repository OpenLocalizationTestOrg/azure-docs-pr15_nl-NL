<properties 
   pageTitle="Releaseopmerkingen StorSimple 8000 reeks Update 2 | Microsoft Azure"
   description="Beschrijft de nieuwe functies, problemen en oplossingen voor StorSimple 8000 reeks Update 2."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-2-release-notes"></a>Releaseopmerkingen StorSimple 8000 reeks Update 2  

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen beschrijven van de nieuwe functies en de kritieke openstaande actie-items aangeven voor StorSimple 8000 reeks Update 2. Ze bevatten ook een lijst met de software StorSimple, stuurprogramma en schijf firmware-updates is opgenomen in deze release. 

Update 2 kan worden toegepast op elk gewenst Release (GA) of een Update 0,1 uitvoeren van Update 1.2 StorSimple-apparaat. De apparaatversie dat is gekoppeld aan Update 2 is 6.3.9600.17673.

Controleer de gegevens in de releaseopmerkingen voordat u de update in uw StorSimple-oplossing implementeert.

>[AZURE.IMPORTANT]
> 
- Het duurt ongeveer 4 tot en met 7 uur deze update (inclusief de Windows-updates) te installeren. 
- Update 2 heeft software, LSI stuurprogramma en SSD firmware-updates.
- Voor nieuwe versies, kan er geen updates direct omdat we een gefaseerd uitrol van de updates doen. Wacht een paar dagen en vervolgens zoeken naar updates opnieuw als deze binnenkort beschikbaar komen.


## <a name="whats-new-in-update-2"></a>Wat is er nieuw in Update 2

Update 2 maakt u kennis met de volgende nieuwe functies.

- **Lokaal vastgemaakt volumes** – zijn In eerdere versies van de reeks StorSimple 8000 blokken gegevens doorverbonden naar de cloud op basis van gebruik. Er is geen manier om ervoor te zorgen dat blokken op lokaal wilt blijven. In Update 2 wanneer u een volume, maakt kunt u aanwijzen een volume lokaal vastgemaakte en primaire gegevens van het volume wordt niet worden doorverbonden naar de cloud. Momentopnamen van lokaal vastgemaakte hoeveelheden nog steeds worden gekopieerd naar de cloud voor back-up zodat de cloud kan worden gebruikt voor gegevens mobiliteit en noodgevallen. Bovendien kunt u het volumetype wijzigen (dat wil zeggen converteren doorverbonden volumes lokaal vastgemaakte volume en converteren lokaal vastgemaakt hoeveelheden die moeten worden doorverbonden). 

- **Verbeteringen in de virtuele apparaat StorSimple** – eerder, de reeks StorSimple 8000 geplaatst het virtuele apparaat als noodgevallen herstel of ontwikkeling/testen oplossing. Er is slechts één model van virtueel apparaat (model 1100). Update 2 maakt u kennis met twee virtueel apparaat modellen: 

     - 8010 (voorheen de 1100) – niet veranderd ten opzichte; een capaciteit van 30 TB heeft en gebruikt Azure standaard opslag.
     - 8020 – heeft een capaciteit van 64 TB en Azure Premium opslag voor verbeterde prestaties wordt gebruikt.

    Er is een enkel VHD voor beide virtueel apparaat-modellen (8010/8020). Wanneer u het virtuele apparaat voor het eerst start, wordt de parameters platform gedetecteerd en is van toepassing de versie van het juiste.

- **Verbeteringen in de netwerkproblemen** – Update 2 bevat de volgende netwerken verbeteringen:

     - Meerdere NIC's kunnen worden ingeschakeld voor de cloud zodat failover optreden kan als een NIC mislukt.
     - Routeren verbeteringen, met vast aan de doelstellingen voor cloud ingeschakeld blokken.
     - Online opnieuw mislukte resources voordat u een failover.
     - Nieuwe waarschuwingen voor services oplost.

- **Verbeteringen in de bijwerken** – In Update 1.2 en eerdere, de reeks StorSimple 8000 werd bijgewerkt via twee kanalen: Windows Update voor cluster, iSCSI, en dergelijke en Microsoft Update voor binaire bestanden en firmware.
    Update 2 gebruikt Microsoft Update voor alle pakketten bijwerken. Dit moet leiden tot minder tijd egaliseren en failovers doen. 

- **Firmware-updates** : de volgende firmware-updates zijn opgenomen:
    - LSI: lsi_sas2.sys productversie 2.00.72.10
    - Alleen SSD (geen updates van de harde schijf): XMGG, XGEG KZ50, F6C2 en VR08

- **Ondersteuning voor proactief** – Update 2 kunt Microsoft om op te halen extra diagnostische informatie van het apparaat. Wanneer het team bewerkingen wordt aangegeven welke apparaten problemen ondervindt, zijn wij beter uitgerust voor het verzamelen van gegevens van het apparaat en een diagnose stellen bij problemen. **Door te accepteren Update 2, kunnen we dit proactief ondersteuning bieden**.    
 

## <a name="issues-fixed-in-update-2"></a>Problemen opgelost met 2

De volgende tabellen wordt een overzicht van problemen die zijn opgelost met Updates 2.    

| Nee. | Functie | Probleem | Dit geldt voor fysiek apparaat | Dit geldt voor virtueel apparaat |
|-----|---------|-------|--------------------------------|--------------------------------|
| 1 | Netwerkinterfaces | Na een upgrade naar Update 1, de StorSimple Manager-service gemeld dat de poorten Data2 en Data3 op één controller is mislukt. Dit probleem is opgelost. | Ja | Nee |
| 2 | Updates | Na een upgrade naar Update 1, hoorbare waarschuwing waarschuwingen opgetreden in de klassieke Azure-portal op meerdere apparaten. Dit probleem is opgelost. | Ja | Nee |
| 3 | Openstack verificatie | Wanneer u Openstack als uw provider cloud, kan een foutbericht dat uw cloud verificatie tekenreeks te lang is. Dit probleem is opgelost. | Ja | Nee |


## <a name="known-issues-in-update-2"></a>Bekende problemen in Update 2

De volgende tabel bevat een overzicht van bekende problemen in deze release.

| Nee. | Functie | Probleem | Opmerkingen / tijdelijke oplossing | Dit geldt voor fysiek apparaat | Dit geldt voor virtueel apparaat |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Schijf quorum | In uitzonderlijke gevallen als de meeste schijven in de ruimte EBOD van een 8600 apparaat geen verbinding met resultaat geen quorum schijf, gaat klikt u vervolgens de opslag van toepassingen offline. Deze worden offline blijven, zelfs als de schijven opnieuw verbinding wordt gemaakt. | U moet start het apparaat opnieuw op. Als het probleem zich blijft voordoen, neemt u contact op met Microsoft Support voor de volgende stappen. | Ja | Nee |
| 2 | Onjuiste controller-ID | Wanneer een vervangende controller wordt uitgevoerd, mogelijk controller 0 als controller 1 weergegeven. Tijdens de vervangende controller, wanneer de afbeelding is geladen vanuit het knooppunt peer worden de controller-ID weergegeven in eerste instantie als van de besturing van het peer-ID. In uitzonderlijke gevallen kan ook dit gedrag zichtbaar na het systeem opnieuw opstarten. | Er is geen gebruikersactie vereist. Deze situatie worden zelf opgelost nadat de vervangende controller voltooid is. | Ja | Nee |
| 3 | Opslag-accounts | Gebruik van de Storage-service het account opslagruimte verwijderen is een niet-ondersteunde scenario. Dit leidt tot een situatie waarin gebruikersgegevens kan niet worden opgehaald.|  | Ja | Ja |
| 4 | Apparaat failover | Meerdere failovers van een container volume van het bronapparaat met dezelfde voor verschillende apparaten wordt niet ondersteund. Failover vanaf een apparaat met één dode naar meerdere apparaten, kunt u het volume containers op de eerste apparaat overgenomen Gegevenseigendom kwijtraakt. Na een failover wordt deze containers volume worden weergegeven of gedragen zich anders wanneer u deze in de portal van Azure klassieke weergeven. | | Ja | Nee |
| 5 | Installatie | Tijdens StorSimple Adapter voor SharePoint-installatie moet u een apparaat IP-adres in volgorde voor de installatie te voltooien.    | | Ja | Nee |
| 6 | Webproxy | Als de configuratie van uw web-proxy HTTPS als het opgegeven protocol heeft, klikt u vervolgens de communicatie van uw apparaat-naar-service worden beïnvloed en het apparaat gaat offline. Ondersteuningspakketten wordt ook in het proces aanzienlijk bronnen op uw apparaat worden gegenereerd. | Zorg ervoor dat de URL van de web-proxy HTTP als het opgegeven protocol heeft. Ga naar [de web-proxy configureren voor uw apparaat](storsimple-configure-web-proxy.md)voor meer informatie. | Ja | Nee |
| 7 | Webproxy | Als u configureert en webproxy op een apparaat met geregistreerde inschakelt, moet u start de actieve domeincontroller op uw apparaat. | | Ja | Nee |
| 8 | Cloud hoge latentie en hoge i/o-werkbelasting | Wanneer uw apparaat StorSimple een combinatie van zeer hoog cloud vertragingstijden (volgorde van seconden) en hoge i/o-werkbelasting tegenkomt, de hoeveelheden apparaat dieper op een anders werken staat en het i/o kan mislukken met een foutbericht 'apparaat niet gereed'. | U moet handmatig start opnieuw op apparaten of uitvoeren van een apparaat failover deze situatie te herstellen. | Ja | Nee |
| 9 | Azure PowerShell | Wanneer u de StorSimple-cmdlet Get-AzureStorSimpleStorageAccountCredential **& #124; gebruiken Select-Object - eerst 1 - wachten** Selecteer het eerste object zodat u kunt een nieuw **VolumeContainer** -object maken, de cmdlet retourneert alle objecten. | De cmdlet als volgt tussen haakjes teruglopen: **(Get-Azure-StorSimpleStorageAccountCredential) & #124; Select-Object - eerst 1 - wachten** | Ja | Ja |
| 10| Migratie | Wanneer meerdere volume containers worden doorgegeven voor migratie, wordt de ETA voor de meest recente back-up alleen voor de eerste volume-container klopt. Daarnaast kunnen begint parallelle migratie nadat de eerste 4 back-ups in de eerste volume-container worden gemigreerd. | Het is raadzaam om één volume container tegelijk te migreren. | Ja | Nee |
| 11| Migratie | Na het terugzetten, worden niet volumes toegevoegd aan het back-beleid of de groep virtuele schijf. | U moet deze volumes toevoegen aan een back-beleid om te maken van back-ups. | Ja | Ja |
| 12| Migratie | Nadat de migratie voltooid is, moet het apparaat 5000/7000 reeks geen toegang tot de containers gemigreerde gegevens. | Het is raadzaam dat u de containers gemigreerde gegevens verwijderen nadat de migratie voltooid en vastgelegde is. | Ja | Nee |
| 13| Klonen en DR | Een StorSimple-apparaat met Update 1 niet mogelijk klonen of herstel naar een apparaat met oude update 1-software uitvoeren. | U moet het doelapparaat met Update 1 toe te staan dat deze bewerkingen bijwerken | Ja | Ja |
| 14 | Migratie | Back-up van configuratie voor migratie kan mislukken op een apparaat van de reeks 5000-7000 wanneer er volume groepen met niet bijbehorende delen zijn. | Verwijder alle lege volume groepen met niet bijbehorende delen en probeer het back-up van de configuratie.| Ja | Nee |
| 15 | Azure PowerShell-cmdlets en lokaal vastgemaakte volumes | U kunt geen lokaal vastgemaakte volume via Azure PowerShell-cmdlets maken. (Een volume die u via Azure PowerShell maken worden doorverbonden.) |Gebruik altijd de StorSimple Manager-service voor het configureren van lokaal vastgemaakte volumes.| Ja | Nee |
| 16 |Ruimte beschikbaar voor lokaal vastgemaakte volumes | Als u een lokaal vastgemaakte volume verwijdert, worden de beschikbare schijfruimte voor nieuwe volumes mogelijk niet direct bijgewerkt. De service StorSimple Manager bijgewerkt de lokale schijfruimte nagenoeg elk uur.| Wacht totdat een uur voordat u probeert te maken van het nieuwe volume. | Ja | Nee |
| 17 | Lokaal vastgemaakte volumes | Uw terugzettaak beschrijft de tijdelijke momentopname back-up in de back-catalogus, maar alleen voor de duur van de terugzettaak. Daarnaast kunnen worden getoond een virtuele schijfgroep met voorvoegsel **tmpCollection** op de pagina **Beleid voor het back-up maken** , maar alleen voor de duur van de terugzettaak. | Dit kan gebeuren als uw terugzettaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en doorverbonden hoeveelheden vastgemaakt heeft. Als de terugzettaak alleen doorverbonden volumes bevat, klikt u vervolgens plaats dit gedrag niet. Zonder tussenkomst is vereist. | Ja | Nee |
| 18 | Lokaal vastgemaakte volumes | Als u een terugzettaak en een controller overname bij storing annuleren plaatsvindt direct daarna de terugzettaak **mislukt** in plaats van **geannuleerd**wordt weergegeven. Als een terugzettaak mislukt en een controller storing direct daarna de terugzettaak **geannuleerd** in plaats van **is mislukt**wordt weergegeven. | Dit kan gebeuren als uw terugzettaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en doorverbonden hoeveelheden vastgemaakt heeft. Als de terugzettaak alleen doorverbonden volumes bevat, klikt u vervolgens plaats dit gedrag niet. Zonder tussenkomst is vereist. | Ja | Nee |
| 19 |Lokaal vastgemaakte volumes | Als u een terugzettaak annuleren of als een herstellen mislukt en klikt u vervolgens een controller storing, wordt een extra terugzettaak weergegeven op de pagina **taken** . | Dit kan gebeuren als uw terugzettaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en doorverbonden hoeveelheden vastgemaakt heeft. Als de terugzettaak alleen doorverbonden volumes bevat, klikt u vervolgens plaats dit gedrag niet. Zonder tussenkomst is vereist. | Ja | Nee |
| 20 |Lokaal vastgemaakte volumes | Als u probeert een doorverbonden volume (gemaakt en gekloonde met Update 1.2 of eerder) converteren naar een lokaal vastgemaakte volume en het apparaat heeft bijna geen ruimte of er een cloud-storing is, kan de clone(s) is beschadigd.| Dit probleem doet zich alleen met delen die gemaakt en gekloonde met oude Update 2-software zijn. Dit moet een onregelmatig scenario.|
| 21 | Volumeconversie | Werk niet de ACRs die is gekoppeld aan een volume terwijl een volumeconversie uitgevoerd wordt (doorverbonden naar lokaal vastgemaakt of vice versa). Bijwerken van de ACRs, kan dit leiden tot beschadiging. | Indien nodig de ACRs voordat u het volumeconversie bijwerken en breng eventuele andere ACR updates tijdens de conversie uitgevoerd wordt. |

## <a name="controller-and-firmware-updates-in-update-2"></a>Controller en firmware-updates in Update 2

Deze release bijgewerkt via het stuurprogramma en de firmware schijf op uw apparaat.
 
- Voor meer informatie over de firmware LSI, raadpleegt u Microsoft Knowledge base-artikel 3121900. 
- Voor meer informatie over de schijf firmware, raadpleegt u Microsoft Knowledge base-artikel 3121899.
 
## <a name="virtual-device-updates-in-update-2"></a>Virtuele apparaatupdates in Update 2

Deze update kan niet worden toegepast op het virtuele apparaat. Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-step"></a>Volgende stap

Leer hoe u [2 hebt geïnstalleerd](storsimple-install-update-2.md) op uw apparaat StorSimple.
