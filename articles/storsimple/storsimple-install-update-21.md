<properties
   pageTitle="Update 2.2 installeren op uw apparaat StorSimple | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe u StorSimple 8000 reeks Update 2.2 installeert op uw apparaat van de reeks StorSimple 8000."
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
   ms.date="08/02/2016"
   ms.author="alkohli" />

# <a name="install-update-22-on-your-storsimple-device"></a>Update 2.2 installeren op uw apparaat StorSimple

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe u Update 2.2 installeert op een apparaat StorSimple met een eerdere softwareversie via de portal van Azure klassieke en de methode hotfix. De methode hotfix wordt gebruikt wanneer een gateway is geconfigureerd op een netwerkinterface dan gegevens 0 van het apparaat StorSimple en u wilt bijwerken vanuit een oude Update 1 softwareversie.

Update 2.2 bevat apparaatsoftware, WMI en iSCSI-updates. Als het bijwerken van versie 2.1, moet alleen het apparaat software-update worden toegepast. Als bijwerken vanuit een oude Update 2-versie, wordt u ook vereist om toe te passen LSI stuurprogramma, Spaceport Storport en schijf firmware-updates. Het apparaatsoftware, WMI iSCSI, LSI stuurprogramma, Spaceport en Storport correcties ononderbroken updates zijn en via de portal van Azure klassieke kunnen worden toegepast. De schijf firmware-updates zijn storend updates en kunnen alleen worden toegepast via de Windows PowerShell-interface van het apparaat. 

> [AZURE.IMPORTANT]

> - Een reeks handmatige en automatische oude controles wordt uitgevoerd voordat u de installatie om te bepalen de status van het apparaat met hardware provincie en de netwerkconnectiviteit. Deze oude controles uitgevoerd alleen als u de updates vanaf de portal van Azure klassieke toepassen.
> - Het is raadzaam dat u de software en stuurprogramma's via de portal van Azure klassieke installeren. U moet alleen Ga naar de Windows PowerShell-interface van het apparaat (voor het installeren van updates) als het selectievakje oude update gateway is mislukt in de portal. Afhankelijk van de versie die u bijwerkt, duurt de updates 1,5-2,5 uur te installeren. De updates van de modus onderhoud moeten worden geïnstalleerd via de Windows PowerShell-interface van het apparaat. Zoals onderhoud modus updates storend updates zijn, resulteert dit in een tijd voor uw apparaat.
> - Als het optionele StorSimple momentopname Manager uitgevoerd, moet u controleert u uw momentopname Manager-versie hebt bijgewerkt naar de Update 2.2 vóór het bijwerken van het apparaat.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-22-via-the-azure-classic-portal"></a>Update 2.2 installeren via de portal van Azure klassieke

De volgende stappen als u wilt bijwerken van uw apparaat aan [2.2 bijwerken](storsimple-update21-release-notes.md)uitvoeren.


> [AZURE.NOTE]
Als u Update 2 toepast of later (inclusief Update 2.1), kunnen Microsoft extra diagnostische informatie van het apparaat te halen. Hierdoor wanneer het team bewerkingen wordt aangegeven welke apparaten problemen ondervindt, zijn we beter uitgerust voor het verzamelen van gegevens van het apparaat en een diagnose stellen bij problemen. Accepteert Update 2 of hoger, kunnen we dit proactief ondersteuning bieden.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Controleer of uw apparaat **StorSimple 8000 reeks Update 2.2 (6.3.9600.17708)**wordt uitgevoerd. **Laatst bijgewerkt datum** moet ook worden gewijzigd. 

    Als u vanuit een eerdere versie dan Update 2 bijwerken wilt, ook ziet u dat het onderhoud modus updates beschikbaar zijn (dit bericht mogelijk verder moet worden weergegeven voor tot 24 uur nadat u de updates hebt geïnstalleerd).

    Onderhoud modus updates zijn storend updates die resulteren in apparaat downtime en kunnen alleen worden toegepast via de Windows PowerShell-interface van uw apparaat. In sommige gevallen wanneer u de Update 1.2, zijn met uw firmware schijf mogelijk al bijgewerkt, in welke hoofdletters/kleine letters die u niet nodig hebt voor het onderhoud modus updates installeren.

    Als u van de Update 2 bijwerken wilt, kunnen uw apparaat moet nu zijn bijgewerkt. U kunt de overige stappen overslaan.

