<properties
   pageTitle="Update 2 installeren op uw apparaat StorSimple | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe u StorSimple 8000 reeks Update 2 installeert op uw apparaat van de reeks StorSimple 8000."
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

# <a name="install-update-2-on-your-storsimple-device"></a>Update 2 installeren op uw apparaat StorSimple

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe u Update 2 installeert op een StorSimple-apparaat met een eerdere softwareversie via de portal van Azure klassieke. De zelfstudie behandelt ook de stappen die zijn vereist voor het bijwerken wanneer een gateway is geconfigureerd op een netwerkinterface dan gegevens 0 van het apparaat StorSimple en u wilt bijwerken vanuit een oude Update 1 softwareversie.

Update 2 bevat apparaat software-updates, LSI stuurprogramma updates en schijf firmware-updates. Het apparaatsoftware LSI updates ononderbroken updates en zijn via de portal van Azure klassieke kunnen worden toegepast. De schijf firmware-updates zijn storend updates en kunnen alleen worden toegepast via de Windows PowerShell-interface van het apparaat.

> [AZURE.IMPORTANT]

> -  Er kan geen Update 2 onmiddellijk omdat we een gefaseerd uitrol van de updates doen. Zoeken naar updates in een paar dagen weer als deze Update beschikbaar komen binnenkort.
> - Een reeks handmatige en automatische oude controles wordt uitgevoerd voordat u de installatie om te bepalen de status van het apparaat met hardware provincie en de netwerkconnectiviteit. Deze oude controles uitgevoerd alleen als u de updates vanaf de portal van Azure klassieke toepassen.
> - Het is raadzaam dat u de software en stuurprogramma's via de portal van Azure klassieke installeren. U moet alleen Ga naar de Windows PowerShell-interface van het apparaat (voor het installeren van updates) als het selectievakje oude update gateway is mislukt in de portal. De updates duurt 4 tot en met 7-uren te installeren (inclusief de Windows-Updates). De updates van de modus onderhoud moeten worden geïnstalleerd via de Windows PowerShell-interface van het apparaat. Zoals onderhoud modus updates storend updates zijn, resulteert dit in een tijd voor uw apparaat.
> - Als het optionele StorSimple momentopname Manager uitgevoerd, moet u controleert u uw momentopname Manager-versie hebt bijgewerkt naar de Update 2 vóór het bijwerken van het apparaat.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-the-azure-classic-portal"></a>2 hebt geïnstalleerd via de portal van Azure klassieke

De volgende stappen als u wilt bijwerken van uw apparaat met [2 bijwerken](storsimple-update2-release-notes.md)uitvoeren.


> [AZURE.NOTE]
Update 2 zorgt ervoor dat Microsoft om op te halen extra diagnostische informatie van het apparaat. Hierdoor wanneer het team bewerkingen wordt aangegeven welke apparaten problemen ondervindt, zijn we beter uitgerust voor het verzamelen van gegevens van het apparaat en een diagnose stellen bij problemen. Update 2 accepteert, kunnen we dit proactief ondersteuning bieden.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Controleer of uw apparaat **StorSimple 8000 reeks Update 2 (6.3.9600.17673)**wordt uitgevoerd. **Laatst bijgewerkt datum** moet ook worden gewijzigd. U ziet ook dat onderhoud modus updates beschikbaar zijn (dit bericht mogelijk verder moet worden weergegeven voor tot 24 uur nadat u de updates hebt geïnstalleerd).

    Onderhoud modus updates zijn storend updates die resulteren in apparaat downtime en kunnen alleen worden toegepast via de Windows PowerShell-interface van uw apparaat. In sommige gevallen wanneer u de Update 1.2, zijn met uw firmware schijf mogelijk al bijgewerkt, in welke hoofdletters/kleine letters die u niet nodig hebt voor het onderhoud modus updates installeren.

13. Download de updates van de modus onderhoud met behulp van de stappen die worden vermeld in [hotfixes downloaden](#to-download-hotfixes) zoeken en downloaden KB3121899, welke installaties schijf firmware-updates (de andere updates moeten al worden geïnstalleerd door nu).

13. Volg de stappen in [installeren en controleer of onderhoud modus hotfixes](#to-install-and-verify-maintenance-mode-hotfixes) de updates van de modus onderhoud te installeren.


## <a name="install-update-2-as-a-hotfix"></a>Update 2 als hotfix installeren

Gebruik deze procedure als u het selectievakje gateway niet bij het installeren van de updates via de portal van Azure klassieke. Het selectievakje mislukt als u een gateway toegewezen aan een niet-gegevens 0 netwerkinterface hebt en uw apparaat een softwareversie vóór Update 1 wordt uitgevoerd.

De softwareversies die kunnen worden bijgewerkt met de methode hotfix zijn Update 0,1, Update 0,2 en Update 0,3 Update 1, Update 1.1 en Update 1.2. De methode hotfix omvat de volgende drie stappen:

- Download de hotfixes via de Microsoft Update-catalogus.
- Installeren en controleer of de normale modus hotfixes.
- Installeren en controleer of de hotfix onderhoud-modus.

Als u wilt bijwerken 2 als hotfix hebt geïnstalleerd, moet u downloaden en installeren van de volgende hotfixes:

| Volgorde  | KB        | Beschrijving                    | Type update  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3121901 | Software-update         |  Normale     |
| 2      | KB3121900 | LSI-stuurprogramma              |  Normale     |
| 3      | KB3080728 | Storport fix </br> Windows Server 2012 R2 |  Normale     |
| 4      | KB3090322 | Spaceport fix </br> Windows Server 2012 R2 |  Normale     |
| 5      | KB3121899 | Schijf firmware           | Onderhoud  |


> [AZURE.IMPORTANT]
>
> - Als uw apparaat wordt uitgevoerd Release (GA) versie, neem contact op met [Microsoft ondersteuning](storsimple-contact-microsoft-support.md) om u te helpen met de update.
> - Deze procedure moet slechts één keer worden uitgevoerd om toe te passen Update 2. U kunt de portal van Azure klassieke latere updates toepassen.
> - Elke hotfix-installatie kunt ongeveer 20 minuten duren om te voltooien. Totale installatie lijkt op 2 uur.
> - Voordat u deze procedure de update toepassen, zorg dat beide apparaatcontrollers online en alle hardwareonderdelen in orde zijn.

De volgende stappen als u wilt deze update als hotfix toepassen uitvoeren.

[AZURE.INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]



## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [Update 2 release](storsimple-update2-release-notes.md).
