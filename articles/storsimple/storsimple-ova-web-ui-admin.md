<properties 
   pageTitle="Virtuele matrix StorSimple web beheer van de gebruikersinterface | Microsoft Azure"
   description="Beschreven hoe u eenvoudige apparaat beheerderstaken via het web StorSimple virtuele matrix UI."
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
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>Gebruik van de Web-gebruikersinterface voor het beheren van uw virtuele StorSimple-matrix

![Processtroom](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Overzicht

De zelfstudies in dit artikel zijn van toepassing op de Microsoft Azure StorSimple virtuele matrix (ook wel bekend als het StorSimple on-premises implementatie virtuele apparaat) maart 2016 beschikbaarheid (GA) van de algemene release uitgevoerd. In dit artikel worden enkele van de complexe werkstromen en taken voor vergaderingbeheer die kunnen worden uitgevoerd op de virtuele StorSimple-matrix. U kunt de StorSimple virtuele matrix met de Manager StorSimple beheren service-gebruikersinterface (genoemd de portal UI) en het lokale web gebruikersinterface voor het apparaat. In dit artikel ligt de nadruk op taken die u kunt uitvoeren via het web UI.

In dit artikel bevat de volgende zelfstudies:

- De sleutel service gegevens ophalen
- Fouten corrigeren web UI-instelling
- Een pakket log genereren
- Afgesloten of start het apparaat opnieuw

## <a name="get-the-service-data-encryption-key"></a>De sleutel service gegevens ophalen

Een service gegevens versleutelingssleutel wordt gegenereerd wanneer u uw eerste apparaat met de service StorSimple Manager registreren. Deze toets wordt vervolgens vereist met de service registratie-toets om extra apparaten met de service StorSimple Manager registreren.

Als u uw service gegevens versleutelingssleutel en nodig om op te halen bent kwijtgeraakt hebt, voert u de volgende stappen in de lokale web UI van het apparaat dat is geregistreerd bij uw service.

#### <a name="to-get-the-service-data-encryption-key"></a>Om de sleutel service data-versleuteling

1. Verbinding maken met het lokale web UI. Ga naar de **configuratie** > **Cloud-instellingen**.
  

2. Klik onder aan de pagina, op **versleutelingssleutel gegevens ophalen**. Een toets wordt weergegeven. Kopieer en sla deze toets.
    
    ![service gegevens versleutelingssleutel 1 ophalen](./media/storsimple-ova-web-ui-admin/image27.png)
   


## <a name="troubleshoot-web-ui-setup-errors"></a>Fouten corrigeren web UI-instelling

In sommige gevallen wanneer u het apparaat via het lokale web UI, configureren kunnen mogelijk optreden fouten. Als u wilt opsporen en oplossen van dergelijke fouten, kunt u de tests diagnostische gegevens uitvoeren.

#### <a name="to-run-the-diagnostic-tests"></a>De diagnostische tests uitvoeren

1. In het lokale web UI, gaat u naar **Probleemoplossing** > **diagnostische tests**.

    ![Diagnostische gegevens 1 uitvoeren](./media/storsimple-ova-web-ui-admin/image29.png)

2. Klik onder aan de pagina, op **Diagnostische Tests uitvoeren**. Hiermee wordt getest om op te sporen mogelijke problemen hebt met uw netwerk, het apparaat, de webproxy, initiëren tijd of cloud-instellingen. U wordt geïnformeerd dat het apparaat tests wordt uitgevoerd.

3. Nadat de tests hebt voltooid, worden de resultaten worden weergegeven. Het volgende voorbeeld ziet u de resultaten van diagnostische tests. Houd er rekening mee dat de web-proxy-instellingen zijn niet geconfigureerd op dit apparaat, en daarom de web-proxy-test is niet is gestart. Alle andere proeven voor netwerkinstellingen, DNS-server en tijdinstellingen zijn aangebracht.

    ![Diagnostische gegevens van 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Een pakket log genereren

Een pakket log bestaat uit alle relevante logboeken die Microsoft Support helpen kunnen bij het oplossen van problemen apparaat. In deze release, kan een pakket log via het lokale web UI worden gegenereerd.

#### <a name="to-generate-the-log-package"></a>Het pakket log genereren

1. In het lokale web UI, gaat u naar **Probleemoplossing** > **systeemlogboeken**.

    ![log pakket 1 genereren](./media/storsimple-ova-web-ui-admin/image31.png)

2. Klik onder aan de pagina, op **log-pakket maken**. Een pakket met de systeemlogboeken wordt gemaakt. Dit zijn enkele minuten duren.

    ![log pakket 2 genereren](./media/storsimple-ova-web-ui-admin/image32.png)

    U krijgt nadat het pakket is gemaakt en de pagina wordt bijgewerkt om aan te geven van de tijd en datum waarop het pakket is gemaakt.

    ![log pakket 3 genereren](./media/storsimple-ova-web-ui-admin/image33.png)

3. Klik op **log downloaden**. Een ZIP-pakket worden gedownload op uw systeem.

    ![log pakket 4 genereren](./media/storsimple-ova-web-ui-admin/image34.png)

4. U kunt het pakket gedownloade log pak en de systeem-logboekbestanden weergeven.

## <a name="shut-down-and-restart-your-device"></a>Afsluiten en start het apparaat opnieuw

U kunt afgesloten of uw virtuele apparaat via het lokale web UI opnieuw starten. We aanbevolen dat voordat u opnieuw opstart, voert u de volumes of waarden voor aandelen offline op de host en klik vervolgens op het apparaat. Hiermee wordt een mogelijkheid van beschadiging Minimaliseren. 

#### <a name="to-shut-down-your-virtual-device"></a>Het virtuele apparaat afsluiten

1. In het lokale web UI, gaat u naar **onderhoud** > **Power-instellingen**.

2. Klik onderaan op de pagina, op **Afsluiten**.

    ![apparaat afsluiten 1](./media/storsimple-ova-web-ui-admin/image36.png)

3. Een waarschuwing verschijnt met de mededeling dat een afsluiting van het apparaat eventuele IO die zijn uitgevoerd, waardoor een downtime wordt onderbroken. Klik op het pictogram controleren ![pictogram controleren](./media/storsimple-ova-web-ui-admin/image3.png).

    ![Waarschuwing van apparaat afsluiten](./media/storsimple-ova-web-ui-admin/image37.png)

    U wordt geïnformeerd dat de afsluiting is gestart.

    ![apparaat afsluiten gestart](./media/storsimple-ova-web-ui-admin/image38.png)

    Het apparaat wordt nu afgesloten. Als u starten van uw apparaat wilt, moet u dat doen door de Manager Hyper-V.

#### <a name="to-restart-your-virtual-device"></a>Om uw virtuele apparaat opnieuw te starten

1. In het lokale web UI, gaat u naar **onderhoud** > **Power-instellingen**.

2. Klik onder aan de pagina, op **opnieuw starten**.

    ![apparaat opnieuw starten](./media/storsimple-ova-web-ui-admin/image36.png)

3. Een waarschuwing verschijnt met de mededeling dat het opnieuw starten van het apparaat eventuele IOs die zijn uitgevoerd, waardoor een downtime wordt onderbroken. Klik op het pictogram controleren ![pictogram controleren](./media/storsimple-ova-web-ui-admin/image3.png).

    ![Start opnieuw waarschuwing](./media/storsimple-ova-web-ui-admin/image37.png)

    U wordt geïnformeerd dat het opnieuw opstarten is gestart.

    ![opnieuw starten gestart](./media/storsimple-ova-web-ui-admin/image39.png)

    Terwijl de opnieuw moet uitgevoerd worden, wordt de verbinding met de gebruikersinterface wordt verbroken. U kunt het opnieuw opstarten controleren door het vernieuwen van de gebruikersinterface regelmatig. U kunt ook de status van het apparaat opnieuw starten via de Manager Hyper-V controleren.

## <a name="next-steps"></a>Volgende stappen

Informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat](storsimple-manager-service-administration.md).
