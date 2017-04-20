<properties 
   pageTitle="Deactiveer en verwijder een StorSimple-apparaat | Microsoft Azure"
   description="Wordt beschreven hoe StorSimple apparaat uit service verwijderen door deze eerst te deactiveren en vervolgens te verwijderen."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="anoobbacker" />

# <a name="deactivate-and-delete-a-storsimple-device"></a>Deactiveer en verwijder een StorSimple-apparaat

## <a name="overview"></a>Overzicht

U wilt uitvoeren van een apparaat StorSimple afmelden bij service (bijvoorbeeld als u vervangen of een upgrade van uw apparaat of als u niet langer StorSimple gebruikt). Als dit het geval is, moet u voor het deactiveren van het apparaat voordat u deze kunt verwijderen. Het deactiveren van verbreekt de verbinding tussen het apparaat en de bijbehorende StorSimple Manager-service. Deze zelfstudie wordt uitgelegd hoe u een apparaat StorSimple uit service verwijderen door deze eerste te deactiveren en vervolgens te verwijderen. 

Wanneer u een apparaat hebt gedeactiveerd, zijn alle gegevens die lokaal is opgeslagen op het apparaat niet meer worden gebruikt. Alleen de gegevens die zijn gekoppeld aan het apparaat dat is opgeslagen in de cloud kunnen worden hersteld.  

>[AZURE.WARNING] Deactiveren niet ongedaan maken en kan niet ongedaan worden gemaakt. Een gedeactiveerd apparaat kan niet worden geregistreerd met de service StorSimple Manager, tenzij deze eerst opnieuw door de factory instellen. 
>
>De fabriek opnieuw proces Hiermee verwijdert u alle gegevens die lokaal op uw apparaat is opgeslagen. Daarom essentiële dat u een momentopname cloud van al uw gegevens voordat u een apparaat deactiveren. Hierdoor kunt u alle gegevens in een later stadium herstellen.

Deze zelfstudie wordt uitgelegd hoe u:

- Deactiveren van een apparaat en de gegevens verwijderen
- Deactiveren van een apparaat en de gegevens behouden

Ook wordt uitgelegd hoe deactiveren en verwijdering werkt op een virtueel StorSimple-apparaat.

>[AZURE.NOTE] Voordat u een StorSimple fysieke of virtuele apparaat hebt gedeactiveerd, moet u om te stoppen of clients en hosts die afhankelijk van het apparaat zijn verwijderen.

## <a name="deactivate-and-delete-data"></a>Deactiveer en verwijder gegevens

Als u geïnteresseerd bent in het apparaat volledig verwijderen en niet wilt bewaren van de gegevens op het apparaat, voert u de volgende stappen uit.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Het apparaat te deactiveren en verwijderen van de gegevens  

1. Voordat u een apparaat deactiveren, moet u alle het volume verwijderen containers (en de hoeveelheden) die zijn gekoppeld aan het apparaat. Alleen nadat u de bijbehorende back-ups hebt verwijderd, kunt u het volume van containers verwijderen.

2. Het apparaat als volgt uitschakelen:

    1. Selecteer het apparaat dat u wilt uitschakelen en klik op **deactiveren**onderaan op de pagina, op de pagina StorSimple Manager service **apparaten** .

    2. Er wordt een bevestigingsbericht weergegeven. Klik op **Ja** om door te gaan. Het proces deactiveren gestart en een paar minuten duren.

3. Nadat u hebt gedeactiveerd, kunt u het apparaat volledig verwijderen. Als u een apparaat verwijdert, wordt deze verwijderd uit de lijst met apparaten die zijn verbonden met de service. De service kunt niet langer dan de verwijderde apparaat beheren. Gebruik de volgende stappen uit om te verwijderen van het apparaat:

    1. Selecteer op de pagina StorSimple Manager service **apparaten** een gedeactiveerd apparaat dat u wilt verwijderen.

    2. Klik in de linkerbenedenhoek op de pagina, op **verwijderen**.

    3. U wordt gevraagd om bevestiging. Klik op **Ja** om door te gaan.

    Deze mogelijk een paar minuten duren voordat het apparaat zijn verwijderd.

## <a name="deactivate-and-retain-data"></a>Deactiveer en bewaren van gegevens

Als u bent geïnteresseerd bij het verwijderen van het apparaat, maar u wilt de gegevens behouden, voert u de volgende stappen uit.

####<a name="to-deactivate-a-device-and-retain-the-data"></a>Om te deactiveren van een apparaat en de gegevens behouden 

1. Het apparaat deactiveren. Alle volume-containers en de momentopnamen van het apparaat blijven.

    1. Selecteer het apparaat dat u wilt uitschakelen en klik op **deactiveren**onderaan op de pagina, op de pagina StorSimple Manager service **apparaten** .

    2. Er wordt een bevestigingsbericht weergegeven. Klik op **Ja** om door te gaan. Het proces deactiveren gestart en een paar minuten duren.

2. U kunt nu niet via de volume-containers en de bijbehorende momentopnamen. Voor procedures uit, gaat u naar [Failover en noodgevallen herstel voor uw apparaat StorSimple](storsimple-device-failover-disaster-recovery.md).

3. Na deactiveren en failover, kunt u het apparaat volledig verwijderen. Als u een apparaat verwijdert, wordt deze verwijderd uit de lijst met apparaten die zijn verbonden met de service. De service kunt niet langer dan de verwijderde apparaat beheren. Voer de volgende stappen uit als u wilt verwijderen van het apparaat:
 
    1. Selecteer op de pagina StorSimple Manager service **apparaten** een gedeactiveerd apparaat dat u wilt verwijderen.

    2. Klik in de linkerbenedenhoek op de pagina, op **verwijderen**.

    3. U wordt gevraagd om bevestiging. Klik op **Ja** om door te gaan.

    Deze mogelijk een paar minuten duren voordat het apparaat zijn verwijderd.

## <a name="deactivate-and-delete-a-virtual-device"></a>Deactiveer en verwijder een virtueel apparaat

Voor een virtueel apparaat StorSimple deallocates deactiveren de virtuele machine. U kunt de virtuele machine en de resources die zijn gemaakt wanneer deze is ingericht verwijderen. Nadat het virtuele apparaat is gedeactiveerd, kan deze niet worden hersteld naar de vorige status. 

Deactiveren resulteert in de volgende bewerkingen uit:

- Het virtuele StorSimple-apparaat wordt verwijderd.

- De OSDisk en gegevensschijven gemaakt voor het virtuele apparaat StorSimple worden verwijderd.

- De Service die worden gehost en virtuele netwerken die zijn gemaakt tijdens het inrichten worden bewaard. Als u deze entiteiten niet gebruikt, moet u deze handmatig verwijderen.

- Cloud momentopnamen gemaakt door het virtuele apparaat StorSimple worden bewaard.

## <a name="next-steps"></a>Volgende stappen
- Als u wilt terugzetten het gedeactiveerd apparaat op fabrieksinstellingen, gaat u naar [het apparaat standaardinstellingen opnieuw instellen](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

- Voor technische ondersteuning, [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md).

- Meer informatie over het gebruik van de StorSimple Manager-service, gaat u naar [de StorSimple Manager-service voor het beheren van uw apparaat StorSimple gebruiken](storsimple-manager-service-administration.md). 
