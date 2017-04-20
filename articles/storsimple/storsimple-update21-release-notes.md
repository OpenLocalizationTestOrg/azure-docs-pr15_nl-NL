<properties 
   pageTitle="Releaseopmerkingen StorSimple 8000 reeks Update 2.2 | Microsoft Azure"
   description="Beschrijft de nieuwe functies, problemen en oplossingen voor StorSimple 8000 reeks Update 2.2."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-22-release-notes"></a>Releaseopmerkingen StorSimple 8000 reeks Update 2.2  

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen beschrijven van de nieuwe functies en de kritieke openstaande actie-items aangeven voor StorSimple 8000 reeks Update 2.2. Ze bevatten ook een lijst met de software-updates van StorSimple opgenomen in deze release. 

Update 2.2 kan worden toegepast op elk gewenst Release (GA) of een Update 0,1 uitvoeren van Update 2.1 StorSimple-apparaat. De apparaatversie dat is gekoppeld aan Update 2.2 is 6.3.9600.17708.

Controleer de gegevens in de releaseopmerkingen voordat u de update in uw StorSimple-oplossing implementeert.

>[AZURE.IMPORTANT]
> 
> - Update 2.2 heeft alleen software-updates. Het duurt ongeveer 1,5-2 uur deze update te installeren. 

> - Als u Update 2.1 uitvoert, wordt u aangeraden dat u de Update 2.2 zo snel mogelijk toepassen.

> - Voor nieuwe versies, kan er geen updates direct omdat we een gefaseerd uitrol van de updates doen. Wacht een paar dagen en vervolgens zoeken naar updates opnieuw als deze binnenkort beschikbaar komen.


## <a name="whats-new-in-update-22"></a>Wat is er nieuw in Update 2.2

De volgende belangrijke verbeteringen zijn aangebracht in Update 2.2.

 
- **Automatisch ruimte terug optimalisatie** – wanneer u gegevens is verwijderd op dun ingerichte volumes, de niet-gebruikte opslagruimte blokken moeten opnieuw worden gebruikt. Deze release verbeterd, het proces ruimte terug vanuit de cloud met als resultaat de ongebruikte ruimte wordt sneller beschikbaar ten opzichte van de vorige versies.


- **Momentopname prestatieverbeteringen** – Update 2.2 verbeterd, de tijd om een momentopname in bepaalde scenario's waarin grote volumes worden gebruikt en er is minimale naar geen gegevens lospeuteren cloud. Een scenario waarbij de van deze uitbreiding profiteren wilt zou de hoeveelheden archief.


- **Versterking van Support-pakket verzamelen** – was er verbeteringen in de manier waarop het pakket ondersteuning is verzameld en die in deze release zijn geüpload. 


- **Verbeteringen van de betrouwbaarheid bijwerken** – deze release heeft correcties die in een verbeterde betrouwbaarheid van de Update resulteren.

  
 

## <a name="issues-fixed-in-update-22"></a>Problemen in Update 2.2 worden opgelost

De volgende tabellen wordt een overzicht van problemen die in Updates 2.2 en 2.1 verholpen zijn.    

