<properties 
   pageTitle="Weergeven en beheren van StorSimple virtuele matrix taken | Microsoft Azure"
   description="Beschrijving van de pagina StorSimple Manager service taken en hoe gebruiken voor het bijhouden van recente en huidige taken voor de virtuele StorSimple-matrix."
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
   ms.workload="na"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a>De service StorSimple Manager gebruiken om het taken weergeven voor de virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

De pagina **taken** biedt één centrale portal voor weergeven en beheren van taken die zijn gestart op virtuele matrices (ook wel bekend als on-premises implementatie virtuele apparaten) die zijn verbonden met uw StorSimple Manager-service. U kunt actieve, voltooide en mislukte taken voor meerdere virtuele apparaten weergeven. Resultaten worden weergegeven in een tabelvormige indeling. 

![Pagina taken](./media/storsimple-ova-manage-jobs/ovajobs1.png)

U vindt snel de taken die u bent geïnteresseerd door te filteren op velden, zoals:

- **Status** : U kunt zoeken naar alle, actieve, mislukte of voltooide taken.
- **Van en tot** – taken kunnen worden gefilterd op basis van het bereik van de datum en tijd.
- **Type** : het taaktype kan worden all, back-up en herstellen, failover, downloadt u de bijgewerkte of updates installeren.
- **Apparaten** – taken zijn gestart op een specifieke apparaat dat is verbonden met uw service. De gefilterde taken worden vervolgens de tabelindeling op basis van de volgende kenmerken:

    - **Type** : het taaktype kan worden all, back-up en herstellen, failover, downloadt u de bijgewerkte of updates installeren.

    - **Status** : taken kan worden alle, uitgevoerd, voltooid of is mislukt.

    - **Entiteit** – de taken kunnen worden gekoppeld aan een volume, delen of apparaat. 

    - **Apparaat** – de naam van het apparaat op de taak is gestart.

    - **Gestart op** – de tijd waarop de taak is gestart.

    - **Voortgang** – het voltooiingspercentage van een lopend project. Voor een voltooide taak moet dit altijd 100%.

De lijst met taken wordt elke 30 seconden vernieuwd.

## <a name="view-job-details"></a>Details van de taak weergeven

De volgende stappen uit om de details van elke taak uitvoeren.

#### <a name="to-view-job-details"></a>Naar de taakdetails van een weergeven

1. Klik op de pagina **taken** weergeven de taak of taken die u bent geïnteresseerd door een query met de gewenste filters uit te voeren. U kunt zoeken naar voltooide of actieve taken.

2. Selecteer een taak in de lijst in tabelvorm van taken.

3. Onderaan op de pagina, klikt u op **Details**.

4. In het dialoogvenster **Details** , kunt u de status, details en statistieken weergeven. De volgende afbeelding ziet u een voorbeeld van het dialoogvenster **Details van back-up** .
 
    ![De detailpagina van taak](./media/storsimple-ova-manage-jobs/ovajobs2.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a>Taak fouten bij de virtuele machine in de hypervisor is onderbroken

Wanneer een taak voortgang van uw virtuele StorSimple-matrix en het apparaat (deze is ingericht in hypervisor VM) is onderbroken voor groter dan 15 minuten, de taak, mislukt. Dit wordt vanwege uw tijd StorSimple virtuele matrix niet gesynchroniseerd met de Microsoft Azure-tijd. Een voorbeeld van een taak terugzetten is mislukt wordt weergegeven in de volgende schermafbeelding.

![Taak mislukt herstellen](./media/storsimple-ova-manage-jobs/restorejobfailure.png)

Deze fouten wordt toegepast op back-up, terugzetten, bijwerken en failover-taken. Als uw VM is ingericht in Hyper-V, wordt de machine tijd uiteindelijk synchroniseren met uw hypervisor. Wanneer dat gebeurt, kunt u uw taak opnieuw starten. 

## <a name="next-steps"></a>Volgende stappen

[Informatie over het gebruik van de lokale web gebruikersinterface voor het beheren van uw StorSimple virtuele matrix](storsimple-ova-web-ui-admin.md).
