<properties 
   pageTitle="De StorSimple Manager-service voor StorSimple virtuele matrix implementeren | Microsoft Azure"
   description="Wordt uitgelegd hoe u maken en verwijderen van de StorSimple Manager-service in de portal van Azure klassieke en wordt uitgelegd hoe u voor het beheren van de service registratie-toets."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/19/2016"
   ms.author="alkohli" />

# <a name="deploy-the-storsimple-manager-service-for-storsimple-virtual-array"></a>De StorSimple Manager-service voor StorSimple virtuele matrix implementeren

## <a name="overview"></a>Overzicht

De StorSimple Manager-service wordt uitgevoerd in Microsoft Azure en verbinding maakt met meerdere StorSimple-apparaten. Nadat u de service hebt gemaakt, kunt u deze naar de apparaten beheren vanuit het Microsoft Azure klassieke portal wordt uitgevoerd in een browser. Hiermee kunt u de apparaten die met de StorSimple Manager-service vanaf een centrale locatie verbonden zijn, daarmee ook minimaliseren administratieve last controleren.

De pagina van de aantekening StorSimple Manager vermeldt alle StorSimple Manager-services die u gebruiken kunt voor het beheren van uw StorSimple opslag-apparaten. De volgende informatie is bedoeld voor elke service StorSimple Manager, op de pagina beheer StorSimple:

- **Naam** : de naam die is toegewezen aan uw StorSimple Manager-service wanneer deze is gemaakt. De servicenaam van de worden niet gewijzigd nadat de service is gemaakt.

- **Status** – de status van de service, die **actieve**, **maken**of **Online worden kan**.

- **Locatie** – de geografische locatie waarin het apparaat StorSimple wordt geïmplementeerd.

- **Abonnement** – de facturering abonnement dat is gekoppeld aan uw service.

Algemene taken die kunnen worden uitgevoerd via de pagina StorSimple Manager zijn:

- Een service maken
- Een service verwijderen
- De sleutel voor de registratie ophalen
- De sleutel voor de registratie genereren

Deze zelfstudie wordt beschreven hoe u deze taken uitvoert. De informatie in dit artikel is alleen van toepassing op StorSimple virtuele matrices. Ga naar het [implementeren van een service StorSimple Manager](storsimple-manage-service.md)voor meer informatie over StorSimple 8000 reeks.

## <a name="create-a-service"></a>Een service maken

Gebruik de optie **Snelle maken** om te maken van een service StorSimple Manager als u wilt implementeren van uw apparaat StorSimple. Als u wilt maken van een service, moet u beschikken over:

- Een abonnement met een Enterprise Agreement
- Een actieve Microsoft Azure opslag-account
- De informatie die wordt gebruikt om toegang te beheren

U kunt er ook voor kiezen om te genereren van een standaardaccount voor de opslag bij het maken van de service.

Een service voor eenmalige, meerdere apparaten kunt beheren. Een apparaat kan niet meerdere services echter's beslaan. Een grote ondernemingen kan meerdere exemplaren van de service voor gebruik met de verschillende abonnementen, organisaties of zelfs implementatie locaties bevatten.  

> [AZURE.NOTE] Moet u afzonderlijke exemplaren van StorSimple Manager-service voor het beheren van StorSimple 8000 reeks apparaten en StorSimple virtuele matrices.

Voer de volgende stappen uit als u wilt maken van een service.

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

## <a name="delete-a-service"></a>Een service verwijderen

Controleer voordat u een service verwijdert, of dat er geen verbonden apparaten gebruikt. Als de service gebruikt wordt, schakelt u de verbonden apparaten. De bewerking deactiveren server de verbinding tussen het apparaat en de service, maar de apparaatgegevens van het in de cloud behouden. 

> [AZURE.IMPORTANT] Een service wordt verwijderd, de bewerking kan niet ongedaan worden gemaakt. 

Voer de volgende stappen uit als u wilt verwijderen van een service.

### <a name="to-delete-a-service"></a>Een service verwijderen

1. Selecteer op de pagina **StorSimple Manager-service** de service die u wilt verwijderen.

1. Klik op **verwijderen** onder aan de pagina.

1. Klik op **Ja** in de bevestingsmelding. Duurt een paar minuten voor de service moet worden verwijderd.

## <a name="get-the-service-registration-key"></a>De sleutel voor de registratie ophalen

Nadat u een service hebt gemaakt, moet u uw apparaat StorSimple met de service registreren. Als u wilt uw eerste StorSimple apparaat hebt geregistreerd, moet u de service registratie-toets. Als u wilt extra apparaten met een bestaande StorSimple-service registreren, moet u zowel de registratie-toets en de service gegevens versleutelingssleutel (dat is gegenereerd op het eerste apparaat tijdens de registratie). Zie [de sleutel service data versleuteling van de lokale web UI ophalen](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)voor meer informatie over de service gegevenssleutel. 

De volgende stappen om de sleutel voor de registratie uitvoeren.

[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

Houd de sleutel voor de registratie op een veilige locatie. Moet u deze toets, evenals de sleutel service data versleuteling, extra apparaten registreren bij deze service. Nadat de sleutel voor de registratie, moet u voor het configureren van uw apparaat via de Windows PowerShell gebruiken voor StorSimple interface.

## <a name="regenerate-the-service-registration-key"></a>De sleutel voor de registratie genereren

U moet een service registratiegegevens sleutel genereren als u verplicht voor het uitvoeren van belangrijke draaiing of als de lijst met servicebeheerders is gewijzigd. Wanneer u de sleutel opnieuw genereren, wordt de nieuwe sleutel alleen voor het registreren van de volgende apparaten gebruikt. De apparaten die al zijn geregistreerd wordt niet beïnvloed door dit proces.

De volgende stappen om te genereren van een service registratiegegevens sleutel uitvoeren.

### <a name="to-regenerate-the-service-registration-key"></a>Te genereren van de service registratie-toets

1. Klik op de pagina **StorSimple Manager-service** op **Registratiegegevens sleutel**.

1. Klik in het dialoogvenster **Service registratiegegevens sleutel** op **genereren**.

1. Hier ziet u een bevestigingsbericht weergegeven. Klik op **OK** om door te gaan met het opnieuw genereren.

1. Een nieuwe service registratie sleutel wordt weergegeven.

1. Kopieer deze toets en sla deze voor het registreren van nieuwe apparaten met deze service.

1. Klik op het pictogram controleren ![Pictogram controleren](./media/storsimple-ova-manage-service/image7.png) Dit dialoogvenster sluiten.


## <a name="next-steps"></a>Volgende stappen

- Leer hoe u [aan de slag](storsimple-ova-deploy1-portal-prep.md) met een virtuele StorSimple-matrix.
    
- Informatie over het [beheren van uw apparaat StorSimple](storsimple-ova-web-ui-admin.md).

 
