<properties 
   pageTitle="Uw StorSimple volume containers beheren | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe u de pagina StorSimple Manager service volume containers toevoegen, wijzigen of verwijderen van een container volume kunt gebruiken."
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
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-storsimple-volume-containers"></a>Gebruik van de StorSimple Manager-service voor het beheren van StorSimple volume containers

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe u het gebruik van de service StorSimple Manager maken en beheren van StorSimple volume containers.

Een container volume in een Microsoft Azure StorSimple apparaat bevat een of meer volumes die opslag-account, codering en bandbreedte verbruik-instellingen delen. Een apparaat kan meerdere volume containers voor alle bijbehorende volumes hebben. 

Een container volume heeft de volgende kenmerken:

- **Volumes** – de trapsgewijze of lokaal vastgemaakte StorSimple hoeveelheden die deel uitmaken van de container volume. Een container volume kan maximaal 256 StorSimple volumes bevatten.

- **Versleuteling** – een versleutelingssleutel die kan worden gedefinieerd voor elke volume-container. Deze toets wordt gebruikt voor het coderen van de gegevens die is verzonden vanuit uw apparaat StorSimple in de cloud. Een militaire-grade AES-256 bitssleutel wordt gebruikt met de gebruiker ingevoerde-toets. Uw gegevens wilt beveiligen, is het raadzaam dat u altijd cloud opslag versleuteling inschakelen.

- **Opslag account** – het opslag-account dat is gekoppeld aan uw provider van de cloud-opslag. Alle de hoeveelheden die zich in een container volume delen dit account opslag. U kunt een opslag kiezen uit een bestaande lijst of een nieuw account maken wanneer u de container volume maken en geef de access-referenties voor dat account.

- **Cloud bandbreedte** – de bandbreedte die dat door het apparaat wanneer de gegevens van het apparaat wordt verzonden naar de cloud. U kunt de besturing van een bandbreedte afdwingen door op te geven van een waarde tussen 1 en 1000 Mbps wanneer u deze container definieert. Als u het apparaat naar alle beschikbare bandbreedte wilt, kunt u dit veld instelt op onbeperkt. U kunt ook maken en toepassen van een sjabloon bandbreedte om toe te wijzen bandbreedte op basis van de planning.

![Volume containers pagina](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

Deze volgende procedures wordt uitgelegd hoe u de pagina StorSimple **Volume containers** gebruiken om de volgende algemene bewerkingen te voltooien:

- Een container volume toevoegen 
- Een container volume wijzigen 
- Een container volume verwijderen 

## <a name="add-a-volume-container"></a>Een container volume toevoegen

De volgende stappen als u wilt toevoegen van een container volume uitvoeren.

[AZURE.INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]


## <a name="modify-a-volume-container"></a>Een container volume wijzigen

De volgende stappen als u wilt wijzigen van een container volume uitvoeren.

[AZURE.INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]


## <a name="delete-a-volume-container"></a>Een container volume verwijderen

Een container volume heeft volumes erin. Het kan worden verwijderd alleen als de hoeveelheden die zijn opgenomen in deze eerst worden verwijderd. De volgende stappen als u wilt verwijderen van een container volume uitvoeren.

[AZURE.INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [beheren StorSimple volumes](storsimple-manage-volumes.md). 
- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