13. Download de updates van de modus onderhoud met behulp van de stappen die worden vermeld in [hotfixes downloaden](#to-download-hotfixes) zoeken en downloaden KB3121899, welke installaties schijf firmware-updates (de andere updates moeten al worden geïnstalleerd door nu).

13. Volg de stappen in [installeren en controleer of onderhoud modus hotfixes](#to-install-and-verify-maintenance-mode-hotfixes) de updates van de modus onderhoud te installeren. 

  

## <a name="install-update-22-as-a-hotfix"></a>Update 2.2 als hotfix installeren

Gebruik deze procedure als u het selectievakje gateway niet bij het installeren van de updates via de portal van Azure klassieke. Het selectievakje mislukt als u een gateway toegewezen aan een niet-gegevens 0 netwerkinterface hebt en uw apparaat een softwareversie vóór Update 1 wordt uitgevoerd.

De softwareversies die kunnen worden bijgewerkt via de methode hotfix zijn:

- 0,1, 0,2, 0,3 bijwerken
- 1, 1.1, 1.2 bijwerken
- Update 2, 2.1 

> [AZURE.IMPORTANT]
>
> - Als uw apparaat wordt uitgevoerd Release (GA) versie, neem contact op met [Microsoft ondersteuning](storsimple-contact-microsoft-support.md) om u te helpen met de update.

De methode hotfix omvat de volgende drie stappen:

- Download de hotfixes via de Microsoft Update-catalogus.
- Installeren en controleer of de normale modus hotfixes.
- Installeren en controleer of de onderhoud modus hotfix (alleen bij het bijwerken van 2 oude Update-software).

#### <a name="download-updates-for-a-device-running-update-21-software"></a>Updates voor een apparaat met 2.1 Update-software downloaden

**Als uw apparaat wordt uitgevoerd 2.1 bijwerken**, moet u alleen het apparaat software-update KB3179904 downloaden. Alleen de binair bestand voorafgegaan door 'alle-hcsmdssoftwareudpate' installeren. Installeer de Cis niet en de update van de agent MDS voorafgegaan door `all-cismdsagentupdatebundle`. Er treden niet doet in een fout. Dit is een ononderbroken update, IO wordt niet onderbroken en het apparaat hebben geen elke uitvaltijd.


#### <a name="download-updates-for-a-device-running-update-2-software"></a>Updates voor een apparaat met Update 2-software downloaden

**Als uw apparaat Update 2 wordt uitgevoerd**, moet u downloaden en installeren van de volgende hotfixes in de aangegeven volgorde:

| Volgorde  | KB        | Beschrijving                    | Type update  | Tijd installeren |
|--------|-----------|-------------------------|------------- |-------------|
| 1.      | KB3179904 | Software-update & #42;  |  Normale <br></br>Niet-storend     | ~ 45 minuten |
| 2.      | KB3146621 | pakket iSCSI | Normale <br></br>Niet-storend  | ~ 20 minuten |
| 3.      | KB3103616 | WMI-pakket |  Normale <br></br>Niet-storend      | ~ 12 minuten |


 & #42;  *Notitie, software-update bestaat uit twee binaire bestanden: apparaat software-update voorafgegaan door `all-hcsmdssoftwareupdate` en de Cis en Mds-agent voorafgegaan door `all-cismdsagentupdatebundle`. De software-update van het apparaat moet worden geïnstalleerd voordat de Cis en Mds-agent. U moet ook opnieuw starten met de actieve controller via de `Restart-HcsController` cmdlet na het toepassen van de Cis en MDS-agent bijwerken (en voordat het toepassen van de resterende bijgewerkt).* 

#### <a name="download-updates-for-a-device-running-pre-update-2-software"></a>Updates voor een apparaat met 2 oude Update-software downloaden

**Als uw apparaat versies 0,2, 0,3, 1.0, en 1.1 wordt uitgevoerd**, moet u downloaden en installeren de LSI stuurprogramma en firmware bijwerken naast de software, iSCSI en WMI-updates. Deze update is al geïnstalleerd als u werkt met 1.2 bijwerken of 2. 
 
| Volgorde  | KB        | Beschrijving                    | Type update  | Tijd installeren |
|--------|-----------|-------------------------|------------- |-------------|
| 4.      | KB3121900 | LSI stuurprogramma en firmware             |  Normale <br></br>Niet-storend      | ~ 20 minuten |


<br></br>
**Als uw apparaat versies 0,2, 0,3, 1.0, 1.1 en 1.2 wordt uitgevoerd**, moet u downloaden en installeren van de Spaceport en het Storport repareren. Deze zijn al geïnstalleerd als u Update 2 uitvoert.

| Volgorde  | KB        | Beschrijving                    | Type update  | Tijd installeren |
|--------|-----------|-------------------------|------------- |-------------|
| 5.      | KB3090322 | Spaceport fix </br> Windows Server 2012 R2 |  Normale <br></br>Niet-storend      | ~ 20 minuten |
| 6.      | KB3080728 | Storport fix </br> Windows Server 2012 R2 |  Normale <br></br>Niet-storend      | ~ 20 minuten |



<br></br>
Mogelijk moet u ook schijf firmware-updates installeren. U kunt controleren of u de schijf firmware-updates door te voeren moet de `Get-HcsFirmwareVersion` cmdlet. Als u deze firmwareversies: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, klik u niet hoeft te deze updates installeren.


| Volgorde  | KB        | Beschrijving                    | Type update  | Tijd installeren |
|--------|-----------|-------------------------|------------- |-------------|
| 7.      | KB3121899 | Schijf firmware              |  Onderhoud <br></br>Storend      | ~ 30 minuten |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Deze procedure moet slechts één keer worden uitgevoerd als u wilt bijwerken 2.2 toepassen. U kunt de portal van Azure klassieke latere updates toepassen.
> - Als het bijwerken van de Update 2, wordt de tijd van de totale installeren is dicht bij 1,5 uur.
> - Voordat u deze procedure de update toepassen, zorg dat zowel apparaten online zijn en alle hardwareonderdelen in orde zijn.

Voer de volgende stappen uit om te downloaden en installeren van de hotfixes.

[AZURE.INCLUDE [storsimple-install-update21-hotfix](../../includes/storsimple-install-update21-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [Update 2.1 release](storsimple-update21-release-notes.md).