| Nee | Functie                                    | Probleem                                                                                                                                                                                                                                                                                        | Dit geldt voor fysiek apparaat | Dit geldt voor virtueel apparaat |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Host prestaties                      | In de eerdere versie, zijn host aan de clientzijde prestatieproblemen tijdens het maken van een lokaal vastgemaakte volume en tijdens de conversie van een doorverbonden volume naar een lokaal vastgemaakte volume waargenomen. Deze problemen worden opgelost in deze release daarmee ook resulteert in een verbetering van de host prestaties tijdens het volume kunnen maken en conversie procedures.                                                                        | Ja                        | Nee                        |
| 2  | Lokaal vastgemaakte volumes                     | Het systeem zou loopt vast bij het maken van een lokaal vastgemaakte volume in uitzonderlijke gevallen. Deze fout is opgelost in deze release.                                                                                                                                                               | Ja                        | Nee                        |
| 3  | Trapsgewijs schakelen                                    | Zijn er enkel geval loopt vast wanneer de metagegevens voor de StorSimple Cloud toestellen (8010 en 8020) naar de cloud doorverbonden. Dit probleem is opgelost in deze release.                                                                                                                              | Nee                         | Ja                       |
| 4  | Momentopname maken                          | Er zijn problemen met betrekking tot het maken van incrementele momentopnamen in scenario's met grote hoeveelheden en zo min mogelijk naar geen lospeuteren gegevens. Deze problemen worden opgelost in deze release.                                                                                                                 | Ja                        | Ja                       |
| 5  | Openstack verificatie                   | Wanneer u Openstack als de cloud-provider, wordt de gebruiker uitgevoerd in een onregelmatig bug die betrekking hebben op de verificatie waar de JSON-parser geleid in een vastgelopen programma. Dit probleem is opgelost in deze release.                                                                                                                              | Ja                        | Nee                        |
| 6  | Host aan de clientzijde kopiëren                             | In eerdere versies van software, is een onregelmatig bug die betrekking hebben op de timing ODX wanneer u de gegevens van één volume kopieert naar een ander volume zichtbaar. Dit resulteert in een failover controller en het systeem mogelijk herstel kan gaan. Dit probleem is opgelost in deze release. | Ja                        | Nee       |
| 7  | Windows Management Instrumentation (WMI) | In de vorige versies van software, zijn er meerdere exemplaren van web proxy mislukt met de uitzondering "<ManagementException> Provider laden mislukt". Deze fout is toegekend aan een WMI geheugen zijn verdwenen en is nu opgelost.                                                               | Ja                        | Nee                        |
| 8  | Update                                     | In bepaalde gevallen niet vaak voorkomen in de vorige versies van software, ontvangen de gebruiker een 'CisPowershellHcsscripterror' wanneer u probeert te scannen of updates installeren. Dit probleem is opgelost in deze release.                                                                                        | Ja                        | Ja                       |
| 9  | Support-pakket                            | In deze release zijn verbeteringen in de manier waarop het pakket ondersteuning is verzameld en geüpload.                                                                                                                                                                                                      | Ja                        | Ja                                    |


## <a name="known-issues-in-update-22"></a>Bekende problemen in Update 2.2

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
| 20 |Lokaal vastgemaakte volumes | Als u probeert een doorverbonden volume (gemaakt en gekloonde met Update 1.2 of eerder) converteren naar een lokaal vastgemaakte volume en het apparaat heeft bijna geen ruimte of er een cloud-storing is, kan de clone(s) is beschadigd.| Dit probleem doet zich alleen met delen die gemaakt en gekloonde met oude Update 2.1-software zijn. Dit moet een onregelmatig scenario.|
| 21 | Volumeconversie | Werk niet de ACRs die is gekoppeld aan een volume terwijl een volumeconversie uitgevoerd wordt (doorverbonden naar lokaal vastgemaakt of vice versa). Bijwerken van de ACRs, kan dit leiden tot beschadiging. | Indien nodig de ACRs voordat u het volumeconversie bijwerken en breng eventuele andere ACR updates tijdens de conversie uitgevoerd wordt. |

## <a name="controller-and-firmware-updates-in-update-22"></a>Controller en firmware-updates in Update 2.2

Deze release heeft alleen-software-updates. Echter als u vanuit een eerdere versie dan Update 2 bijwerken wilt, u moet installeren stuurprogramma, Storport, Spaceport, en (in sommige gevallen) schijf firmware-updates op uw apparaat.
 
Zie [Update 2.2 installeren](storsimple-install-update-21.md) op uw apparaat StorSimple voor meer informatie over het installeren van het stuurprogramma, Storport, Spaceport en schijf firmware-updates.

 
## <a name="virtual-device-updates-in-update-22"></a>Virtuele apparaatupdates in Update 2.2

Deze update kan niet worden toegepast op het virtuele apparaat. Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-step"></a>Volgende stap

Leer hoe u [Update 2.2 installeren](storsimple-install-update-21.md) op uw apparaat StorSimple.
