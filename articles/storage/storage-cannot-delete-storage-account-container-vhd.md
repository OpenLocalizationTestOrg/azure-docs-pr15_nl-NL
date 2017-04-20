<properties
    pageTitle="Problemen met Azure opslag accounts, containers of VHD's in een klassieke implementatie verwijderen | Microsoft Azure"
    description="Problemen met Azure opslag accounts, containers of VHD's in een klassieke implementatie verwijderen"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="tysonn"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="genli"/>

# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a>Problemen met Azure opslag accounts, containers of VHD's in een klassieke implementatie verwijderen

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

U kunt fouten mogelijk ontvangt wanneer u probeert te verwijderen van de Azure opslag-account, container of VHD in de [portal van Azure](https://portal.azure.com/) of de [Azure klassieke portal](https://manage.windowsazure.com/). De problemen kunnen worden veroorzaakt door de volgende gevallen:

-   Wanneer u een VM verwijdert, worden de schijf en VHD niet automatisch verwijderd. Die mogelijk de reden voor fout van opslag account verwijderen. Verwijder de schijf niet we zodat u de schijf gebruiken kunt om te koppelen van een andere VM.

-   Er is nog steeds een lease op een schijf of de blob dat is gekoppeld aan de schijf.

Als uw probleem Azure niet wordt beantwoord in dit artikel, bezoekt u de Azure-forums op [MSDN en de stapel overloop](https://azure.microsoft.com/support/forums/). U kunt het probleem posten op deze forums of @AzureSupport op Twitter. U kunt ook een verzoek om Azure ondersteuning indienen door te selecteren op de site [Azure ondersteunen](https://azure.microsoft.com/support/options/) **ondersteuning** .

## <a name="symptoms"></a>Symptomen

De volgende sectie bevat algemene fouten die u ontvangt mogelijk wanneer u probeert te verwijderen van de Azure opslag accounts, containers of VHD's.

### <a name="scenario-1-unable-to-delete-a-storage-account"></a>Scenario 1: Kan niet een opslag-account verwijderen

Als u naar het account opslag in de [portal van Azure](https://portal.azure.com/) of [Azure klassieke portal](https://manage.windowsazure.com/) en selecteer **verwijderen**navigeert, ziet u mogelijk het volgende foutbericht weergegeven:

*Opslag account StorageAccountName bevat VM afbeeldingen. Controleer of dat deze VM afbeeldingen worden verwijderd voordat u dit account opslag verwijdert.*

Deze fout kan ook worden weergegeven:

**Klik op het Azure-portal**:

*Opslag account < vm---opslagaccountnaam > verwijderen is mislukt. Kan niet worden verwijderd opslag account < vm---opslagaccountnaam >: ' opslag account < vm---opslagaccountnaam > heeft enkele actieve afbeelding(en) en/of schijven. Zorgen dat deze afbeelding(en) en/of schijven worden verwijderd voordat u dit account opslag verwijdert.'.*

**Klik op het Azure klassieke portal**:

*Opslag account < vm---opslagaccountnaam > heeft enkele actieve afbeelding(en) en/of schijven, bijvoorbeeld xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Controleer of deze afbeelding(en) en/of schijven worden verwijderd voordat u dit account opslag verwijdert.*

Of

**Klik op het Azure-portal**:

*Opslag account < vm---opslagaccountnaam > heeft 1 container(s) waarvoor een actieve afbeelding en/of schijf onderdelen. Zorgen dat deze onderdelen worden verwijderd uit de bibliotheek van de afbeelding voordat u dit account opslag verwijdert*.

**Klik op het Azure klassieke portal**:

*Is mislukt opslag indienen account < vm---opslagaccountnaam > heeft 1 container(s) waarvoor een actieve afbeelding en/of schijf onderdelen. Controleer of dat deze onderdelen worden verwijderd uit de bibliotheek van de afbeelding voordat u dit account opslag verwijdert. Wanneer u probeert te verwijderen van een opslag-account en er nog steeds actieve schijven die zijn gekoppeld zijn, ziet u een bericht dat u er zijn actieve schijven die moeten worden verwijderd*.

### <a name="scenario-2-unable-to-delete-a-container"></a>Scenario 2: Kan niet een container verwijderen

Wanneer u probeert te verwijderen van de container opslag, ziet u mogelijk het volgende foutbericht weergegeven:

*Opslag container verwijderen is mislukt <container name>. Fout: "Er is momenteel een lease op de container en geen lease-ID is opgegeven in het verzoek*.

### <a name="scenario-3-unable-to-delete-a-vhd"></a>Scenario 3: Kan niet een VHD verwijderen

Nadat u een VM verwijderen en vervolgens probeert iets te verwijderen van de BLOB's voor de bijbehorende VHD's, kan het volgende bericht worden weergegeven:

*Verwijderen van blob is mislukt ' pad/XXXXXX-XXXXXX-os-1447379084699.vhd'. Fout: "Er is momenteel een lease op de blob en geen lease-ID is opgegeven in het verzoek.*

## <a name="solution"></a>Oplossing
Probeer de meest voorkomende problemen oplossen door de volgende methode:

### <a name="step-1-delete-any-os-disks-that-are-preventing-deletion-of-the-storage-account-container-or-vhd"></a>Stap 1: OS schijven die verhinderen verwijdering van de opslag-account, container of VHD dat verwijderen

1. Overschakelen naar de [klassieke Azure-portal](https://manage.windowsazure.com/).
2. Selecteer **virtuele MACHINE** > **schijven**.

    ![Afbeelding van schijven op virtuele machines op Azure klassieke portal.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)

3. Zoek de schijven die zijn gekoppeld aan de opslag-account, container of VHD die u wilt verwijderen. Wanneer u de locatie van de schijf controleren, vindt u de bijbehorende opslag-account, container of VHD.

    ![Afbeelding waarin wordt aangegeven locatiegegevens voor schijven op Azure klassieke portal](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)

4. Controleer of er geen VM is opgenomen in het veld **die zijn bijgevoegd bij** schijven en verwijder vervolgens de schijven.

    > [AZURE.NOTE] Als een schijf is gekoppeld aan een VM, is het niet mogelijk om deze te verwijderen. Schijven losgekoppeld van een verwijderde VM asynchroon. Het kan enkele minuten nadat de VM wordt verwijderd voor dit veld om op te ruimen duren.

### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-the-storage-account-or-container"></a>Stap 2: VM afbeeldingen die verhinderen verwijdering van de opslag-account of de container dat verwijderen

1. Overschakelen naar de [klassieke Azure-portal](https://manage.windowsazure.com/).
2. Selecteer **virtuele MACHINE** > **afbeeldingen**, en verwijder vervolgens de afbeeldingen die zijn gekoppeld aan de opslag-account, container of VHD.

    Daarna kunt u opnieuw de opslag-account, de container of de VHD te verwijderen.

> [AZURE.WARNING] Zorg ervoor dat u een back-up alles wat die u opslaan wilt voordat u het account verwijderen. Dit is niet mogelijk is om te herstellen van een verwijderde opslag-account of een van de inhoud die vóór verwijdering bevat ophalen. Dit ook geldt voor alle resources in het account: zodra u een VHD, blob, tabel, wachtrij of bestand verwijdert, wordt deze permanent verwijderd. Controleer of de resource niet wordt gebruikt is.

## <a name="about-the-stopped-deallocated-status"></a>Over de status gestopt (opgeheven)

VMs die zijn gemaakt in het implementatiemodel klassieke en die zijn gehouden heeft de status **gestopt (opgeheven)** op het [Azure-portal](https://portal.azure.com/) of [Azure klassieke portal](https://manage.windowsazure.com/).

**Azure klassieke portal**:

![Gestopt (Deallocated) status voor VMs op Azure-portal.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)


**Azure-portal**:

![Gestopt (opgeheven) status voor VMs op Azure klassieke portal.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

De status van een 'Gestopt (opgeheven)' vrij de computerbronnen, zoals de CPU, geheugen en netwerk. De schijven, blijven echter nog steeds behouden zodat u kunt snel opnieuw maken de VM indien nodig. Deze schijven worden boven aan de VHD's, die worden ondersteund door Azure opslag gemaakt. Het account opslagruimte heeft deze VHD's en de schijven lease hebben op deze VHD's.

## <a name="next-steps"></a>Volgende stappen

- [Een account opslagruimte verwijderen](storage-create-storage-account.md#delete-a-storage-account)
- [Hoe u de vergrendelde lease van blobopslag in Microsoft Azure (PowerShell) verbreken](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
