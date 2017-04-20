<properties 
   pageTitle="Access besturingselement records beheren in StorSimple | Microsoft Azure"
   description="Beschreven hoe u toegang besturingselement records (ACRs) gebruiken om te bepalen welke hosts kunnen worden verbonden met een volume op het apparaat StorSimple."
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a>Gebruik van de StorSimple Manager-service voor het beheren van access besturingselement records

## <a name="overview"></a>Overzicht

Access control-records (ACRs) kunnen u opgeven welke hosts kunnen worden verbonden met een volume op het apparaat StorSimple. ACRs zijn ingesteld op een specifiek volume en de iSCSI-gekwalificeerde namen (IQN's de gebruikershandleiding) van de hosts bevatten. Wanneer een host probeert verbinding maken met een volume, wordt het apparaat controleert de ACR die is gekoppeld aan dat volume voor de naam IQN en als er een overeenkomst, klikt u vervolgens de verbinding tot stand is gebracht. Alle access besturingselement records met de bijbehorende IQN's de gebruikershandleiding hosts worden weergegeven in de sectie toegang besturingselement records op de pagina **configureren** .

Deze zelfstudie wordt de volgende algemene taken die betrekking hebben op ACR beschreven:

- Een access-besturingselement-record toevoegen 
- Een access-besturingselement record bewerken 
- Een access-besturingselement record verwijderen 

> [AZURE.IMPORTANT] 
> 
> - Wanneer u een ACR toewijst aan een volume, zorgen dat het volume wordt niet gelijktijdig geraadpleegd door meer dan één niet-gegroepeerde host omdat dit kan het volume beschadigde. 
> - Wanneer u een ACR verwijdert uit een volume, zorg dat de bijbehorende host is geen toegang heeft tot het volume omdat de verwijdering in een alleen-lezen-onderbreking resulteren kan.

## <a name="add-an-access-control-record"></a>Een access-besturingselement-record toevoegen

De pagina StorSimple Manager service **configureren** kunt u ACRs toevoegen. U kunt één ACR meestal wilt koppelen aan één volume.

Voer de volgende stappen uit als u wilt toevoegen van een ACR.

#### <a name="to-add-an-access-control-record"></a>Een access-besturingselement-record toe te voegen

1. Klik op de pagina service aantekening Selecteer uw service, dubbelklik op de servicenaam van de en klik vervolgens op het tabblad **configureren** .

2. In de vermelding in tabelvorm onder **toegang besturingselement records**, door een **naam** voor uw ACR te geven.

3. Geef de naam IQN van uw Windows-host onder **iSCSI initiatornaam**. Ga als volgt te werk als u de IQN van uw Windows Server-host:

   - Start het beginpunt van Microsoft iSCSI op uw Windows-host.
   - Klik in het venster **iSCSI Initiator eigenschappen** op het tabblad **configuratie** , selecteer en kopieer de tekenreeks uit het veld **Initiatornaam** .
   - Plak deze tekenreeks in het veld **iSCSI initiatornaam** op de tabel ACRs in de portal van Azure klassieke.

4. Klik op **Opslaan** om op te slaan de zojuist gemaakte ACR. De tabelvormige vermelding wordt worden bijgewerkt, zodat deze toevoeging.

## <a name="edit-an-access-control-record"></a>Een access-besturingselement record bewerken

U kunt de pagina **configureren** in de portal van Azure klassieke ACRs bewerken. 

> [AZURE.NOTE] Alleen deze ACRs die momenteel niet gebruikt, kunt u wijzigen. Als u wilt bewerken een ACR die is gekoppeld aan een volume dat is momenteel in gebruik, moet u het volume eerst offline halen.

De volgende stappen als u wilt bewerken, een ACR uitvoeren.

#### <a name="to-edit-an-access-control-record"></a>Een access-besturingselement record bewerken

1. Klik op de pagina service aantekening Selecteer uw service, dubbelklik op de servicenaam van de en klik vervolgens op het tabblad **configureren** .

2. Plaats in de tabelvorm vermelding van de access-besturingselement-records, de muisaanwijzer op de ACR die u wilt wijzigen.

3. Een nieuwe naam en/of IQN voor de ACR opgeeft.

4. Klik op **Opslaan** om op te slaan de gewijzigde ACR. De tabelvormige vermelding wordt bijgewerkt zodat deze wijziging.

## <a name="delete-an-access-control-record"></a>Een access-besturingselement record verwijderen

U kunt de pagina **configureren** in de portal van Azure klassieke ACRs verwijderen. 

> [AZURE.NOTE] Alleen deze ACRs die momenteel niet gebruikt, kunt u verwijderen. Als u wilt verwijderen van een ACR die is gekoppeld aan een volume dat is momenteel in gebruik, moet u het volume eerst offline halen.

De volgende stappen als u wilt verwijderen van een access-besturingselement record uitvoeren.

#### <a name="to-delete-an-access-control-record"></a>Een access-besturingselement-record wilt verwijderen

1. Klik op de pagina service aantekening Selecteer uw service, dubbelklik op de servicenaam van de en klik vervolgens op het tabblad **configureren** .

2. In de tabelvorm vermelding van de access-records van het besturingselement (ACRs), plaats u de muisaanwijzer boven de ACR die u wilt verwijderen.

3. Een verwijderpictogram (**x**) wordt weergegeven in de rechterkolom extreme voor de ACR die u selecteert. Klik op het pictogram **x** als u wilt verwijderen van de ACR.

4. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja** om door te gaan met de verwijdering. De tabelvormige vermelding wordt worden bijgewerkt met de verwijdering.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [beheren StorSimple volumes](storsimple-manage-volumes.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
 
