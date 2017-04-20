<properties
   pageTitle="Update 3 installeren op uw apparaat StorSimple | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe u StorSimple 8000 reeks Update 3 installeert op uw apparaat van de reeks StorSimple 8000."
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
   ms.date="10/05/2016"
   ms.author="alkohli" />

# <a name="install-update-3-on-your-storsimple-device"></a>Update 3 installeren op uw apparaat StorSimple

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe u Update 3 installeert op een apparaat StorSimple met een eerdere softwareversie via de portal van Azure klassieke en de methode hotfix. De methode hotfix wordt gebruikt wanneer een gateway is geconfigureerd op een netwerkinterface dan gegevens 0 van het apparaat StorSimple en u wilt bijwerken vanuit een oude Update 1 softwareversie.

Update 3 bevat apparaatsoftware, LSI stuurprogramma en firmware, Storport en Spaceport updates. Als bijwerkt vanuit Update 2 of een eerdere versie, worden u ook moeten uitgevoerd om te passen iSCSI, WMI, en in bepaalde gevallen schijf firmware-updates. Het apparaatsoftware, WMI iSCSI, LSI stuurprogramma, Spaceport en Storport correcties ononderbroken updates zijn en via de portal van Azure klassieke kunnen worden toegepast. De schijf firmware-updates zijn storend updates en kunnen alleen worden toegepast via de Windows PowerShell-interface van het apparaat. 

> [AZURE.IMPORTANT]

> - Een reeks handmatige en automatische oude controles wordt uitgevoerd voordat u de installatie om te bepalen de status van het apparaat met hardware provincie en de netwerkconnectiviteit. Deze oude controles uitgevoerd alleen als u de updates vanaf de portal van Azure klassieke toepassen.
> - Het is raadzaam dat u de software en stuurprogramma's via de portal van Azure klassieke installeren. U moet alleen Ga naar de Windows PowerShell-interface van het apparaat (voor het installeren van updates) als het selectievakje oude update gateway is mislukt in de portal. Afhankelijk van de versie die u bijwerkt, duurt de updates 1,5-2,5 uur te installeren. De updates van de modus onderhoud moeten worden geïnstalleerd via de Windows PowerShell-interface van het apparaat. Zoals onderhoud modus updates storend updates zijn, resulteert dit in een tijd voor uw apparaat.
> - Als het optionele StorSimple momentopname Manager uitgevoerd, moet u controleert u uw momentopname Manager-versie hebt bijgewerkt naar de Update 2 vóór het bijwerken van het apparaat.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-3-via-the-azure-classic-portal"></a>Update 3 installeren via de portal van Azure klassieke

De volgende stappen als u wilt bijwerken van uw apparaat met [3 bijwerken](storsimple-update3-release-notes.md)uitvoeren.


> [AZURE.NOTE]
Als u Update 2 toepast of later (inclusief Update 2.1), kunnen Microsoft extra diagnostische informatie van het apparaat te halen. Hierdoor wanneer het team bewerkingen wordt aangegeven welke apparaten problemen ondervindt, zijn we beter uitgerust voor het verzamelen van gegevens van het apparaat en een diagnose stellen bij problemen. Accepteert Update 2 of hoger, kunnen we dit proactief ondersteuning bieden.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Controleer of uw apparaat **StorSimple 8000 reeks Update 3 (6.3.9600.17759)**wordt uitgevoerd. **Laatst bijgewerkt datum** moet ook worden gewijzigd. 

    Als u vanuit een eerdere versie dan Update 2 bijwerken wilt, ook ziet u dat het onderhoud modus updates beschikbaar zijn (dit bericht mogelijk verder moet worden weergegeven voor tot 24 uur nadat u de updates hebt geïnstalleerd).

    Onderhoud modus updates zijn storend updates die resulteren in apparaat downtime en kunnen alleen worden toegepast via de Windows PowerShell-interface van uw apparaat. In sommige gevallen wanneer u de Update 1.2, zijn met uw firmware schijf mogelijk al bijgewerkt, in welke hoofdletters/kleine letters die u niet nodig hebt voor het onderhoud modus updates installeren.

    Als u bijwerken van de Update 2 of hoger, kunnen uw apparaat moet nu zijn bijgewerkt. U kunt de overige stappen overslaan.

