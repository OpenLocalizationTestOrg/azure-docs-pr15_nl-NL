<properties
    pageTitle="Het maken, beheren of verwijderen van een account opslag in de Portal Azure | Microsoft Azure"
    description="Een nieuw account voor de opslag maken, beheren van uw account toegangstoetsen of een account opslag in de Portal Azure verwijderen. Meer informatie over de opslag van standard en premium-accounts."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="robinsh"/>


# <a name="about-azure-storage-accounts"></a>Over accounts Azure opslag

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Overzicht

Een account Azure opslagruimte biedt een unieke naamruimte opslaan van en toegang tot uw gegevensobjecten Azure Storage. Alle objecten in een account opslag zijn samen gefactureerd als een groep. Standaard is de gegevens in uw account alleen beschikbaar voor u, de eigenaar van het account.

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Opslag account facturering

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

> [AZURE.NOTE] Wanneer u een Azure virtuele machines maakt, wordt een opslag-account u automatisch gemaakt op de locatie van de implementatie als u nog een account opslagruimte op die locatie. Het is dus niet nodig is om te volgen van de onderstaande stappen voor het maken van een account opslagruimte voor uw VM schijven. De opslagaccountnaam gebaseerd op de naam van de virtuele machine. Zie de [documentatie van Azure virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) voor meer informatie.

## <a name="storage-account-endpoints"></a>Opslag account eindpunten

Elk object die u in de opslagruimte van de Azure opslaat heeft een unieke URL-adres. De opslagaccountnaam vormt het subdomein van dat adres. De combinatie van subdomein en domein, dat wil elke service-specifieke zeggen, vormt een *eindpunt* voor uw account opslag.

Als uw account opslag *mystorageaccount heet*, klikt u vervolgens zijn de Standaardeindpunten voor uw account opslag bijvoorbeeld:

- BLOB-service: http://*mystorageaccount*. blob.core.windows.net

- Tabel van service: http://*mystorageaccount*. table.core.windows.net

- Wachtrij service: http://*mystorageaccount*. queue.core.windows.net

- File-service: http://*mystorageaccount*. file.core.windows.net

> [AZURE.NOTE] Een account van Blob storage beschrijft alleen het Blob service-eindpunt.

De URL voor toegang tot een object in een opslag-account is gemaakt door de locatie van het object in het account opslag naar het eindpunt toe te voegen. Stel, dat een adres blob hebt deze indeling: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

