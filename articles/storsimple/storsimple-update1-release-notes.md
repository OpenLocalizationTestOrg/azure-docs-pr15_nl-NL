<properties 
   pageTitle="Releaseopmerkingen StorSimple 8000 reeks Update 1.2 | Microsoft Azure"
   description="Beschrijft de nieuwe functies, problemen en oplossingen voor StorSimple 8000 reeks Update 1.2."
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-12-release-notes"></a>Releaseopmerkingen StorSimple 8000 reeks Update 1.2  

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen beschrijven van de nieuwe functies en de kritieke openstaande actie-items aangeven voor StorSimple 8000 reeks Update 1.2. Ze ook bevatten een lijst met de software, stuurprogramma en schijf firmware-updates is opgenomen in deze release van StorSimple. 

Update 1.2 kan worden toegepast op elk gewenst StorSimple-apparaat met Release (GA), Update 0,1, Update 0,2 of 0,3 Update-software. Update 1.2 is niet beschikbaar als uw apparaat Update 1 of Update 1.1 wordt uitgevoerd. Als uw apparaat wordt uitgevoerd Release (GA), neemt u [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) om u te helpen bij het installeren van deze update.

De volgende tabel bevat de versies van het apparaat software overeenkomt met Updates 1, 1.1 en 1.2.

| Als u update uitvoert... | Dit is uw versie van de software apparaat. |
|---------------------|---------------------------------------|
| Update 1.2          | 6.3.9600.17584                        |
| 1.1 bijwerken          | 6.3.9600.17521                        |
| 1.0 bijwerken          | 6.3.9600.17491                        |

Controleer de gegevens in de releaseopmerkingen voordat u de update in uw StorSimple-oplossing implementeert. Zie voor meer informatie het [installeren van de Update 1.2 op uw apparaat StorSimple](storsimple-install-update-1.md). 

>[AZURE.IMPORTANT]
> 
- Het duurt ongeveer 5-10 uur deze update (inclusief de Windows-Updates) te installeren. 
- Update 1.2 heeft software, LSI stuurprogramma en schijf firmware-updates. Als u wilt installeren, volgt u de instructies in [Update 1.2 op uw apparaat StorSimple installeren](storsimple-install-update-1.md).
- Voor nieuwe versies, kan er geen updates direct omdat we een gefaseerd uitrol van de updates doen. Zoeken naar updates in een paar dagen weer als deze beschikbaar komen binnenkort.


## <a name="whats-new-in-update-12"></a>Wat is er nieuw in Update 1.2

Deze functies zijn eerst uitgebracht met Update 1 die naar een beperkt aantal gebruikers beschikbaar is gesteld. Meest van de gebruikers StorSimple wilt met de release Update 1.2 zien de volgende nieuwe functies en verbeteringen:

- **Migratie van 5000-7000 reeks naar 8000 reeks apparaten** – deze release bevat een nieuw migratie-functie waarin de StorSimple 5000-7000 reeks toestel gebruikers kunnen hun gegevens migreren naar een StorSimple 8000 reeks fysiek toestel of een virtuele toestel. De migratiefunctie heeft twee sleutelwaarde voorstellen:                                                                  

    - **Bedrijfscontinuïteit**, doordat de migratie van bestaande gegevens op 5000-7000 reeks toestellen op 8000 reeks toestellen.
    - **Verbeterde functie aanbiedingen 8000 reeks apparaten**, zoals efficiënt Centraal beheer van meerdere toestellen tot en met StorSimple Manager-service, beter categorie hardware en firmware, virtuele toestellen, gegevensmobiliteit en functies in de toekomstige roadmap bijgewerkt.

    Raadpleeg de [Migratiehandleiding](http://www.microsoft.com/download/details.aspx?id=47322) voor meer informatie over het migreren van een StorSimple 5000-7000 reeks naar een 8000 reeks-apparaat. 

- **Beschikbaarheid in de Portal voor de overheid Azure** : StorSimple is nu beschikbaar in de portal Azure overheid. Zie hoe u [een apparaat StorSimple in de Portal van Azure overheid implementeren](storsimple-deployment-walkthrough-gov.md).

- **Ondersteuning voor andere serviceproviders cloud** – de andere cloud serviceproviders ondersteund zijn Amazon S3, Amazon S3 met records, HP en OpenStack (beta).

- **Update voor de meest recente Storage-API's** – is met deze release, StorSimple bijgewerkt naar de meest recente Azure Storage-service API's. StorSimple 8000 reeks apparaten met een Update 1 voor oude software versie (Release, 0,1 0,2 en 0,3) met een versie van de Azure Storage-Service API-ouder zijn dan 17 juli 2009. De wijze beschreven in de bijgewerkte [aankondiging over verwijdering van opslag Serviceversies](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx), met 1 augustus 2016, wordt deze API's worden afgeschaft. Het is noodzakelijk dat u het StorSimple 8000 reeks 1 vóór 1 augustus 2016 toepassen. Als u niet kunt doen, stopt StorSimple apparaten goed werkt.

- **Ondersteuning voor Zone overtollige opslag (ZRS)** – met de upgrade naar de nieuwste versie van de opslag-API's, de reeks StorSimple 8000 biedt ondersteuning voor Zone overtollige opslag (ZRS) naast de lokaal overtollige opslag (LRS) en geografische-redundante opslag (GRS). Raadpleeg dit [artikel over Azure Storage redundantieopties](../storage/storage-redundancy.md) voor ZRS meer informatie.

- **Eerste implementatie- en update experience enhanced** – zijn In deze release, de installatie en updateprocessen verbeterd. De installatie van de wizard setup gaat gebruiken is als u feedback wilt aan de gebruiker als de netwerkconfiguratie en firewallinstellingen onjuist zijn verbeterd. Aanvullende diagnostische cmdlets hebben om u te helpen bij het oplossen van netwerk van het apparaat dat is opgegeven. Zie het [artikel implementatie probleemoplossing](storsimple-troubleshoot-deployment.md) voor meer informatie over de nieuwe diagnostische cmdlets die worden gebruikt voor probleemoplossing.

## <a name="issues-fixed-in-update-12"></a>Problemen worden opgelost in Update 1.2

De volgende tabel bevat een overzicht van problemen die zijn opgelost in Updates 1.2, 1.1 en 1.    


| Nee. | Functie | Probleem | Vaste in Update | Dit geldt voor fysiek apparaat | Dit geldt voor virtueel apparaat |
|-----|---------|-------|-----------------|---------------------------------|--------------------------------|
| 1 | Windows PowerShell voor StorSimple | Wanneer een gebruiker externe toegang krijgen het StorSimple-apparaat tot met Windows PowerShell voor StorSimple en vervolgens de installatiewizard worden gestart, opgetreden vastlopen zo spoedig gegevens 0 IP is ingevoerd. Deze fout wordt nu verholpen in Update 1. | Update 1 | Ja | Ja |
| 2 | Factory opnieuw instellen | In sommige gevallen, wanneer u een Beginwaarden factory uitgevoerd het apparaat StorSimple kwam hangen en dit bericht weergegeven: **terugzetten naar factory wordt uitgevoerd (fase 8)**. Dit is er gebeurd als u op CTRL + C drukt terwijl de cmdlet werd uitgevoerd. Deze fout is nu opgelost.| Update 1 | Ja | Nee |
| 3 | Factory opnieuw instellen | Nadat een factory mislukte dubbele controller opnieuw hebt ingesteld, zijn u mag apparaatregistratie voortzetten. Waardoor een niet-ondersteunde systeemconfiguratie. Klik in de Update 1, een foutbericht wordt weergegeven en registratie is geblokkeerd op een apparaat dat een mislukte factory opnieuw is. | Update 1 | Ja | Nee |
| 4 | Factory opnieuw instellen | In sommige gevallen zijn ONWAAR positieve verkeerde combinaties waarschuwingen verheven. Onjuiste verkeerde combinaties waarschuwingen wordt niet meer op apparaten met Update 1 gegenereerd. | Update 1 | Ja | Nee |
| 5 | Factory opnieuw instellen | Als het opnieuw instellen van een fabriek is onderbroken voordat deze is voltooid, wordt het apparaat herstelmodus ingevoerd en geeft u voor toegang tot Windows PowerShell voor StorSimple geen toestemming. Deze fout is nu opgelost. | Update 1 | Ja | Nee |
| 6 | Problemen oplossen | Een noodgevallen herstel (DR) fout is opgelost waarin DR zou mislukt tijdens het detecteren van back-ups op het doelapparaat. | Update 1 | Ja | Ja |
| 7 | Cmdlets voor controle LED 's | In bepaalde gevallen wordt hebt monitoring LED's aan het einde van toestel geeft niet juist status. De blauwe LED is uitgeschakeld. GEGEVENS 0 en 1 LED's van gegevens zijn knipperende zelfs wanneer deze interfaces zijn niet geconfigureerd. Het probleem is opgelost en controle LED's geven nu de juiste status.  | Update 1 | Ja | Nee |
| 8 | Cmdlets voor controle LED 's | In bepaalde gevallen wordt na het toepassen van Update 1, de blauwe light op de actieve controller uitgeschakeld waardoor moeilijk te identificeren van de actieve controller. Dit probleem is opgelost in deze release patch.| Update 1.2 | Ja | Nee |
| 9 | Netwerkinterfaces | In eerdere versies, kan een StorSimple apparaat dat is geconfigureerd met een niet-omgeleid gateway offline gaan. In deze release, de routering meetwaarde voor gegevens 0 is aangebracht de laagste; zelfs als er andere netwerkinterfaces zijn cloud is ingeschakeld, wordt er daarom alle cloud-verkeer van het apparaat worden gerouteerd via gegevens 0. | Update 1 | Ja | Ja | 
| 10 | Back-ups | Een fout in Update 1 waardoor back-ups mislukt na 24 dagen is vastgesteld in de patch versie 1.1 van de Update. | 1.1 bijwerken | Ja | Ja |
| 11 | Back-ups | Het resultaat is een fout in eerdere versies in slechte prestaties voor cloud momentopnamen met lage wijzigen tarieven. Deze fout is opgelost in deze release patch.| Update 1.2 | Ja | Ja |
| 12 | Updates | In deze release patch is een fout in Update 1 die aangegeven een mislukte upgrade en de controllers om te gaan in de modus voor herstel, veroorzaakt opgelost.| Update 1.2 | Ja | Ja |


## <a name="known-issues-in-update-12"></a>Bekende problemen in Update 1.2

De volgende tabel bevat een overzicht van bekende problemen in deze release.

| Nee. | Functie | Probleem | Opmerkingen/tijdelijke oplossing | Dit geldt voor fysiek apparaat | Dit geldt voor virtueel apparaat |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Schijf quorum | In uitzonderlijke gevallen als de meeste schijven in de ruimte EBOD van een 8600 apparaat geen verbinding met resultaat geen quorum schijf, klikt u vervolgens de opslag van toepassingen niet offline. Deze worden offline blijven, zelfs als de schijven opnieuw verbinding wordt gemaakt. | U moet start het apparaat opnieuw op. Als het probleem zich blijft voordoen, neemt u contact op met Microsoft Support voor de volgende stappen. | Ja | Nee |
| 2 | Onjuiste controller-ID | Wanneer een vervangende controller wordt uitgevoerd, mogelijk controller 0 als controller 1 weergegeven. Tijdens de vervangende controller, wanneer de afbeelding is geladen vanuit het knooppunt peer worden de controller-ID weergegeven in eerste instantie als van de besturing van het peer-ID. In uitzonderlijke gevallen kan ook dit gedrag zichtbaar na het systeem opnieuw opstarten. | Er is geen gebruikersactie vereist. Deze situatie worden zelf opgelost nadat de vervangende controller voltooid is. | Ja | Nee |
| 3 | Opslag-accounts | Gebruik van de Storage-service het account opslagruimte verwijderen is een niet-ondersteunde scenario. Dit leidt tot een situatie waarin gebruikersgegevens kan niet worden opgehaald. | Ja | Ja |
| 4 | Apparaat failover | Meerdere failovers van een container volume van het bronapparaat met dezelfde voor verschillende apparaten wordt niet ondersteund. Apparaat failover vanaf een apparaat met één dode naar meerdere apparaten, kunt u het volume containers op de eerste apparaat overgenomen Gegevenseigendom kwijtraakt. Na een failover wordt deze containers volume worden weergegeven of gedragen zich anders wanneer u deze in de portal van Azure klassieke weergeven. | | Ja | Nee |
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

## <a name="physical-device-updates-in-update-12"></a>Fysiek apparaat-updates in Update 1.2

Als patch bijwerken 1.2 wordt toegepast op een fysiek apparaat (met versies vóór Update 1), versie van de software wordt gewijzigd in 6.3.9600.17584.

## <a name="controller-and-firmware-updates-in-update-12"></a>Controller en firmware-updates in Update 1.2

Deze release bijgewerkt via het stuurprogramma en de firmware schijf op uw apparaat.
 
- Zie voor meer informatie over het bijwerken van de controller SA's, [Update 1 voor besturing voor LSI SA's in Microsoft Azure StorSimple toestel](https://support.microsoft.com/kb/3043005). 

- Voor meer informatie over de schijf firmware, raadpleegt u [Schijfopruiming firmware Update 1 voor Microsoft Azure StorSimple toestel](https://support.microsoft.com/kb/3063416).
 
## <a name="virtual-device-updates-in-update-12"></a>Virtuele apparaatupdates in Update 1.2

Deze update kan niet worden toegepast op het virtuele apparaat. Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-steps"></a>Volgende stappen

- [Update 1.2 installeren op uw apparaat](storsimple-install-update-1.md).
 