13. Download de updates van de modus onderhoud met behulp van de stappen die worden vermeld in [hotfixes downloaden](#to-download-hotfixes) zoeken en downloaden KB3121899, welke installaties schijf firmware-updates (de andere updates moeten al worden geïnstalleerd door nu).

13. Volg de stappen in [installeren en controleer of onderhoud modus hotfixes](#to-install-and-verify-maintenance-mode-hotfixes) de updates van de modus onderhoud te installeren. 

  

## <a name="install-update-3-as-a-hotfix"></a>Update 3 als hotfix installeren

Gebruik deze procedure als u het selectievakje gateway niet bij het installeren van de updates via de portal van Azure klassieke. Het selectievakje mislukt als u een gateway toegewezen aan een niet-gegevens 0 netwerkinterface hebt en uw apparaat een softwareversie vóór Update 1 wordt uitgevoerd.

De softwareversies die kunnen worden bijgewerkt via de methode hotfix zijn:

- 0,1, 0,2, 0,3 bijwerken
- 1, 1.1, 1.2 bijwerken
- 2, 2.1, 2.2 bijwerken 

> [AZURE.IMPORTANT]
>
> - Als uw apparaat wordt uitgevoerd Release (GA) versie, neem contact op met [Microsoft ondersteuning](storsimple-contact-microsoft-support.md) om u te helpen met de update.

De methode hotfix omvat de volgende drie stappen:

1.  Download de hotfixes via de Microsoft Update-catalogus.

2.  Installeren en controleer of de normale modus hotfixes.

3.  Installeren en controleer of de onderhoud modus hotfix (alleen bij het bijwerken van 2 oude Update-software).


#### <a name="download-updates-for-your-device"></a>Updates voor uw apparaat downloaden

**Als uw apparaat Update 2.1 of 2.2 wordt uitgevoerd**, moet u downloaden en installeren van de volgende hotfixes in de aangegeven volgorde:

| Volgorde  | KB        | Beschrijving                    | Type update  | Tijd installeren |
|--------|-----------|-------------------------|------------- |-------------|
| 1.      | KB3186843 | Software-update & #42;  |  Normale <br></br>Niet-storend     | ~ 45 minuten |
| 2.      | KB3186859 | LSI stuurprogramma en firmware             |  Normale <br></br>Niet-storend      | ~ 20 minuten |
| 3.      | KB3121261 | Storport en Spaceport fix </br> Windows Server 2012 R2 |  Normale <br></br>Niet-storend      | ~ 20 minuten |

& #42;  *Notitie, software-update bestaat uit twee binaire bestanden: apparaat software-update voorafgegaan door `all-hcsmdssoftwareupdate` en de Cis en Mds-agent voorafgegaan door `all-cismdsagentupdatebundle`. De software-update van het apparaat moet worden geïnstalleerd voordat de Cis en Mds-agent. U moet ook opnieuw starten met de actieve controller via de `Restart-HcsController` cmdlet na het toepassen van de Cis en Mds-agent bijwerken (en voordat het toepassen van de resterende bijgewerkt).* 


**Als uw apparaat Update 0,1, 0,2, 0,3, 1.0, 1.1, 1.2, of 2.0 wordt uitgevoerd**, moet u downloaden en installeren van de volgende hotfixes naast de software, LSI stuurprogramma en firmware-updates (in de bovenstaande tabel weergegeven), in de aangegeven volgorde:

| Volgorde  | KB        | Beschrijving                    | Type update  | Tijd installeren |
|--------|-----------|-------------------------|------------- |-------------|
| 4.      | KB3146621 | pakket iSCSI | Normale <br></br>Niet-storend  | ~ 20 minuten |
| 5.      | KB3103616 | WMI-pakket |  Normale <br></br>Niet-storend      | ~ 12 minuten |


<br></br>

**Als uw apparaat versies 0,2, 0,3, 1.0, 1.1 en 1.2 wordt uitgevoerd**, moet u mogelijk ook schijf firmware updates boven aan de weergegeven in de voorgaande tabellen updates installeren. U kunt controleren of u de schijf firmware-updates door te voeren moet de `Get-HcsFirmwareVersion` cmdlet. Als u deze firmwareversies: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, klik u niet hoeft te deze updates installeren.


| Volgorde  | KB        | Beschrijving                    | Type update  | Tijd installeren |
|--------|-----------|-------------------------|------------- |-------------|
| 6.      | KB3121899 | Schijf firmware              |  Onderhoud <br></br>Storend      | ~ 30 minuten |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Deze procedure moet slechts één keer worden uitgevoerd om toe te passen Update 3. U kunt de portal van Azure klassieke latere updates toepassen.
> - Als Update 2.2 bijwerkt, wordt de tijd van de totale installeren is dicht bij 1.1 uur.
> - Voordat u deze procedure de update toepassen, zorg dat zowel apparaten online zijn en alle hardwareonderdelen in orde zijn.

Voer de volgende stappen uit om te downloaden en installeren van de hotfixes.

[AZURE.INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [Update 3 release](storsimple-update3-release-notes.md).
