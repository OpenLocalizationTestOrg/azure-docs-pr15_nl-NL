<properties 
    pageTitle="StorSimple 8000 Update 0.1 releaseopmerkingen | Microsoft Azure"
    description="Beschrijving van de nieuwe functies en reparaties, openstaande actie-items en beschikbaar tijdelijke oplossingen voor de oktober 2014 Microsoft Azure StorSimple release (Update van 0,1)."
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
    ms.date="09/21/2016"
    ms.author="alkohli" />

# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>StorSimple 8000 reeks Update 0,1 releaseopmerkingen – oktober 2014  

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen Identificeer de kritieke openstaande actie-items voor StorSimple 8000 reeks Update 0,1 uitgebracht in oktober 2014. Ze bevatten ook een lijst met de StorSimple-software en firmware updates zijn opgenomen in deze release. Dit is de eerste versie nadat de StorSimple 8000 reeks versie beschikbaar is gesteld over het algemeen in juli 2014 en komt met softwareversie 6.3.9600.17312 overeen.  

Het is raadzaam dat u naar zoeken en beschikbare updates toepassen onmiddellijk nadat u het apparaat hebt geïnstalleerd. U kunt ook automatische updates downloaden en installeren van essentiële updates van Microsoft zodra ze zijn vrijgegeven inschakelen. Zie voor meer informatie het [bijwerken van uw apparaat StorSimple](storsimple-update-device.md).  

Controleer de gegevens in de releaseopmerkingen voordat u de updates in uw StorSimple-oplossing implementeert.  

>[AZURE.IMPORTANT]
> 
-   Gebruik de StorSimple Manager-service en niet Windows PowerShell voor StorSimple de updates voor oktober te installeren.  
-   De updates worden meestal ongeveer 3 uur duren om te voltooien.  
-   De versie oktober van StorSimple bevat geen updates van het virtuele StorSimple-apparaat. U kunt nog steeds toepassen beschikbare Windows-updates, inclusief recente beveiligingscorrecties, maar een wijziging in versie voor het virtuele apparaat wordt niet weergegeven.  

Zorg ervoor dat de volgende voorwaarden wordt voldaan vóór het bijwerken van uw apparaat StorSimple.  

