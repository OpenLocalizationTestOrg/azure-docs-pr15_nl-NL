<properties
   pageTitle="Het volume van de StorSimple klonen | Microsoft Azure"
   description="Beschrijft de verschillende klonen typen en wanneer ze worden gebruikt en wordt uitgelegd hoe u een back-up ingesteld op een afzonderlijke volume klonen kunt gebruiken."
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
   ms.date="07/27/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a>Gebruik van de service StorSimple Manager klonen een volume (Update 2)

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Overzicht

De pagina StorSimple Manager service **Back-catalogus** wordt weergegeven de back-sets die worden gemaakt wanneer handmatige of automatische back-ups zijn die u hebt gemaakt. U kunt deze pagina alle de back-ups weergeven voor een back-beleid of een volume, selecteren of verwijderen van back-ups gebruikt of een back-up gebruiken om te zetten of een volume klonen.

![Back-up cataloguspagina](./media/storsimple-clone-volume-u2/backupCatalog.png)  

Deze zelfstudie wordt beschreven hoe u een back-up ingesteld op een afzonderlijke volume klonen kunt gebruiken. Ook wordt het verschil tussen *tijdelijke* en *permanente* klonen uitgelegd.

>[AZURE.NOTE] 
>
>Een lokaal vastgemaakte volume wordt als een doorverbonden volume worden gekopieerd. Als u het volume van de gekloonde voor lokaal is vastgemaakt nodig hebt, kunt u de klonen converteren naar een lokaal vastgemaakte volume nadat de klonen bewerking is voltooid. Ga naar [het volumetype wijzigen](storsimple-manage-volumes-u2.md#change-the-volume-type)voor informatie over het converteren van een doorverbonden volume naar een lokaal vastgemaakte volume.
>
>Als u probeert een gekloonde volume uit converteren mislukt doorverbonden naar lokaal vastgemaakt direct na klonen (als deze nog steeds een tijdelijke klonen is), de conversie met het volgende foutbericht weergegeven:
>
>`Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
>
>Deze fout wordt ontvangen alleen als u met een ander apparaat zijn klonen. U kunt het volume lokaal vastgemaakt als u eerst de tijdelijke klonen converteren naar een permanente klonen correct worden geconverteerd. Als u wilt de tijdelijke klonen omzetten in een permanente klonen, een momentopname cloud in ervan.

## <a name="create-a-clone-of-a-volume"></a>Een volume maken

U kunt een klonen op hetzelfde apparaat, een ander apparaat of zelfs een virtuele machine maken met behulp van een lokaal of een momentopname van de cloud.

#### <a name="to-clone-a-volume"></a>Een volume klonen

1. Klik op het tabblad **back-up-catalogus** en selecteer een back-ups instellen op de pagina van de service StorSimple Manager.

2. Vouw de back-up instellen om weer te geven van de bijbehorende hoeveelheden. Klik op en selecteert u een volume van de back-ups instellen.

     ![Een volume klonen](./media/storsimple-clone-volume-u2/CloneVol.png) 

3. Klik op **klonen** om te beginnen met het geselecteerde volume klonen.

4. Klik in de wizard klonen Volume onder **locatie en naam opgeven**:

  1. Geef aan dat een apparaat. Dit is de locatie waar de klonen worden gemaakt. U kunt hetzelfde apparaat kiezen of een ander apparaat. Als u een volume dat is gekoppeld aan andere serviceproviders cloud kiezen (niet Azure), de vervolgkeuzelijst voor het doelapparaat wordt alleen weergegeven voor fysieke apparaten. U kunt een volume dat is gekoppeld aan andere serviceproviders cloud op een virtueel apparaat kan niet klonen.

        >[AZURE.NOTE] Zorg ervoor dat de vereiste capaciteit voor de klonen lager dan de capaciteit beschikbaar op het doelapparaat is.

  2. Geef een unieke volumenaam voor uw klonen. De naam moet tussen 3 en 127 tekens bevatten. 
    
        >[AZURE.NOTE] Het veld **Klonen Volume als** is **Tiered** zelfs als u een lokaal vastgemaakte volume zijn klonen. U kunt deze instelling; niet wijzigen echter als u het gekloonde volume ook lokaal is vastgemaakt nodig hebt, kunt u converteren de klonen naar een lokaal vastgemaakte volume nadat u de klonen voor de knop ongedaan maken. Ga naar [het volumetype wijzigen](storsimple-manage-volumes-u2.md#change-the-volume-type)voor informatie over het converteren van een doorverbonden volume naar een lokaal vastgemaakte volume.

        ![Klonen wizard 1](./media/storsimple-clone-volume-u2/clone1.png) 

  3. Klik op het pijlpictogram ![pijl-pictogram](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) om verder te gaan naar de volgende pagina.

5. Onder **Geef hosts die dit volume kunnen gebruiken**:

  1. Geef een access control-record (ACR) voor de klonen. U kunt een nieuwe ACR toevoegen of Kies in de bestaande lijst.

        ![Klonen wizard 2](./media/storsimple-clone-volume-u2/clone2.png) 

  2. Klik op het pictogram controleren ![selectievakje-pictogram](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)aan de bewerking niet voltooien.

6. Een taak klonen wordt gestart en u wordt geïnformeerd wanneer de klonen is gemaakt. Klik op **Taak weergeven** als u wilt controleren van de taak klonen op de pagina **taken** . U kunt het volgende bericht wordt weergegeven, wanneer de klonen taak is voltooid:

    ![Klonen bericht](./media/storsimple-clone-volume-u2/CloneMsg.png) 

7. Nadat de klonen taak is voltooid:

  1. Ga naar de pagina **apparaten** en selecteer het tabblad **Volume Containers** . 
  2. Selecteer de volume-container die is gekoppeld aan het bronvolume van de die u gekopieerd. Klik in de lijst van hoeveelheden ziet u de klonen die is gemaakt.

>[AZURE.NOTE] Cmdlets voor controle en standaard back-up worden automatisch uitgeschakeld op een gekloonde volume.

Een klonen die u deze methode maakt is een tijdelijke klonen. Voor meer informatie over klonen typen, raadpleegt u [tijdelijke versus permanente klonen](#transient-vs.-permanent-clones).

Deze klonen is nu een normale volume en elke bewerking die is het mogelijk dat op een volume is beschikbaar voor de klonen. U moet deze het volume van een back-ups configureren.

## <a name="transient-vs-permanent-clones"></a>Tijdelijk versus permanente klonen

Tijdelijke klonen worden alleen gemaakt wanneer u naar een ander apparaat zijn klonen. U kunt het volume van een specifieke uit een back-up ingesteld op een ander apparaat beheerd door de StorSimple Manager klonen. De tijdelijke klonen heeft verwijzingen naar de gegevens in het oorspronkelijke volume en deze gegevens gebruiken om te lezen en schrijven lokaal op het doelapparaat. 

Nadat u een cloud momentopname van een tijdelijke klonen, worden de resulterende klonen een *permanente* klonen. Tijdens dit proces wordt een kopie van de gegevens in de cloud gemaakt en de tijd voor het kopiëren van deze gegevens wordt bepaald door de grootte van de gegevens en de Azure vertragingstijden (dit is een kopie van de Azure-naar-Azure). Dit proces kan dagen naar weken duren. De tijdelijke klonen verandert in een permanente klonen deze manier en geen verwijzingen naar de oorspronkelijke volumegegevens die deze is gekopieerd. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scenario's voor tijdelijke en permanente klonen

De volgende gedeelten wordt beschreven voorbeeld situaties waarin tijdelijke en permanente klonen kunnen worden gebruikt.

### <a name="item-level-recovery-with-a-transient-clone"></a>Itemniveau herstel met een tijdelijke klonen

Moet u één jaar oud Microsoft PowerPoint-presentatie-bestand herstellen. Uw IT-beheerder de specifieke back-up van die periode aangeeft en klikt u vervolgens het volume worden gefilterd. De beheerder vervolgens het volume klonen, wordt gezocht naar het bestand dat u zoekt en wordt dit aangeboden aan u. In dit scenario wordt een tijdelijke klonen gebruikt. 
 
![Video beschikbaar](./media/storsimple-clone-volume-u2/Video_icon.png) **Video beschikbaar**

Klik op een video die laat zien hoe u kunt de klonen gebruiken en herstellen van functies in StorSimple herstellen van verwijderde bestanden bekijken [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>In de productieomgeving met een permanente klonen testen

U moet controleren of een testen fout in de productieomgeving. U het volume maken in de productieomgeving en maakt u een momentopname cloud van deze klonen om een onafhankelijke gekloonde volume te maken. In dit scenario wordt een permanente klonen gebruikt.  

## <a name="next-steps"></a>Volgende stappen
- Leer hoe u [een volume StorSimple uit een back-up set herstelt](storsimple-restore-from-backup-set-u2.md).

- Informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).

 
