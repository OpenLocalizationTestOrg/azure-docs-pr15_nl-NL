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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume"></a>De StorSimple Manager-service gebruiken om een volume klonen

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Overzicht

De pagina StorSimple Manager service **Back-catalogus** wordt weergegeven de back-sets die worden gemaakt wanneer handmatige of automatische back-ups zijn die u hebt gemaakt. U kunt deze pagina alle de back-ups weergeven voor een back-beleid of een volume, selecteren of verwijderen van back-ups gebruikt of een back-up gebruiken om te zetten of een volume klonen.

![Back-up cataloguspagina](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

Deze zelfstudie wordt beschreven hoe u een back-up ingesteld op een afzonderlijke volume klonen kunt gebruiken. Ook wordt het verschil tussen *tijdelijke* en *permanente* klonen uitgelegd. 

## <a name="create-a-clone-of-a-volume"></a>Een volume maken

U kunt een klonen op hetzelfde apparaat, een ander apparaat of zelfs een virtuele machine maken met behulp van een lokale of een momentopname van de cloud.

#### <a name="to-clone-a-volume"></a>Een volume klonen

1. Klik op het tabblad **back-up-catalogus** en selecteer een back-ups instellen op de pagina van de service StorSimple Manager.

2. Vouw de back-up instellen om weer te geven van de bijbehorende hoeveelheden. Klik op en selecteert u een volume van de back-ups instellen.

     ![Een volume klonen](./media/storsimple-clone-volume/HCS_Clone.png) 

3. Klik op **klonen** om te beginnen met het geselecteerde volume klonen.

4. Klik in de wizard klonen Volume onder **locatie en naam opgeven**:

  1. Geef aan dat een apparaat. Dit is de locatie waar de klonen worden gemaakt. U kunt hetzelfde apparaat kiezen of een ander apparaat. Als u een volume dat is gekoppeld aan andere serviceproviders cloud kiezen (niet Azure), de vervolgkeuzelijst voor het doelapparaat wordt alleen weergegeven voor fysieke apparaten. U kunt een volume dat is gekoppeld aan andere serviceproviders cloud op een virtueel apparaat kan niet klonen.

        >  [AZURE.NOTE] Zorg ervoor dat de vereiste capaciteit voor de klonen lager dan de capaciteit beschikbaar op het doelapparaat is.
  2. Geef een unieke volumenaam voor uw klonen. De naam moet tussen 3 en 127 tekens bevatten.
  3. Klik op het pijlpictogram ![pijl-pictogram](./media/storsimple-clone-volume/HCS_ArrowIcon.png) om verder te gaan naar de volgende pagina.

5. Onder **Geef hosts die dit volume kunnen gebruiken**:

  1. Geef een access control-record (ACR) voor de klonen. U kunt een nieuwe ACR toevoegen of Kies in de bestaande lijst.
  2. Klik op het pictogram controleren ![selectievakje-pictogram](./media/storsimple-clone-volume/HCS_CheckIcon.png)aan de bewerking niet voltooien.

6. Een taak klonen wordt gestart en u wordt geïnformeerd wanneer de klonen is gemaakt. Klik op **Taak weergeven** als u wilt controleren van de taak klonen op de pagina **taken** .

7. Nadat de klonen taak is voltooid:

  1. Ga naar de pagina **apparaten** en selecteer het tabblad **Volume Containers** . 
  2. Selecteer de volume-container die is gekoppeld aan het bronvolume van de die u gekopieerd. Klik in de lijst van hoeveelheden ziet u de klonen die is gemaakt.

>[AZURE.NOTE] Cmdlets voor controle en standaard back-up worden automatisch uitgeschakeld op een gekloonde volume.

Een klonen die u deze methode maakt is een tijdelijke klonen. Voor meer informatie over klonen typen, raadpleegt u [tijdelijke versus permanente klonen](#transient-vs.-permanent-clones).

Deze klonen is nu een normale volume en elke bewerking die is het mogelijk dat op een volume is beschikbaar voor de klonen. U moet deze het volume van een back-ups configureren.

## <a name="transient-vs-permanent-clones"></a>Tijdelijk versus permanente klonen

Tijdelijke en permanente klonen worden alleen gemaakt wanneer u met een ander apparaat zijn klonen. U kunt het volume van een specifieke uit een back-up ingesteld op een ander apparaat klonen. Een kopie gemaakt op deze manier is een *tijdelijke* klonen. De tijdelijke klonen verwijzingen naar het oorspronkelijke volume heeft en dat volume te lezen bij het schrijven van lokaal gebruikt. 

Nadat u een cloud momentopname van een tijdelijke klonen, worden de resulterende klonen een *permanente* klonen. De permanente klonen onafhankelijk is en geen verwijzingen naar de oorspronkelijke volume waarop deze is gekopieerd.  

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scenario's voor tijdelijke en permanente klonen

De volgende gedeelten wordt beschreven voorbeeld situaties waarin tijdelijke en permanente klonen kunnen worden gebruikt.

### <a name="item-level-recovery-with-a-transient-clone"></a>Itemniveau herstel met een tijdelijke klonen

Moet u één jaar oud Microsoft PowerPoint-presentatie-bestand herstellen. Uw IT-beheerder de specifieke back-up van die periode aangeeft en klikt u vervolgens het volume worden gefilterd. De beheerder vervolgens het volume klonen, wordt gezocht naar het bestand dat u zoekt en wordt dit aangeboden aan u. In dit scenario wordt een tijdelijke klonen gebruikt. 
 
![Video beschikbaar](./media/storsimple-clone-volume/Video_icon.png) **Video beschikbaar**

Klik op een video die laat zien hoe u kunt de klonen gebruiken en herstellen van functies in StorSimple herstellen van verwijderde bestanden bekijken [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>In de productieomgeving met een permanente klonen testen

U moet controleren of een testen fout in de productieomgeving. Maakt u een kopie van het volume in de productieomgeving door een momentopname cloud van deze klonen. Het volume van de gekloonde is nu onafhankelijke. In dit scenario wordt een permanente klonen gebruikt.

## <a name="next-steps"></a>Volgende stappen
- Leer hoe u [een volume StorSimple uit een back-up set herstelt](storsimple-restore-from-backup-set.md).

- Informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).

 
