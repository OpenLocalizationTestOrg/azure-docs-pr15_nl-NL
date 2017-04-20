<properties 
   pageTitle="Beheer uw back-catalogus StorSimple | Microsoft Azure"
   description="Wordt uitgelegd hoe u met de cataloguspagina StorSimple Manager service back-lijst, selecteert u en back-sets voor een volume verwijderen."
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
   ms.date="04/28/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>Gebruik van de StorSimple Manager-service voor het beheren van uw back-catalogus

## <a name="overview"></a>Overzicht

De pagina StorSimple Manager service **Back-catalogus** wordt weergegeven de back-sets die worden gemaakt wanneer de back-ups van een handmatige of geplande zijn die u hebt gemaakt. U kunt deze pagina alle de back-ups weergeven voor een back-beleid of een volume, selecteren of verwijderen van back-ups gebruikt of een back-up gebruiken om te zetten of een volume klonen.

Deze zelfstudie wordt uitgelegd hoe u een lijst met, selecteren en verwijderen van een back-ups instellen. Informatie over het herstellen van uw apparaat van de back-up maken, gaat u naar [uw apparaat uit een back-set herstellen](storsimple-restore-from-backup-set.md). Als u wilt weten hoe u een volume klonen, gaat u naar [een volume StorSimple klonen](storsimple-clone-volume.md).

![Back-catalogus](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

De pagina **Back-catalogus** vindt u een query toevoegen aan het beperken van de back-up selectie instellen. U kunt de back-sets die worden opgehaald, op basis van de volgende parameters filteren:

- **Apparaat** – het apparaat waarop u de back-set is gemaakt.

- **Back-beleid of Volume** – de back-beleid of het volume dat is gekoppeld aan dit back-ups instellen.

- **Van en tot** – het bereik datum en tijd als de set back-up is gemaakt.

De gefilterde back-sets worden vervolgens de tabelindeling op basis van de volgende kenmerken:

- **Naam** : de naam van de back-beleid of het volume dat is gekoppeld aan de back-ups instellen.

- **Grootte** – de werkelijke grootte van de back-ups instellen.

- **Gemaakt op** de datum en tijd waarop de back-ups zijn gemaakt. 

- **Type** , back-up sets kunnen worden lokale momentopnamen of momentopnamen cloud. Een lokale momentopname is een back-up van al uw volumegegevens lokaal opgeslagen op het apparaat, dat een momentopname cloud verwijst naar de back-up van het van volumegegevens in de cloud. Lokale momentopnamen bieden snellere toegang tot dat cloud momentopnamen voor tolerantie gegevens zijn geselecteerd.

- **Gestart door** – de back-ups automatisch kunnen worden gestart door een planning of handmatig door een gebruiker. U kunt een back-beleid back-ups plannen. U kunt ook de optie **back-up maken** een handmatige back-up uitvoeren.

## <a name="list-backup-sets-for-a-volume"></a>Lijst met back-up wordt ingesteld voor een volume
 
Voer de volgende stappen uit om alle de back-ups weergeven voor een volume.

#### <a name="to-list-backup-sets"></a>Aan de lijst back-sets

1. Klik op het tabblad **back-up-catalogus** op de pagina van de service StorSimple Manager.

2. Als volgt te werk om de selecties te filteren:

    1. Selecteer het gewenste apparaat.

    2. Kies een volume om weer te geven de bijbehorende de back-ups in de vervolgkeuzelijst.

    3. Geef het tijdsbereik.

    4. Klik op het pictogram controleren ![Pictogram controleren](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) naar het uitvoeren van deze query.
 
    De back-ups die is gekoppeld aan het geselecteerde volume moeten worden weergegeven in de lijst met back-sets.

## <a name="select-a-backup-set"></a>Selecteer een back-ups instellen

De volgende stappen om te selecteren van een back-up instellen voor een volume of back-beleid.

#### <a name="to-select-a-backup-set"></a>Een back-ups instellen selecteren

1. Klik op het tabblad **back-up-catalogus** op de pagina van de service StorSimple Manager.

2. Als volgt te werk om de selecties te filteren:

    1. Selecteer het gewenste apparaat.

    2. Kies het volume of back-up-beleid voor de back-up die u wilt selecteren in de vervolgkeuzelijst.

    3. Geef het tijdsbereik.

    4. Klik op het pictogram controleren ![Pictogram controleren](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) naar het uitvoeren van deze query.

    De back-ups die is gekoppeld aan het geselecteerde volume of back-beleid moet worden weergegeven in de lijst met back-sets.

3. Selecteer en vouwen van een back-ups instellen. De opties **herstellen** en **verwijderen** worden onder aan de pagina weergegeven. U kunt een van deze acties uitvoeren op de back-ups instellen die u hebt geselecteerd.

## <a name="delete-a-backup-set"></a>Een back-reeks verwijderen

Een back-up verwijderen wanneer u niet langer wilt bewaren van de gegevens die zijn gekoppeld. De volgende stappen als u wilt verwijderen van een back-ups instellen uitvoeren.

#### <a name="to-delete-a-backup-set"></a>Verwijderen van een back-ups instellen

1. Klik op het **tabblad van de back-up-catalogus**op de pagina van de service StorSimple Manager.

2. Als volgt te werk om de selecties te filteren:

    1. Selecteer het gewenste apparaat.

    2. Kies het volume of back-up-beleid voor de back-up die u wilt selecteren in de vervolgkeuzelijst.

    3. Geef het tijdsbereik.

    4. Klik op het pictogram controleren ![Pictogram controleren](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) naar het uitvoeren van deze query.

    De back-ups die is gekoppeld aan het geselecteerde volume of back-beleid moet worden weergegeven in de lijst met back-sets.

3. Selecteer en vouwen van een back-ups instellen. De opties **herstellen** en **verwijderen** worden onder aan de pagina weergegeven. Klik op **verwijderen**.

4. U krijgt wanneer de verwijdering in uitvoering en wanneer deze is voltooid. Wanneer de verwijdering klaar is, kunt u de query op deze pagina vernieuwen. De verwijderde back-ups instellen wordt niet meer weergegeven in de lijst met back-sets.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [gebruik van de back-catalogus als u wilt herstellen van uw apparaat uit een back-set](storsimple-restore-from-backup-set.md).

- Informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
