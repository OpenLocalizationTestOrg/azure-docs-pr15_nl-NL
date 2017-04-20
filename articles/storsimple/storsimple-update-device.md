<properties
   pageTitle="Bijwerken van uw apparaat StorSimple | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe u met de functie van de update StorSimple reguliere en onderhoud modus updates en hotfixes installeren."
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
   ms.date="06/28/2016"
   ms.author="v-sharos" />

# <a name="update-your-storsimple-8000-series-device"></a>Bijwerken van uw apparaat StorSimple 8000 reeks

## <a name="overview"></a>Overzicht

De functies van de updates StorSimple kunnen u eenvoudig uw apparaat StorSimple om up-to-date te houden. Afhankelijk van het updatetype, kunt u updates toepassen op het apparaat via de portal van Azure klassieke of via de Windows PowerShell-interface. Deze zelfstudie wordt beschreven welke van de update en hoe u elk van deze installeert.

U kunt twee soorten apparaatupdates toepassen: 

- Normale (of de normale modus) updates
- Updates voor onderhoud-modus

U kunt gewone updates via de portal van Azure klassieke of Windows PowerShell; installeren u moet echter Windows PowerShell gebruiken voor het onderhoud modus updates installeren. 

Elk updatetype wordt afzonderlijk hieronder beschreven.

### <a name="regular-updates"></a>Regelmatige updates

Regelmatige updates zijn ononderbroken updates die kunnen worden geïnstalleerd wanneer het apparaat in de normale modus is. Deze updates worden op elk apparaat controller via de website Microsoft Update toegepast. 

> [AZURE.IMPORTANT] Een failover controller kan optreden tijdens het updateproces. Echter dit is niet van invloed op beschikbaarheid van het systeem of -bewerking.

- Zie voor meer informatie over het installeren van regelmatig updates via de portal van Azure klassieke [regelmatig updates voor de installatie via het Azure klassieke portal(#install-regular-updates-via-the-azure-classic-portal).

- U kunt ook regelmatig updates via Windows PowerShell installeren voor StorSimple. Zie de [normale updates via Windows PowerShell voor StorSimple installeren](#install-regular-updates-via-windows-powershell-for-storsimple)voor meer informatie.

### <a name="maintenance-mode-updates"></a>Updates voor onderhoud-modus

Onderhoud modus updates zijn storend updates zoals upgrades van de schijf. Deze updates vragen om het apparaat in de modus onderhoud moet worden geplaatst. Zie voor meer informatie [stap 2: Voer onderhoud modus](#step2). U kunt de Azure klassieke portal niet gebruiken om het onderhoud modus updates installeren. In plaats daarvan, moet u Windows PowerShell gebruiken voor StorSimple. 

Voor meer informatie over het installeren van onderhoud modus updates [installeren onderhoud modus updates via Windows PowerShell voor StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple)te bekijken.

> [AZURE.IMPORTANT] Onderhoud modus updates moeten afzonderlijk worden toegepast op elke controller. 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Regelmatige updates via de portal van Azure klassieke installeren

U kunt de portal van Azure klassieke updates toepassen op uw apparaat StorSimple.

[AZURE.INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Regelmatige updates via Windows PowerShell installeren voor StorSimple

U kunt ook kunt u Windows PowerShell voor StorSimple reguliere (normale modus) updates toe te passen.

> [AZURE.IMPORTANT] Hoewel u regelmatig updates via Windows PowerShell voor StorSimple installeren kunt, wordt aangeraden normale updates via de portal van Azure klassieke te installeren. Beginnen met Update 1, moet oude worden gecontroleerd vóór de installatie van updates van de portal. Deze oude controles wordt voorrang fouten en via de telefoon soepeler zijn voor gebruik. 

[AZURE.INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Onderhoud modus updates via Windows PowerShell installeren voor StorSimple

Windows PowerShell voor StorSimple kunt u onderhoud modus updates toepassen op uw apparaat StorSimple. Alle i/o-aanvragen worden in deze modus onderbroken. Services zoals niet-vluchtige RAM geheugen (NVRAM) of de clusteringservice worden ook gestopt. Beide controllers zijn opnieuw gestart wanneer u voert u deze modus openen of sluiten. Wanneer u deze modus afsluit, worden alle services wordt hervat en moeten orde. (Dit kan een paar minuten duren.)

Als u onderhoud modus updates toepassen wilt, ontvangt u een waarschuwing via de portal van Azure klassieke dat er updates die moeten worden geïnstalleerd. Deze melding bevat instructies voor het gebruik van Windows PowerShell voor StorSimple de updates te installeren. Nadat u uw apparaat bijwerkt, moet u dezelfde procedure gebruiken om te wijzigen van het apparaat naar de normale modus. Zie voor stapsgewijze instructies [stap 4: afsluiten onderhoud modus](#step4).

> [AZURE.IMPORTANT] 
> 
> - Voordat u de modus voor onderhoud invoert, controleert u of beide apparaatcontrollers orde door te schakelen van de **Hardware Status** op de pagina **onderhoud** in de portal van Azure klassieke. Als de controller beschadigd is, neem dan contact op met Microsoft Support voor de volgende stappen. Ga naar contact opnemen met Microsoft ondersteuning voor meer informatie. 
> - Wanneer u in de modus voor onderhoud bent, moet u eerst de update toepassen op één controller en klikt u op de andere controller.

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>Stap 1: Verbinding maken met de seriële console<a name="step1">

Eerst een toepassing zoals stopverf gebruiken voor toegang tot de seriële console. De volgende procedure wordt uitgelegd hoe u met stopverf verbinding maken met de seriële console.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Stap 2: Onderhoud gezet<a name="step2">

Nadat u verbinding met de console maakt, kunt u bepalen of er updates installeren en onderhoud gezet om te kunnen installeren ze zijn.

[AZURE.INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Stap 3: Uw updates installeren<a name="step3">

Vervolgens uw updates installeren.

[AZURE.INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]
 
### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Stap 4: Afsluiten onderhoudsmodus<a name="step4">

Tot slot onderhoudsmodus afsluit.

[AZURE.INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>Hotfixes via Windows PowerShell installeren voor StorSimple

In tegenstelling tot-updates voor Microsoft Azure StorSimple zijn hotfixes geïnstalleerd van een gedeelde map. Er zijn twee soorten hotfixes zoals bij updates: 

- Normale hotfixes 
- Onderhoud modus hotfixes  

De volgende procedures wordt uitgelegd hoe u het Windows PowerShell voor StorSimple voor het installeren van reguliere en onderhoud modus hotfixes gebruiken.

[AZURE.INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[AZURE.INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Wat gebeurt er met updates als u een factory opnieuw instellen van het apparaat uitvoeren?

Als u een apparaat is standaardinstellingen gebruiken, klikt u vervolgens gaan alle updates verloren. Nadat het apparaat factory-Beginwaarden is geregistreerd en hebt geconfigureerd, moet u handmatig updates via de portal van Azure klassieke en/of Windows PowerShell installeren voor StorSimple. Voor meer informatie over factory opnieuw instellen, raadpleegt u [het apparaat standaardinstellingen opnieuw instellen](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van Windows PowerShell voor StorSimple voor het beheren van uw apparaat StorSimple](storsimple-windows-powershell-administration.md).
- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