U kunt ook een aangepaste domeinnaam wilt gebruiken met uw account opslag configureren. Zie voor klassieke opslag-accounts [configureren een aangepast domein naam voor uw Blob Storage-eindpunt](storage-custom-domain-name.md) voor meer informatie. Deze functie is nog niet toegevoegd bij de [portal van Azure](https://portal.azure.com) voor resourcemanager opslag-accounts, maar kunt u deze configureren met PowerShell. Zie de cmdlet [Set-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607146.aspx) voor meer informatie.  

## <a name="create-a-storage-account"></a>Een opslag-account maken

1. Meld u aan bij de [portal van Azure](https://portal.azure.com).

2. Selecteer **Nieuw**op het menu Hub -> **gegevens + opslagruimte** -> **opslag-account**.

3. Voer een naam voor uw account opslag. Zie [opslagruimte account eindpunten](#storage-account-endpoints) voor meer informatie over hoe de opslagaccountnaam worden gebruikt om de objecten in Azure Storage.

    > [AZURE.NOTE] Opslag accountnamen moeten tussen 3 en 24 tekens lang en kunnen bevatten getallen en alleen van toepassing zijn op kleine letters.
    >  
    > De naam van uw opslag-account moet uniek zijn binnen Azure. De portal van Azure wordt aangegeven of de opslagaccountnaam die u selecteert zich al in gebruik.

4. Geef de implementatiemodel moet worden gebruikt: **Resourcemanager** of **klassieke**. **Resourcemanager** is het aanbevolen implementatiemodel. Zie [lidmaatschap resourcemanager implementatie en klassieke implementatie](../resource-manager-deployment-model.md)voor meer informatie.

    > [AZURE.NOTE] BLOB storage accounts kunnen alleen worden gemaakt met het implementatiemodel resourcemanager.

5. Selecteer het type account opslag: **algemene** of **-blobopslag**. **Algemene** is de standaardwaarde.

    Als u **algemene** hebt ingeschakeld en geef vervolgens de laag prestaties: **standaard** of **Premium**. De standaardinstelling is **standaard**. Zie [Inleiding tot Microsoft Azure Storage](storage-introduction.md) voor meer informatie over de opslag van standard en premium-accounts, en [Premium opslag: High-Performance-opslag voor Azure virtuele machines werkbelasting](storage-premium-storage.md).

    Als **Blob Storage** is geselecteerd, geef de laag access: **warm** of **leuke**. De standaardinstelling is **warm**. Zie [Azure-blobopslag: Cool en directe lagen](storage-blob-storage-tiers.md) voor meer informatie.

6. Selecteer de optie herhaling voor de opslag-account: **LRS**, **GRS**, **AB-GRS**of **ZRS**. De standaardinstelling is **AB-GRS**. Zie voor meer informatie over replicatieopties van Azure-opslag, [Azure Storage herhaling](storage-redundancy.md).

7. Selecteer het abonnement waarin u wilt maken van het nieuwe opslag-account.

8. Een nieuwe resourcegroep opgeven of Selecteer een bestaande resourcegroep. Zie [overzicht van de Azure resourcemanager](../azure-resource-manager/resource-group-overview.md)voor meer informatie over resourcegroepen.

9. Selecteer de geografische locatie voor uw account opslag. Zie [Azure regio's](https://azure.microsoft.com/regions/#services) voor meer informatie over welke services beschikbaar in welke regio zijn.

10. Klik op **maken** om de opslag-account maken.

## <a name="manage-your-storage-account"></a>Uw account opslag beheren

### <a name="change-your-account-configuration"></a>De configuratie van uw account wijzigen

Nadat u uw opslagruimte-account hebt gemaakt, kunt u de configuratie, zoals het wijzigen van de replicatieoptie is gebruikt voor het account wijzigen of het wijzigen van de laag toegang voor een Blob storage-account. Ga naar uw account opslag in de [portal van Azure](https://portal.azure.com), klikt u op **alle instellingen** en klik op **configuratie** als u wilt weergeven en/of de accountconfiguratie te wijzigen.

> [AZURE.NOTE] Afhankelijk van de prestaties laag die u ervoor hebt gekozen bij het maken van het account opslag, sommige replicatieopties mogelijk niet beschikbaar.

Wijzigen van de optie herhaling verandert uw prijzen. Voor meer informatie, Zie [Azure opslag prijzen](https://azure.microsoft.com/pricing/details/storage/) pagina.

Voor Blob storage-accounts wijzigen van de access-laag mogelijk rekening gebracht kosten voor de wijziging naast het wijzigen van uw prijzen. Raadpleeg de [Blob storage accounts: prijzen en facturering](storage-blob-storage-tiers.md#pricing-and-billing) voor meer informatie.

### <a name="manage-your-storage-access-keys"></a>Uw toegangstoetsen opslag beheren

Wanneer u een account opslag maakt, genereert Azure twee 512 bits opslag toegangstoetsen, die worden gebruikt voor verificatie als de opslagruimte-account wordt geopend. Doordat twee opslag toegangstoetsen, kunt Azure u de toetsen met uw storage-service niet onderbroken of toegang tot deze service genereren.

> [AZURE.NOTE] Het is raadzaam dat u voorkomen dat uw opslagruimte toegangstoetsen delen met iemand anders. U kunt een *handtekening voor gedeelde access*gebruiken om toegang te verlenen tot opslagbronnen zonder het geven van uw toegangstoetsen. De handtekening van een gedeelde toegang biedt toegang tot een bron in uw account voor een interval die u definieert en met de machtigingen die u opgeeft. Zie [Gebruik van gedeelde Access handtekeningen (SA's)](storage-dotnet-shared-access-signature-part-1.md) voor meer informatie.

#### <a name="view-and-copy-storage-access-keys"></a>Bekijken en de toegangstoetsen opslag kopiëren

Ga naar uw account opslag in de [portal van Azure](https://portal.azure.com), klikt u op **alle instellingen** en klik op **toegangstoetsen** als u wilt weergeven, kopiëren en de toegangstoetsen van uw account te genereren. Het blad **Toegangstoetsen** bevat ook verbindingstekenreeksen met de vooraf geconfigureerde met uw primaire en secundaire toetsen die u kopiëren kunt als u wilt gebruiken in uw toepassingen.

#### <a name="regenerate-storage-access-keys"></a>Toegangstoetsen opslag genereren

Het is raadzaam dat u de toegangstoetsen wijzigen bij uw account opslag regelmatig om uw verbindingen opslag te beveiligen. Twee toegangstoetsen zijn toegewezen, zodat u verbindingen met het account opslag onderhouden kunt met behulp van een access-toets ingedrukt terwijl u de andere access-sleutel opnieuw genereren.

> [AZURE.WARNING] Opnieuw genereren van uw toegangstoetsen kunt invloed op services in Azure, evenals uw eigen toepassingen die afhankelijk van het account opslag zijn. Alle clients die de access-toets gebruiken voor toegang tot het account opslag moeten worden bijgewerkt als de nieuwe sleutel wilt gebruiken.

**Mediaservices** - als u mediaservices die afhankelijk van uw opslag-account zijn hebt, u moet opnieuw synchroniseren de toegangstoetsen met uw service media nadat u de toetsen genereren.

**Toepassingen** - als u webtoepassingen of cloudservices met het opslag-account hebt, gaan verloren de verbindingen als u toetsen, tenzij u uw sleutels implementeren.

**Opslag Explorers** - als u alle [toepassingen moeten worden opgeslagen explorer](storage-explorers.md)gebruikt, waarschijnlijk moet u de opslag-sleutel die wordt gebruikt door deze toepassingen bijgewerkt.

Dit is het proces voor het draaien van uw opslagruimte toegangstoetsen:

1. De verbindingstekenreeksen in uw toepassingscode als u verwijst naar de secundaire toegangstoets van het account opslag bijwerken.

2. De primaire toegangstoets voor uw account opslag genereren. Klik op het blad **Toegangstoetsen** op **Sleutel1 genereren**en klik vervolgens op **Ja** om te bevestigen dat u wilt een nieuwe sleutel genereren.

3. De verbindingstekenreeksen in uw code om te verwijzen naar de nieuwe primaire toegangstoets bijwerken.

4. De secundaire toegangstoets genereren op dezelfde manier.

## <a name="delete-a-storage-account"></a>Een account opslagruimte verwijderen

Verwijderen van een opslag-account dat u niet langer gebruikt, navigeer naar de opslag-account in de [portal van Azure](https://portal.azure.com), en klik op **verwijderen**. Een account opslag verwijdert, wordt de hele account, waaronder alle gegevens in het account.

> [AZURE.WARNING] Dit is niet mogelijk is om te herstellen van een verwijderde opslag-account of een van de inhoud die vóór verwijdering bevat ophalen. Zorg ervoor dat u een back-up alles wat die u opslaan wilt voordat u het account verwijderen. Dit ook geldt voor alle resources in het account, nadat u een blob, tabel, wachtrij of bestand verwijdert, wordt deze permanent verwijderd.

Als u wilt een opslag-account dat is gekoppeld aan een Azure virtuele machines verwijdert, moet u eerst ervoor zorgen dat alle schijven VM zijn verwijderd. Als u uw schijven VM niet eerst verwijdert, klikt u vervolgens ziet wanneer u probeert te verwijderen van uw account opslag, u een foutbericht vergelijkbaar met:

    Failed to delete storage account <vm-storage-account-name>. Unable to delete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.

Als de opslag-account gebruikt het model Klassiek implementatie, kunt u de schijf VM verwijderen door deze stappen uit in de [portal van Azure](https://manage.windowsazure.com):

1. Ga naar de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Ga naar het tabblad virtuele Machines.
3. Klik op het tabblad schijven.
4. Selecteer de gegevensschijf en klik op de schijf verwijderen.
5. Als u wilt verwijderen schijf afbeeldingen, Ga naar het tabblad afbeeldingen en afbeeldingen die zijn opgeslagen in het account verwijderen.

Zie de [documentatie van Azure virtuele machines](http://azure.microsoft.com/documentation/services/virtual-machines/)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- [Azure-blobopslag: Fraaie en warm lagen](storage-blob-storage-tiers.md)
- [Azure opslag herhaling](storage-redundancy.md)
- [Azure opslag verbindingstekenreeksen configureren](storage-configure-connection-string.md)
- [Gegevens met het hulpprogramma AzCopy-opdrachtregel overbrengen](storage-use-azcopy.md)
- Ga naar de [teamblog Azure opslag](http://blogs.msdn.com/b/windowsazurestorage/).
