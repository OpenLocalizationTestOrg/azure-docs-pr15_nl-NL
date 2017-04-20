<properties
    pageTitle="Synchronisatiefouten oplossen wanneer u Azure opslag accounts, containers of VHD's in een implementatie resourcemanager verwijdert | Microsoft Azure"
    description="Synchronisatiefouten oplossen wanneer u Azure opslag accounts, containers of VHD's in een resourcemanager-implementatie verwijdert"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="genli"/>

# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds-in-a-resource-manager-deployment"></a>Synchronisatiefouten oplossen wanneer u Azure opslag accounts, containers of VHD's in een resourcemanager-implementatie verwijdert

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

U kunt fouten mogelijk ontvangt wanneer u probeert te verwijderen van een account van Azure opslag, container of virtuele harde schijf (VHD) in de [portal van Azure](https://portal.azure.com). In dit artikel bevat voor probleemoplossing richtlijnen om op te lossen van het probleem in een resourcemanager Azure-implementatie.

Als dit artikel niet adres van uw Azure probleem, bezoekt u de Azure-forums op [MSDN en stapel overloop](https://azure.microsoft.com/support/forums/). U kunt uw probleem posten op deze forums of @AzureSupport op Twitter. U kunt ook een verzoek om Azure ondersteuning indienen door te selecteren op de site [Azure ondersteunen](https://azure.microsoft.com/support/options/) **ondersteuning** .

## <a name="symptoms"></a>Symptomen

### <a name="scenario-1"></a>Scenario 1

Wanneer u probeert te verwijderen van een VHD in een account opslag in de implementatie van een Resource Manager, wordt het volgende foutbericht weergegeven:

**Blob 'vhds/BlobName.vhd' verwijderen is mislukt. Fout: Er is momenteel een lease op de blob en geen lease-ID is opgegeven in het verzoek.**

Dit probleem kan optreden omdat een virtuele machine (VM) heeft een lease op de VHD die u wilt verwijderen.

### <a name="scenario-2"></a>Scenario 2

Wanneer u probeert een container in een account opslag in de implementatie van een Resource Manager, wordt het volgende foutbericht weergegeven:

**Verwijderen van opslagruimte container 'VHD's ' is mislukt. Fout: Er is momenteel een lease op de container en geen lease-ID is opgegeven in het verzoek.**

Dit probleem kan optreden omdat de container heeft een VHD die in de stand lease is vergrendeld.

### <a name="scenario-3"></a>Scenario 3

Wanneer u probeert te verwijderen van een account opslag in de implementatie van een Resource Manager, wordt het volgende foutbericht weergegeven:

**Verwijderen van opslagruimte account 'StorageAccountName' is mislukt. Fout: Het opslag-account kan niet worden verwijderd vanwege de onderdelen die wordt gebruikt.**

Dit probleem kan optreden omdat het opslag-account bevat een VHD met de status van de lease.

## <a name="solution"></a>Oplossing

Als u wilt oplossen, moet u de VHD dat wordt veroorzaakt door de fout en de bijbehorende VM identificeren. Vervolgens loskoppelen van de VHD uit de VM (voor gegevensschijven) of verwijderen van de VM waarin de VHD (voor OS schijven) wordt gebruikt. Hiermee verwijdert u de lease vanaf de VHD en kan worden verwijderd.

### <a name="step-1-identify-the-problem-vhd-and-the-associated-vm"></a>Stap 1: Het probleem identificeren VHD en de bijbehorende VM


1. Meld u aan bij de [portal van Azure](https://portal.azure.com).
2. Selecteer in het menu **Hub voor** **alle resources**. Ga naar het opslag-account dat u wilt verwijderen en selecteer vervolgens **BLOB's** > **VHD's**.

    ![Schermafbeelding van de portal, waarbij het opslag-account en de container 'VHD's ' is gemarkeerd](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

3. Controleer de eigenschappen van elke VHD in de container. Zoek de VHD met de status **huurlijnen** . Klik, bepalen welke VM is via de VHD. Meestal kunt u bepalen welk VM bevat de VHD door te schakelen van de naam van de VHD:

    - OS schijven doorgaans volgt u deze naamgevingsconventie: VMNameYYYYMMDDHHMMSS.vhd
    - Gegevensschijven doorgaans volgt u deze naamgevingsconventie: VMName-JJJJMMDD-HHMMSS.vhd

    ![Schermafbeelding van de container info in de portal met de naam van de VM, de status lease van 'Vergrendeld' en de status lease van "huurlijnen' gemarkeerd](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/locatevm.png)

### <a name="step-2-remove-the-lease-from-the-vhd"></a>Stap 2: De lease verwijderen uit de VHD

Verwijderen van de VM waarin de VHD (voor OS schijven) wordt gebruikt:

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com).
2.  Selecteer in het menu **Hub** **virtuele Machines**.
3.  Selecteer de VM waarin een lease op de VHD.
4.  Zorg ervoor dat er niets actief de virtuele-computer gebruikt en dat u de virtuele machine niet meer nodig hebt.
5.  Aan de bovenkant van het blad **VM details** , selecteert u **verwijderen**en klik vervolgens op **Ja** om te bevestigen.
6.  De VM moet worden verwijderd, maar de VHD moet worden bewaard. De VHD moet echter niet langer een lease hebben op is geïnstalleerd. Het kan enkele minuten duren voordat de lease verschijnen duren. Om te bevestigen dat de lease is uitgebracht, gaat u naar **alle resources** > **Opslagaccountnaam** > **BLOB's** > **VHD's**. Klik in het deelvenster **Eigenschappen Blob** moet de waarde **Lease Status** **ontgrendeld**.

Loskoppelen van de VHD uit de VM die wordt gebruikt (voor gegevensschijven):

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com).
2.  Selecteer in het menu **Hub** **virtuele Machines**.
3.  Selecteer de VM waarin een lease op de VHD.
4.  Selecteer **schijven** op het blad **VM details** .
5.  Selecteer de gegevensschijf met een lease op de VHD. Kunt u bepalen welk VHD vastzit de schijf door te schakelen van de URL van de VHD.
6.  Hiermee bepaalt u met kunt dat er niets is actief met behulp van de gegevens.
7.  Klik op **loskoppelen** op het blad **schijf details** .
8.  De schijf moet nu worden losgekoppeld van de VM, en de VHD niet langer een lease nodig hebt op is geïnstalleerd. Het kan enkele minuten duren voordat de lease verschijnen duren. Om te bevestigen dat de lease is uitgebracht, gaat u naar **alle resources** > **Opslagaccountnaam** > **BLOB's** > **VHD's**. Klik in het deelvenster **Eigenschappen Blob** moet de waarde **Lease Status** **ontgrendeld**.

## <a name="what-is-a-lease"></a>Wat is een lease?

Een lease is een vergrendeling die kan worden gebruikt om toegang tot een blob (bijvoorbeeld een VHD). Wanneer een blob is lease, alleen de eigenaren van de lease, hebben toegang tot de blob. Een lease is het belangrijk om de volgende redenen:

-   Als u meerdere eigenaren probeert te schrijven naar hetzelfde gedeelte van de blob op hetzelfde moment kunnen beschadiging.
-   Dit wordt voorkomen dat de blob als iets actief gebruikt wordt (bijvoorbeeld een VM) wordt verwijderd.
-   Dit wordt voorkomen dat het opslag-account als iets actief gebruikt wordt (bijvoorbeeld een VM) wordt verwijderd.



## <a name="next-steps"></a>Volgende stappen

- [Een account opslagruimte verwijderen](storage-create-storage-account.md#delete-a-storage-account)
- [Hoe u de vergrendelde lease van blobopslag in Microsoft Azure (PowerShell) verbreken](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
