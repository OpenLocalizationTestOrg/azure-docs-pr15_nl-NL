<properties 
   pageTitle="Weergeven en beheren van StorSimple taken | Microsoft Azure"
   description="Beschrijving van de pagina StorSimple Manager service taken en hoe gebruiken voor het bijhouden van recente, huidige en geplande back-taken."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs-update-2"></a>Gebruik van de service StorSimple Manager weergeven en beheren van StorSimple taken (Update 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Overzicht

De pagina **taken** biedt één centrale portal voor weergeven en beheren van taken die zijn gestart op apparaten verbonden met uw StorSimple Manager-service. U kunt geplande, actieve voltooide, geannuleerde en mislukte taken voor meerdere apparaten weergeven. Resultaten worden weergegeven in een tabelvormige indeling. 

![Pagina taken](./media/storsimple-manage-jobs-u2/jobs.png)

U vindt snel de taken die u bent geïnteresseerd door te filteren op velden, zoals:

- **Status** : taken kunnen worden uitgevoerd, voltooid, geannuleerd, is mislukt, annuleren of voltooid met fouten.
- **Van en tot** – taken kunnen worden gefilterd op basis van het bereik van de datum en tijd.
- **Type** : het taaktype kan zijn back-up en handmatig back-up, herstellen, klonen, failover-apparaat, lokaal vastgemaakte volume maken, wijzigen volume, bijwerken en pakket of inrichting van virtueel apparaat te ondersteunen.

- **Apparaten** – taken zijn gestart op een bepaalde apparaat verbonden met uw service.
De gefilterde taken worden vervolgens de tabelindeling op basis van de volgende kenmerken:

    - **Type** , back-up en handmatig back-up, herstellen, klonen, failover-apparaat, lokaal vastgemaakte volume maken, wijzigen volume, bijwerken en pakket of inrichting van virtueel apparaat te ondersteunen.

    - **Status** – uitgevoerd, voltooid, geannuleerd, is mislukt, annuleren of voltooid met fouten.

    - **Entiteit** – de taken kunnen worden gekoppeld aan een volume, een back-beleid of een apparaat. Een taak klonen is bijvoorbeeld gekoppeld aan een volume, dat een geplande back-uptaak gekoppeld aan een back-beleid is. Een taak apparaat wordt gemaakt als gevolg van een herstel (DR) of een bewerking voor terugzetten.

    - **Apparaat** – de naam van het apparaat op de taak is gestart.

    - **Gestart op** – de tijd waarop de taak is gestart.

    - **Voortgang** – het voltooiingspercentage van een lopend project. Voor een voltooide taak moet dit altijd 100%.

De lijst met taken wordt elke 30 seconden vernieuwd.

U kunt de volgende projectgerelateerde acties uitvoeren op deze pagina:

- Details van de taak weergeven

- Een taak annuleren

## <a name="view-job-details"></a>Details van de taak weergeven

De volgende stappen uit om de details van elke taak uitvoeren.

#### <a name="to-view-job-details"></a>Naar de taakdetails van een weergeven

1. Klik op de pagina **taken** weergeven de taak of taken die u bent geïnteresseerd door een query met de gewenste filters uit te voeren. U kunt zoeken naar voltooid, actief is of taken geannuleerd.

2. Selecteer een taak.

3. Onderaan op de pagina, klikt u op **Details**.

4. Klik in het dialoogvenster **Details van back-up** kunt u de status, details, statistieken en gegevens statistieken weergeven.
 
    ![De detailpagina van taak](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a>Een taak annuleren

De volgende stappen om te annuleren van een actieve taak uitvoeren.

>[AZURE.NOTE] Sommige taken, zoals het wijzigen van een volume als u wilt wijzigen van het volumetype of uitvouwen van een volume, kunnen niet worden geannuleerd.

### <a name="to-cancel-a-job"></a>Om te annuleren van een taak

1. Klik op de pagina **taken** weergeven de actieve taak of taken die u Annuleren wilt door een query met de gewenste filters uit te voeren.

1. Selecteer de taak.

1. Onderaan op de pagina, klikt u op **Annuleren**.

1. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja**.

Deze taak wordt nu geannuleerd.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [beheren van de back-beleid van uw StorSimple](storsimple-manage-backup-policies.md).

- Informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
