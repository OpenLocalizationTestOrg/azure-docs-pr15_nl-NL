<properties 
   pageTitle="Access besturingselement records beheren voor de virtuele matrix StorSimple | Microsoft Azure"
   description="Beschrijving van het voor het beheren van access control-records (ACRs) om te bepalen welke hosts kunnen worden verbonden met een volume op de virtuele StorSimple-matrix."
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
   ms.date="05/03/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-access-control-records-for-the-storsimple-virtual-array"></a>Gebruik van de StorSimple Manager-service voor het beheren van access control-records voor de virtuele StorSimple-matrix 

## <a name="overview"></a>Overzicht

Access control-records (ACRs) kunnen u opgeven welke hosts kunnen worden verbonden met een volume op de StorSimple virtuele matrix (ook wel bekend als het StorSimple on-premises implementatie virtuele apparaat). ACRs zijn ingesteld op een specifiek volume en de iSCSI-gekwalificeerde namen (IQN's de gebruikershandleiding) van de hosts bevatten. Wanneer een host probeert te verbinden met een volume, het apparaat gecontroleerd of de ACR die is gekoppeld aan dat volume voor de naam IQN en als er een overeenkomst, klikt u vervolgens de verbinding tot stand is gebracht. Alle access besturingselement records met de bijbehorende IQN's de gebruikershandleiding hosts worden weergegeven in de sectie **toegang besturingselement records** op de pagina **configureren** .

Deze zelfstudie wordt de volgende algemene taken die betrekking hebben op ACR beschreven:

- De IQN ophalen
- Een access-besturingselement-record toevoegen 
- Een access-besturingselement record bewerken 
- Een access-besturingselement record verwijderen 

> [AZURE.IMPORTANT] 
> 
> - Wanneer u een ACR toewijst aan een volume, zorgen dat het volume wordt niet gelijktijdig geraadpleegd door meer dan één niet-gegroepeerde host omdat dit kan het volume beschadigde. 
> - Wanneer u een ACR verwijdert uit een volume, zorg dat de bijbehorende host is geen toegang heeft tot het volume omdat de verwijdering in een alleen-lezen-onderbreking resulteren kan.

## <a name="get-the-iqn"></a>De IQN ophalen

De volgende stappen om de IQN van een Windows-host met Windows Server 2012 uitvoeren.

[AZURE.INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Een ACR toevoegen

De pagina **configuratie** StorSimple Manager kunt u ACRs toevoegen. U kunt één ACR meestal wilt koppelen aan één volume.

Voor informatie over het koppelen van een ACR met een volume, gaat u [een volume](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume)toevoegen.

>[AZURE.IMPORTANT] 
> 
>Wanneer u een ACR toewijst aan een volume, zorgen dat het volume wordt niet gelijktijdig geraadpleegd door meer dan één niet-gegroepeerde host omdat dit kan het volume beschadigde.
 
Voer de volgende stappen uit als u wilt toevoegen van een ACR.

#### <a name="to-add-an-acr"></a>Een ACR toevoegen

1. Klik op de pagina service aantekening Selecteer uw service, dubbelklik op de servicenaam van de en klik vervolgens op het tabblad **configuratie** .

    ![tabblad configuratie](./media/storsimple-ova-manage-acrs/acr1.png)

2. In de vermelding in tabelvorm onder **toegang besturingselement records**, door een **naam** voor uw ACR te geven.

3. Geef onder **iSCSI initiatornaam**, de naam van de IQN van uw Windows-host. 

4. Klik op **Opslaan** onder aan de pagina om de zojuist gemaakte ACR opslaan. Hier ziet u het volgende bevestigingsbericht weergegeven.

    ![bevestigingsbericht](./media/storsimple-ova-manage-acrs/acr2.png)

5. Klik op het pictogram controleren ![pictogram controleren](./media/storsimple-ova-manage-acrs/check-icon.png). De tabelvormige vermelding wordt worden bijgewerkt, zodat deze toevoeging.

## <a name="edit-an-acr"></a>Een ACR bewerken

U kunt de pagina **configuratie** in de portal van Azure klassieke ACRs bewerken. 

> [AZURE.NOTE] Alleen deze ACRs die momenteel niet gebruikt, moet u wijzigen. Als u wilt bewerken een ACR die is gekoppeld aan een volume dat is momenteel in gebruik, moet u het volume eerst offline halen.

De volgende stappen als u wilt bewerken, een ACR uitvoeren.

#### <a name="to-edit-an-acr"></a>Een ACR bewerken

1. Klik op de pagina service aantekening Selecteer uw service, dubbelklik op de servicenaam van de en klik vervolgens op het tabblad **configuratie** .

2. Plaats in de tabelvorm vermelding van de access-besturingselement-records, de muisaanwijzer op de ACR die u wilt wijzigen.

3. Een nieuwe naam en/of IQN voor de ACR opgeeft.

4. Klik op **Opslaan** onder aan de pagina om de gewijzigde ACR opslaan. Hier ziet u een bevestigingsbericht weergegeven. 

5. Klik op het pictogram controleren ![pictogram controleren](./media/storsimple-ova-manage-acrs/check-icon.png). De tabelvormige vermelding wordt bijgewerkt zodat deze wijziging.

## <a name="delete-an-access-control-record"></a>Een access-besturingselement record verwijderen

U kunt de pagina **configuratie** in de portal van Azure klassieke ACRs verwijderen. 

> [AZURE.NOTE] 
> 
> - Alleen deze ACRs die momenteel niet gebruikt, moet u verwijderen. Als u wilt verwijderen van een ACR die is gekoppeld aan een volume dat is momenteel in gebruik, moet u het volume eerst offline halen.
> - Wanneer u een ACR verwijdert uit een volume, zorg dat de bijbehorende host is geen toegang heeft tot het volume omdat de verwijdering in een alleen-lezen-onderbreking resulteren kan.

De volgende stappen als u wilt verwijderen van een access-besturingselement record uitvoeren.

#### <a name="to-delete-an-access-control-record"></a>Een access-besturingselement-record wilt verwijderen

1. Klik op de pagina service aantekening Selecteer uw service, dubbelklik op de servicenaam van de en klik vervolgens op het tabblad **configuratie** .

2. In de tabelvorm vermelding van de access-records van het besturingselement (ACRs), plaats u de muisaanwijzer boven de ACR die u wilt verwijderen.

3. Een verwijderpictogram (**x**) wordt weergegeven in de rechterkolom extreme voor de ACR die u selecteert. Klik op het pictogram **x** als u wilt verwijderen van de ACR. Hier ziet u het volgende bevestigingsbericht weergegeven.

    ![bevestigingsbericht](./media/storsimple-ova-manage-acrs/acr3.png)

5. Klik op het pictogram controleren ![pictogram controleren](./media/storsimple-ova-manage-acrs/check-icon.png). De tabelvormige vermelding wordt worden bijgewerkt met de verwijdering.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [toevoegen van volumes en ACRs configureren](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).
