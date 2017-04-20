<properties 
   pageTitle="Virtuele matrix StorSimple back-zelfstudie | Microsoft Azure"
   description="Beschreven hoe u een back-up StorSimple virtuele matrix waarden voor aandelen en volumes."
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
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="back-up-your-storsimple-virtual-array"></a>Back-up van uw virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht 

Deze zelfstudie geldt voor de versie van Microsoft Azure StorSimple virtuele matrix (ook wel bekend als de StorSimple on-premises implementatie virtueel apparaat of een virtueel apparaat StorSimple) actieve maart 2016 algemene beschikbaarheid (GA) of hogere versies.

De StorSimple virtuele matrix is een hybride cloud on-premises implementatie virtuele apparaat voor gegevensopslag die kan worden geconfigureerd als een bestandsserver of een iSCSI-server. Dit kunt maken van back-ups, Technical Library en apparaat failover uitvoeren als herstel nodig is. Als geconfigureerd als een bestandsserver, kunt deze itemniveau herstel. Deze zelfstudie wordt beschreven hoe u van de Azure klassieke portal of het web StorSimple UI gepland en handmatig back-ups van uw StorSimple virtuele matrix maken.


## <a name="back-up-shares-and-volumes"></a>Een back-up waarden voor aandelen en volumes

Back-ups point-in-time beveiliging bieden, herstelmogelijkheden verbeteren en herstellen tijden voor waarden voor aandelen en volumes minimaliseren. U kunt een back-up een delen of het volume op uw apparaat StorSimple op twee manieren: **gepland** of **handmatig**. Elk van de methoden wordt in de volgende secties besproken.

> [AZURE.NOTE] In deze release, worden de geplande back-ups gemaakt door een standaardbeleid die wordt uitgevoerd dagelijks op een opgegeven tijd en back-ups van alle waarden voor aandelen of volumes op het apparaat. Het is niet mogelijk te maken van aangepaste beleidsregels voor geplande back-ups op dit moment.

## <a name="change-the-backup-schedule"></a>De back-planning wijzigen

Het virtuele StorSimple-apparaat heeft een back-up standaardbeleid dat begint op een opgegeven tijdstip (22:30) en back-ups van alle waarden voor aandelen of volumes op het apparaat één keer per dag. De tijd waarop de back-up is gestart, maar de frequentie en het bewaarbeleid (welke bevat het nummer van de back-ups moeten worden bewaard) kunnen niet worden gewijzigd, kunt u wijzigen. Tijdens deze back-ups, het gehele virtuele apparaat back-up; Daarom, is het raadzaam dat u deze back-ups voor rustige uren plannen.

De volgende stappen uitvoeren in de [portal van Azure klassieke](https://manage.windowsazure.com/) wijzigen van de standaardtijd voor back-up starten.

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>De starttijd van het standaardbeleid voor back-ups wijzigen

1. Ga naar het tabblad apparaat **configureren** .

2. Onder de sectie **back-** Geef de starttijd van de dagelijkse back-up.

3. Klik op **Opslaan**.

### <a name="take-a-manual-backup"></a>Een handmatige back-up maken

Naast geplande back-ups die u kunt uitvoeren (op aanvraag) van een handmatig back-up op elk gewenst moment.

#### <a name="to-create-a-manual-on-demand-backup"></a>Een handmatige (op aanvraag) back-up maken

1. Ga naar het tabblad **waarden voor aandelen** of het tabblad **Volumes** .

2. Klik onder aan de pagina, op **alle back-up**. U wordt gevraagd om te bevestigen dat u wilt de back-up nu uitvoeren. Klik op het pictogram van het selectievakje ![Controleer pictogram](./media/storsimple-ova-backup/image3.png) om verder te gaan met de back-up.

    ![back-bevestiging](./media/storsimple-ova-backup/image4.png)

    U wordt geïnformeerd dat er een back-uptaak wordt gestart.

    ![back-up starten](./media/storsimple-ova-backup/image5.png)

    U wordt geïnformeerd dat de taak is gemaakt.

    ![back-uptaak gemaakt](./media/storsimple-ova-backup/image7.png)

3. Als u wilt de voortgang van het project bijhouden, klikt u op **Taak weergeven**.

4. Nadat de back-uptaak is voltooid, gaat u naar het tabblad **back-up-catalogus** . Hier ziet u de voltooide back-up.

    ![Voltooide back-up maken](./media/storsimple-ova-backup/image8.png)

5. De filterselecties aan het gewenste apparaat, back-beleid en tijdsbereik instellen en klik op het pictogram controleren ![pictogram controleren](./media/storsimple-ova-backup/image3.png).

    De back-up moet worden weergegeven in de lijst met back-sets die wordt weergegeven in de catalogus.

## <a name="view-existing-backups"></a>Bestaande back-ups weergeven

De volgende stappen uitvoeren in de portal van Azure klassieke om weer te geven van de bestaande back-ups.

#### <a name="to-view-existing-backups"></a>Bestaande back-ups weergeven

1. Klik op het tabblad **back-up-catalogus** op de pagina van de service StorSimple Manager.

2. Selecteer een back-up instellen als volgt:

    1. Selecteer het apparaat.

    2. Kies de delen of het volume voor de back-up die u wilt selecteren in de vervolgkeuzelijst.

    3. Geef het tijdsbereik.

    4. Klik op het pictogram van het selectievakje ![](./media/storsimple-ova-backup/image3.png) deze query uit te voeren.

    De back-ups die is gekoppeld aan de geselecteerde optie voor delen of het volume moet worden weergegeven in de lijst met back-sets.

![video_icon](./media/storsimple-ova-backup/video_icon.png) **Video beschikbaar**

Bekijk de video om te zien hoe u kunt waarden voor aandelen maken, een back-up waarden voor aandelen en herstellen van gegevens op een virtuele StorSimple-matrix.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren van uw StorSimple virtuele matrix](storsimple-ova-web-ui-admin.md).
