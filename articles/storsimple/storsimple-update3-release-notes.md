<properties 
   pageTitle="Releaseopmerkingen StorSimple 8000 reeks Update 3 | Microsoft Azure"
   description="Beschrijft de nieuwe functies, problemen en oplossingen voor StorSimple 8000 reeks Update 3."
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
   ms.date="10/14/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-3-release-notes"></a>Releaseopmerkingen StorSimple 8000 reeks Update 3  

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen beschrijven van de nieuwe functies en de kritieke openstaande actie-items aangeven voor StorSimple 8000 reeks Update 3. Ze bevatten ook een lijst met de software-updates van StorSimple opgenomen in deze release. 

Update 3 kan worden toegepast op elk gewenst Release (GA) of een Update 0,1 uitvoeren van Update 2.2 StorSimple-apparaat. De apparaatversie dat is gekoppeld aan Update 3 is 6.3.9600.17759.

Controleer de gegevens in de releaseopmerkingen voordat u de update in uw StorSimple-oplossing implementeert.

>[AZURE.IMPORTANT]
> 
> - Update 3 heeft apparaatsoftware, LSI stuurprogramma en firmware en Storport en Spaceport updates. Het duurt ongeveer 1,5-2 uur deze update te installeren. 

> - Voor nieuwe versies, kan er geen updates direct omdat we een gefaseerd uitrol van de updates doen. Wacht een paar dagen en vervolgens zoeken naar updates opnieuw als deze binnenkort beschikbaar komen.


## <a name="whats-new-in-update-3"></a>Wat is er nieuw in Update 3

De volgende belangrijke verbeteringen en correcties hebt aangebracht in Update 3.

 
- **Automatisch ruimte terug wijzigingen** – Update 3 starten, de ruimte terug algoritmen uitgevoerd op de stand-by controller van het systeem waardoor sneller kan worden uitgevoerd. Voor meer informatie over de poorten die nodig zijn voor gebruik met ruimte terug, raadpleegt u de [StorSimple netwerken vereisten](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

- **Verbeterde prestaties** – Update 3 verbeterd prestaties lezen / schrijven in de cloud.

- **Verbeteringen in de migratie gerelateerde** – zijn In deze release, verschillende correcties en verbeteringen uitgevoerd voor de migratie-functie van 5000/7000 reeks apparaten 8000 reeks apparaten. Ga naar de [migratie van 5000/7000 reeks apparaat naar 8000 reeks apparaat](https://www.microsoft.com/en-us/download/details.aspx?id=47322)voor meer informatie over het gebruik van de migratiefunctie. 

- **Cmdlets voor controle gerelateerde reparaties** - zijn In deze release, fouten die zijn gerelateerd aan grafieken, servicedashboard en apparaat dashboard cmdlets voor controle opgelost.


## <a name="issues-fixed-in-update-3"></a>Problemen worden opgelost in Update 3

De volgende tabellen wordt een overzicht van problemen die in Update 3 verholpen zijn.    

| Nee | Functie                                    | Probleem                                                                                                                                                                                                                                                                                        | Dit geldt voor fysiek apparaat | Dit geldt voor virtueel apparaat |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Host aan de clientzijde gegevensmigratie                      | In de eerdere versie, is het StorSimple Cloud toestel offline gaan tijdens een gegevensmigratie host aan de clientzijde. Dit probleem is opgelost in deze release.                                                | Nee                        | Ja                        |
| 2  | Lokaal vastgemaakte volumes                     | In de vorige versie zijn er problemen met i/o-fouten, mislukte volume-conversies en datapath fouten voor lokaal vastgemaakte volumes. Deze problemen zijn hoofdsite veroorzaakt en vaste in deze release.                | Ja                        | Nee                       |
| 3  | Cmdlets voor controle                                    | Er zijn meerdere problemen met het rapporteren van eenheden en monitoring evenals apparaat dashboard grafieken waar onjuiste informatie voor lokaal vastgemaakte volumes werd weergegeven. Deze problemen worden opgelost in deze release. | Ja                         | Nee                       |
| 4  | Veel schrijft I/O                          | Wanneer u StorSimple voor werkbelasting die betrekking hebben op personen dik schrijft, wordt de gebruiker uitgevoerd in een onregelmatig bug waar de werkset is worden doorverbonden naar de cloud. Dit probleem is opgelost in deze release. | Ja                        | Ja                       |
| 5  | Back-up maken                                     | In bepaalde gevallen niet vaak voorkomen in de vorige versies van software, wanneer de gebruiker heeft een back-up van een externe klonen, zou ze cloud fouten optreden en de bewerking zou fout af. In deze release, het probleem is opgelost en de bewerking is voltooid.             | Ja                        | Ja                       |
| 6  | Back-beleid                                     | In bepaalde gevallen niet vaak voorkomen in de eerdere versies van software, er is een fout die betrekking hebben op de verwijdering van back-beleid. Dit probleem is opgelost in deze release.       | Ja                        | Ja                       |



## <a name="known-issues-in-update-3"></a>Bekende problemen in Update 3

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
| 22 | Updates | Wanneer u Update 3 toepast, wordt **de onderhoudspagina voor in de portal van Azure klassieke** verschijnt het volgende bericht betrekking hebben op Update 2: "StorSimple 8000 reeks Update 2 biedt de mogelijkheid voor Microsoft proactief logboekinformatie verzamelen van uw apparaat wanneer we potentiële problemen detecteren". Dit is misleidende zoals deze wordt aangegeven dat het apparaat wordt bijgewerkt naar Update 2. Wanneer het apparaat bijgewerkt naar de Update 3 succeesfully is, kan dit bericht verdwijnt. | Dit gedrag wordt opgelost in toekomstige versie. | Ja | Nee |


## <a name="controller-and-firmware-updates-in-update-3"></a>Controller en firmware-updates in Update 3

Deze release heeft LSI stuurprogramma en firmware updates. Zie [Update 3 installeren](storsimple-install-update-3.md) op uw apparaat StorSimple voor meer informatie over het installeren van de LSI stuurprogramma en firmware-updates.

 
## <a name="virtual-device-updates-in-update-3"></a>Virtuele apparaatupdates in Update 3

Deze update kan niet worden toegepast op het StorSimple Cloud toestel (ook wel bekend als het virtuele apparaat). Nieuwe virtuele apparaten moet worden gemaakt. 


## <a name="next-step"></a>Volgende stap

Leer hoe u [Update 3 installeren](storsimple-install-update-3.md) op uw apparaat StorSimple.
