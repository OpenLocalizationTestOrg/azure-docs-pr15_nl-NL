<properties 
   pageTitle="Updates installeren op een virtuele matrix StorSimple | Microsoft Azure"
   description="Wordt beschreven hoe u het web StorSimple virtuele matrix UI updates met behulp van de portal en hotfix methode toepassen"
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
   ms.date="09/07/2016"
   ms.author="alkohli" />

# <a name="install-updates-on-your-storsimple-virtual-array"></a>Updates installeren op uw virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

In dit artikel worden de benodigde stappen voor het installeren van updates op uw virtuele matrix StorSimple via het lokale web UI en via de portal van Azure klassieke. U wilt toepassen software-updates of hotfixes voor dat uw StorSimple virtuele matrix bijgewerkt blijft. 

Houd er rekening mee dat een update of hotfix installatie opnieuw is opgestart uw apparaat. Gezien het feit dat de StorSimple virtuele matrix een apparaat één knooppunt is, een I/O bezig is onderbroken en uw apparaat ervaringen downtime. 

Voordat u een update toepast, wordt aangeraden dat u voert u de volumes of waarden voor aandelen offline op de host eerste en klik vervolgens op het apparaat. Hiermee wordt een mogelijkheid van beschadiging geminimaliseerd.

> [AZURE.IMPORTANT] Als u Update 0,1 of GA softwareversies uitvoert, moet u de methode hotfix via het lokale web UI update 0,3 te installeren. Als u Update 0,2 uitvoert, wordt u aangeraden dat u de updates via de portal van Azure klassieke installeert.

## <a name="use-the-local-web-ui"></a>Het lokale web UI gebruiken 
 
Er zijn twee stappen bij gebruik van de lokale web UI:

- Download de update of de hotfix
- Installeer de update of de hotfix

### <a name="download-the-update-or-the-hotfix"></a>Download de update of de hotfix

Voer de volgende stappen uit om te downloaden van de software-update van de Microsoft Update-catalogus.

#### <a name="to-download-the-update-or-the-hotfix"></a>Download de update of de hotfix

1. Start Internet Explorer en Ga naar [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Als dit de eerste keer dat u de Microsoft Update-catalogus op deze computer, klikt u op **installeren** wanneer u wordt gevraagd om te installeren van de invoegtoepassing Microsoft Update-catalogus.
  
3. Voer in het zoekvak van de Microsoft Update-catalogus, het Knowledge Base (KB)-nummer van de hotfix die u wilt downloaden. Voer **3182061** voor Update 0,3 en klikt u op **Zoeken**.

    De hotfix-vermelding wordt weergegeven, bijvoorbeeld **StorSimple virtuele matrix Update 0,3**.

    ![Zoekcatalogus](./media/storsimple-ova-install-update-01/download1.png)

4. Klik op **toevoegen**. De update wordt toegevoegd aan de Favorieten.

5. Klik op **mandje weergeven**.

6. Klik op **downloaden**. Opgeven of **Blader** naar een lokale locatie waar u de downloads die moet worden weergegeven. De updates worden gedownload naar de opgegeven locatie en in een submap met dezelfde naam als de update geplaatst. De map kan ook worden gekopieerd naar een netwerkshare dat bereikbaar is via het apparaat.

7. Open de gekopieerde map, ziet u een zelfstandig pakket met Microsoft Update-bestand `WindowsTH-KB3011067-x64`. In dit bestand wordt gebruikt voor het installeren van de update of hotfix.


### <a name="install-the-update-or-the-hotfix"></a>Installeer de update of de hotfix

Controleer of dat u de update hebt of de hotfix gedownload lokaal op uw host of toegankelijk via een netwerkshare voordat u de installatie update of hotfix. 

Gebruik deze methode updates installeren op een apparaat met GA of bijwerken 0.1 softwareversies. Deze procedure kunt minder dan twee minuten om te voltooien. De volgende stappen om te kunnen installeren van de update of hotfix uitvoeren.


#### <a name="to-install-the-update-or-the-hotfix"></a>De update of de hotfix te installeren

1. In het lokale web UI, gaat u naar **onderhoud** > **Software-Update**.

    ![apparaat bijwerken](./media/storsimple-ova-install-update-01/update1m.png)

2. Voer in het **bestandspad bijwerken**, de bestandsnaam voor het bijwerken of de hotfix. U kunt ook Blader naar het installatiebestand update of hotfix als op een netwerkshare geplaatst. Klik op **toepassen**.

    ![apparaat bijwerken](./media/storsimple-ova-install-update-01/update2m.png)

3.  Een waarschuwing weergegeven. Opgegeven dit is u een apparaat met één knooppunt nadat de update wordt toegepast, het apparaat opnieuw is opgestart en er downtime. Klik op het pictogram van het selectievakje.

    ![apparaat bijwerken](./media/storsimple-ova-install-update-01/update3m.png)

4. Hiermee start u de update. Nadat het apparaat is bijgewerkt, opnieuw wordt gestart. De lokale UI is niet toegankelijk in deze duur.

    ![apparaat bijwerken](./media/storsimple-ova-install-update-01/update5m.png)

5. Nadat u het opnieuw opstarten is voltooid, worden geklikt gaat u naar de pagina **aanmelden** . Om te bevestigen dat de apparaatsoftware heeft bijgewerkt, klikt u in het lokale web UI, gaat u naar **onderhoud** > **Software-Update**. Versie van de weergegeven software moet **10.0.0.0.0.10288.0** voor Update 0,3.

    > [AZURE.NOTE] We rapporteren de versies van software in iets anders in het lokale web UI en de Azure klassieke portal. Bijvoorbeeld het lokale web UI **10.0.0.0.0.10288** en de Azure klassieke portal verslagen **10.0.10288.0** voor dezelfde versie. 

    ![apparaat bijwerken](./media/storsimple-ova-install-update-01/update6m.png)





## <a name="use-the-azure-classic-portal"></a>Gebruik van de Azure klassieke portal

Als u Update 0,2 uitvoert, wordt u aangeraden dat u updates via de portal van Azure klassieke installeert. De portal procedure moet de gebruiker te scannen, download en installeer de updates. Deze procedure duurt ongeveer 7 minuten om te voltooien. De volgende stappen om te kunnen installeren van de update of hotfix uitvoeren.

[AZURE.INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

Nadat de installatie is voltooid (zoals aangegeven met taakstatus op 100%), gaat u naar **apparaten > onderhoud > Software-Updates**. Het versienummer weergegeven software moet 10.0.10288.0 zijn.

![apparaat bijwerken](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren van uw StorSimple virtuele matrix](storsimple-ova-web-ui-admin.md).