- Zorg ervoor dat beide apparaatcontrollers worden uitgevoerd voordat u naar updates zoeken. Als een van beide controller wordt niet uitgevoerd, mislukt de gescande afbeelding. Als u wilt controleren of de controllers zijn in orde zijn, navigeer naar **Hardware Status** onder **de onderhoudspagina voor** . Als er onderdelen die **aandacht nodig**, neem dan contact op met Microsoft Support voordat u verdergaat.  
- Zorg ervoor dat vaste IP-adressen voor beide Controller 0 en 1 Controller geschikt zijn en kunnen worden verbonden met Internet terwijl deze worden gebruikt voor het onderhoud van de updates bij het apparaat. U kunt de [testverbinding cmdlet](https://technet.microsoft.com/library/hh849808.aspx) ping een bekend adres buiten het netwerk zoals outlook.com, om te bevestigen dat de controller connectiviteit met het externe netwerk heeft.  
- Zorg ervoor dat de vereiste uitgaande poort op uw apparaat StorSimple voor uitgaande communicatie beschikbaar zijn. Zie de [vereisten voor uw apparaat StorSimple toegang](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)voor meer informatie.  
- Als de versie van de software apparaat ouder is dan 6.3.9600.17312 (update van oktober 2014), de poorten gegevens 2 en 3 van de gegevens uitschakelen als ingeschakeld, voordat u begint met de update. Als u de gegevens 2 of 3 gegevens poorten ingeschakeld als u de update verlaten, is het mogelijk dat van uw apparaat controller om te gaan herstel. Houd er rekening mee dat wanneer u de netwerkinterfaces uitschakelt, de bijbehorende hoeveelheden wordt offline worden geplaatst en de I/O wordt voor de duur van de update worden onderbroken.  

## <a name="whats-new-in-the-october-release"></a>Wat is er nieuw in de oktober-versie

Deze update bevat de volgende verbeteringen:

- U kunt nu de StorSimple Manager-service UI gebruiken voor het beheren van uw apparaatcontrollers. Het beheer acties opnemen opnieuw opstart, afsluiten, of een controller inschakelen. Ga naar [Apparaatcontrollers StorSimple beheren](storsimple-manage-device-controller.md)voor meer informatie.  
- U kunt WAN-bandbreedte worden toegewezen op basis van de dag-van-de-week en tijdgegevens-combinaties plannen. Hiermee kunt u beter gebruikmaken van WAN-bandbreedte na kantooruren. Verschillende bandbreedte sjablonen zijn voor verschillende volume containers toegestaan. Ga naar [uw StorSimple bandbreedte-distributiegroepen beheren](storsimple-manage-bandwidth-templates.md)voor meer informatie.  
- U kunt e-mailmeldingen te informeren de databasebeheerder (s) en andere van bestaande of mogelijk komende problemen configureren. Ga naar [de waarschuwingsinstellingen configureren](storsimple-manage-alerts.md#configure-alert-settings)voor meer informatie.  

## <a name="issues-fixed-in-the-october-release"></a>Problemen opgelost in de release oktober


De volgende tabel bevat een overzicht van fouten die in deze update worden verholpen.  

| Nee. | Functie | Probleem | Dit geldt voor fysiek apparaat | Dit geldt voor virtueel apparaat |
|-----|---------|-------|---------------------------------|--------------------------------|
| 1 | Netwerkinterfaces | In de vorige versie, zijn de netwerkinterfaces gegevens 2 en 3 van de gegevens in de software omgewisseld. Dit probleem is opgelost in deze update. Wis de instellingen en deze netwerkinterfaces uitschakelen voordat u de update installeren. Na de installatie van de update, wordt u moet deze interfaces configureren. | Ja | Nee |
| 2 | Support-pakket | In de vorige versie, als u de Windows PowerShell **Exporteren-HcsSupportPackage** -cmdlet voor het ophalen van de logboeken Baseboard Management Controller (BMC) uitgevoerd, kunt u de bewerking is mislukt met de volgende waarschuwing: "de bewerking is geslaagd op deze controller, maar kan niet op de controller peer vanwege de volgende fouten. Controleer of de peer in orde is en of het huidige knooppunt kan worden verbonden met de peer." Dit probleem is nu opgelost. | Ja | Nee |
| 3 | Apparaat failover | In de vorige versie is er een kans inconsistenties in de gegevens als een taak **kennismaken met back-up** is mislukt tijdens een de overname van een apparaat. Dit probleem is nu opgelost. | Ja | Nee |
| 4 | Apparaat failover | Back-ups werden weergegeven in de vorige versie, na een failover apparaat, maar de container gekoppeld volume is niet aanwezig zijn in de doelapparaat. Dit probleem is nu opgelost. | Ja | Nee |
| 5 | Apparaat failover | In de vorige versie, moet u er een fout is in de opsomming van back-ups van cloud tijdens het register-terugzetten die mogelijk tot inconsistenties in de gegevens leiden kan als er problemen met de cloud zijn. | Ja | Nee |
| 6 | Firmware-update | In de vorige versie, de taak apparaat firmware update is mislukt en een fout die vermeld dat de cmdlets niet herkenbaar zijn en dat de update als gevolg worden niet weergegeven. De controller vervolgens werd herstel. Dit probleem is nu opgelost. | Ja | Nee |
| 7 | Installatie | Fouten die worden veroorzaakt door het apparaat niet wordt afbeelding juist tijdens de installatie zijn nu opgelost. | Ja | Nee |
| 8 | Factory opnieuw instellen | U kunt nu de firmware controleren op factory opnieuw instellen (optioneel) overslaan. Dit is een wijziging van de vorige versie. | Ja | Nee |
| 9 | Factory opnieuw instellen | In de vorige versie, dat tijdens het uitvoeren van een fabriek Beginwaarden cmdlet, firmware versie controles zijn aangebracht alleen voor bepaalde hardwareonderdelen. Aanvullende firmware controles zijn aangebracht na de eerste keer opnieuw opstarten in het proces, waardoor de beginwaarden mislukt. Deze fix zorgt ervoor dat alle controles firmware zijn doorgevoerd wanneer de fabriek opnieuw cmdlet wordt uitgevoerd en voordat u het eerste systeem start opnieuw op. | Ja | Nee |
| 10 | Belangrijke draaiing voor opslag-account | De **Roep-HcsmServiceDataEncryptionKeyChange** -cmdlet die wordt gebruikt voor het omwisselen van de opslagruimte account toetsen nu wordt de gebruiker de sleutel service gegevens invoeren. Dit is een wijziging van de vorige versie waarin de service gegevenssleutel is doorgegeven als een inline-parameter. | Ja | Nee |
| 11 | Failback binnen 24 uur | Tijdens herstel, het opruimen op het bronapparaat duidelijke, waardoor failback mislukt niet plaatsvindt. Dit probleem is opgelost in deze release. | Ja | Nee |

## <a name="known-issues-in-the-october-release"></a>Bekende problemen in de release oktober

De volgende tabel bevat een overzicht van bekende problemen in deze release.

| Nee. | Functie | Probleem | Opmerkingen/tijdelijke oplossing | Dit geldt voor fysiek apparaat | Dit geldt voor virtueel apparaat |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Factory opnieuw instellen | In sommige gevallen, wanneer u een Beginwaarden factory uitvoert het apparaat StorSimple vastgelopen en dit bericht weergeven: **terugzetten naar factory wordt uitgevoerd (fase 8)**. Dit gebeurt als u op CTRL + C drukt terwijl de cmdlet uitgevoerd wordt. | Druk CTRL + C niet op na het starten van een fabriek opnieuw instellen. Als u al in deze status bent, neemt u contact op met Microsoft Support voor de volgende stappen. | Ja | Nee |
| 2 | Factory opnieuw instellen | Kan geen factory Beginwaarden een StorSimple-apparaat, dat wordt bijgewerkt van GA tot oktober 2014 loslaat. | Deze bewerking werkt alleen als een patch is geïnstalleerd. Neem contact op met Microsoft Support als u deze patch vereist. | Ja | Nee | 
| 3 | Schijf quorum | In uitzonderlijke gevallen als de meeste schijven in de ruimte EBOD van een 8600 apparaat geen verbinding met resultaat geen quorum schijf, klikt u vervolgens de opslag van toepassingen niet offline. Deze worden offline blijven, zelfs als de schijven opnieuw verbinding wordt gemaakt. | U moet start het apparaat opnieuw op. Als het probleem zich blijft voordoen, neemt u contact op met Microsoft Support voor de volgende stappen. | Ja | Nee |
| 4 | Cloud momentopname fouten | In uitzonderlijke gevallen kan een momentopname cloud mislukken met de fout **Maximum back-limiet is bereikt**. Dit gebeurt als u meer dan 255 online klonen op hetzelfde apparaat, van hetzelfde oorspronkelijke volume die is verwijderd. | | Ja | Ja |
| 5 | Onjuiste controller-ID | Wanneer een vervangende controller wordt uitgevoerd, mogelijk controller 0 als controller 1 weergegeven. Tijdens de vervangende controller, wanneer de afbeelding is geladen vanuit het knooppunt peer worden de controller-ID weergegeven in eerste instantie als van de besturing van het peer-ID. In uitzonderlijke gevallen kan ook dit gedrag zichtbaar na het systeem opnieuw opstarten. |Er is geen gebruikersactie vereist. Deze situatie worden zelf opgelost nadat de vervangende controller voltooid is. | Ja | Nee |
| 6 | Apparaat grafieken bewaken | In de service StorSimple Manager, het apparaat controleren grafieken werken niet als eenvoudige of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat. | Wijzig de configuratie van de web-proxy voor het apparaat dat is geregistreerd bij uw Manager StorSimple-service zodat dat verificatie is ingesteld op geen. Klik hiertoe uitvoeren de de Windows PowerShell voor de cmdlet Set-HcsWebProxy van StorSimple. | Ja | Ja |
| 7 | Opslag-accounts | Gebruik van de Storage-service het account opslagruimte verwijderen is een niet-ondersteunde scenario. Dit leidt tot een situatie waarin gebruikersgegevens kan niet worden opgehaald. | | Ja | Ja |
| 8 | Apparaat failover | Meerdere failovers van een container volume van het bronapparaat met dezelfde voor verschillende apparaten wordt niet ondersteund. | Failover vanaf een apparaat met één dode naar meerdere apparaten, kunt u het volume containers op de eerste apparaat overgenomen Gegevenseigendom kwijtraakt. Na een failover wordt deze containers volume worden weergegeven of gedragen zich anders wanneer u deze in de portal van Azure klassieke weergeven. | Ja | Nee |
| 9 | Installatie | Tijdens StorSimple Adapter voor SharePoint-installatie moet u een apparaat IP-adres in volgorde voor de installatie te voltooien.    | | Ja | Nee |
| 10 | Webproxy | Als de configuratie van uw web-proxy HTTPS als het opgegeven protocol heeft, klikt u vervolgens de communicatie van uw apparaat-naar-service worden beïnvloed en het apparaat gaat offline. Ondersteuningspakketten wordt ook in het proces aanzienlijk bronnen op uw apparaat worden gegenereerd. | Zorg ervoor dat de URL van de web-proxy HTTP als het opgegeven protocol heeft. Meer informatie over de [web-proxy configureren voor uw apparaat](storsimple-configure-web-proxy.md). | Ja | Nee |
| 11 | Webproxy | Als u configureert en webproxy op een apparaat met geregistreerde inschakelt, moet u start de actieve domeincontroller op uw apparaat. | | Ja | Nee |
| 12 | Cloud hoge latentie en hoge i/o-werkbelasting | Wanneer uw apparaat StorSimple een combinatie van zeer hoog cloud vertragingstijden (volgorde van seconden) en hoge i/o-werkbelasting tegenkomt, de hoeveelheden apparaat dieper op een anders werken staat en het i/o kan mislukken met een foutbericht 'apparaat niet gereed'. | U moet handmatig start opnieuw op apparaten of uitvoeren van een apparaat failover deze situatie te herstellen. | Ja | Nee |

## <a name="physical-device-updates-in-the-october-release"></a>Fysiek apparaat-updates in de oktober vrijgeven

Wanneer u deze updates zijn toegepast op een fysiek apparaat, worden de softwareversie wordt gewijzigd in 6.3.9600.17312. Tenzij anderszins is opgegeven, worden deze releaseopmerkingen toepassen op alle modellen van het apparaat StorSimple. Zie voor meer informatie over deze updates voor [oktober 2014 fysiek toestelsoftware-update voor Microsoft Azure StorSimple toestel](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-october-release"></a>Seriële bijgevoegd SCSI (SA's) controller en firmware-updates in de oktober vrijgeven

Deze release bijgewerkt via het stuurprogramma en de firmware op de controller SA's van uw apparaat. Voor meer informatie over de controller SA's, raadpleegt u [oktober 2014 bijwerken voor besturing voor LSI SA's in Microsoft Azure StorSimple toestel](http://support.microsoft.com/kb/2987020).   

Deze versie geldt ook een update van de cumulatieve firmware die betrouwbaarheidsproblemen met de onderdelen van de hardware apparaat adressen. Zie voor meer informatie over de update firmware, [oktober 2014 firmware update voor Microsoft Azure StorSimple toestel](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-the-october-release"></a>Virtuele apparaatupdates in de oktober vrijgeven

Deze release bevat niet alle updates voor het virtuele apparaat. De softwareversie van een virtueel apparaat wordt niet gewijzigd als u deze update toepassen.
 
