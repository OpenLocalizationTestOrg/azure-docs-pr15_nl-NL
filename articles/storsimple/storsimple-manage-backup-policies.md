<properties 
   pageTitle="Uw beleid voor het back-up van StorSimple beheren | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe u de service StorSimple Manager maken en beheren van handmatige back-ups, back-planningen en back-up bewaarbeleid kunt gebruiken."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/10/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-manage-backup-policies"></a>De service StorSimple Manager gebruiken om het back-beleid beheren

[AZURE.INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe u het gebruik van de pagina **Beleid voor het back-up** van StorSimple Manager service om te bepalen back-processen en back-up bewaarbeleid voor StorSimple begint. Hierin wordt ook het voltooien van een handmatige back-up.

De pagina **Beleid voor het back-up** kunt u back-beleid beheren en lokale plannen en cloud momentopnamen. (Back-beleid worden gebruikt voor het configureren van back-planningen en back-up bewaarbeleid voor een siteverzameling van hoeveelheden.) Beleid voor het back-up kunnen u een momentopname van meerdere hoeveelheden tegelijk. Dit betekent dat de back-ups gemaakt door een back-beleid vastlopen consistente exemplaren. Deze pagina bevat het back-beleid, hun typen, de bijbehorende hoeveelheden, het aantal back-ups behouden en de optie voor het inschakelen van dit beleid.

De pagina **Beleid voor het back-up** kunt u het bestaande back-beleid door een of meer van de volgende velden filteren:

- **Beleidsnaam** : de naam die is gekoppeld aan het beleid. De verschillende soorten beleidsregels zijn:

   - Geplande beleid die expliciet worden gemaakt door de gebruiker.
   - Automatische beleidsregels, die worden gemaakt wanneer de standaard back-up voor deze optie volume is ingeschakeld op het moment van volume maken. Dit beleid zijn benoemde als VolumeName_Default waarbij de naam van de Volume naar de naam van het volume van het StorSimple geconfigureerd door de gebruiker in de portal van Azure klassieke verwijst. De automatische beleid resulteert in de dagelijkse cloud momentopnamen beginnen bij 22:30 apparaat tijd.
   - Geïmporteerde beleid, in de StorSimple momentopname Manager oorspronkelijk zijn gemaakt. Deze hebben een tag die goed de StorSimple momentopname Manager-host die het beleid zijn geïmporteerd beschrijft uit.

- **Volumes** – de hoeveelheden die is gekoppeld aan het beleid. Alle de hoeveelheden die is gekoppeld aan een back-beleid worden gegroepeerd wanneer back-ups worden gemaakt.

- **Laatste geslaagde back-up** – de datum en tijd van de laatste geslaagde back-up die is gemaakt met dit beleid.

- **Volgende back-up** – de datum en tijd van de volgende geplande back-up wordt gestart door dit beleid.

- **Planningen** – het aantal planningen die is gekoppeld aan het back-beleid.

De veelgebruikte bewerkingen die u van deze pagina uitvoeren kunt zijn:

- Een back-beleid toevoegen 
- Toevoegen of wijzigen van een planning 
- Een back-beleid verwijderen 
- Een handmatige back-up maken 
- Een aangepast back-beleid maken met meerdere volumes en planningen 

## <a name="add-a-backup-policy"></a>Een back-beleid toevoegen

Een back-beleid automatisch het plannen van uw back-ups toevoegen. De volgende stappen uitvoeren in de portal van Azure klassieke back-beleid voor uw apparaat StorSimple toe te voegen. Nadat u het beleid hebt toegevoegd, kunt u een planning definiëren (Zie [toevoegen of wijzigen van een planning](#add-or-modify-a-schedule)).

[AZURE.INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]

![Video beschikbaar](./media/storsimple-manage-backup-policies/Video_icon.png) **Video beschikbaar**

Klik op een video waarin wordt aangegeven hoe een lokale maken of back-beleid cloud bekijken [hier](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).


## <a name="add-or-modify-a-schedule"></a>Toevoegen of wijzigen van een planning

U kunt toevoegen of wijzigen van een planning die is gekoppeld aan een bestaand back-beleid op uw apparaat StorSimple. De volgende stappen uitvoeren in de portal van Azure klassieke toevoegen of wijzigen van een planning.

[AZURE.INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## <a name="delete-a-backup-policy"></a>Een back-beleid verwijderen

De volgende stappen uitvoeren in de portal van Azure klassieke een back-beleid op uw apparaat StorSimple verwijderen.

[AZURE.INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]


## <a name="take-a-manual-backup"></a>Een handmatige back-up maken

De volgende stappen uitvoeren in de portal van Azure klassieke een bellen op back-up (handmatige) voor één volume maken.

[AZURE.INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Een aangepast back-beleid maken met meerdere volumes en planningen

De volgende stappen uitvoeren in de portal van Azure klassieke de back-beleid van een aangepaste met meerdere volumes en schema's maken.

[AZURE.INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
