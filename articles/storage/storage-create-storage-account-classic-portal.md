<properties
    pageTitle="Het maken, beheren of verwijderen van een account opslag in de klassieke Azure-Portal | Microsoft Azure"
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

Een account Azure opslagruimte hebt u toegang tot de services Azure Blob, wachtrij tabel en bestand in Azure opslag. Uw opslag-account bevat de unieke naamruimte voor uw gegevensobjecten Azure Storage. Standaard is de gegevens in uw account alleen beschikbaar voor u, de eigenaar van het account.

Er zijn twee typen opslag-accounts:

- Een standaard-opslag-account bevat Blob, tabel, wachtrij en bestand opslag.
- Een account van de opslagruimte premium ondersteunt momenteel alleen Azure virtuele machines schijven. Zie [Premium opslag: High-performance-opslag voor Azure virtuele machines werkbelasting](storage-premium-storage.md) voor een uitgebreide overzicht van de Premium-opslag.

## <a name="storage-account-billing"></a>Opslag account facturering

U zijn gefactureerd voor gebruik van de Azure-opslagruimte op basis van uw account opslag. Opslagkosten zijn gebaseerd op vier factoren: opslagcapaciteit, herhaling kleurenschema, opslag transacties en gegevens egress.

- Opslagcapaciteit verwijst naar hoeveel van uw account-toewijzing van opslagruimte u bent voor het opslaan van gegevens. De kosten van uw gegevens gewoon opslaat, wordt bepaald door de hoeveelheid gegevens die u opslaat en hoe deze worden gerepliceerd.
- Replicatie bepaalt hoeveel exemplaren van uw gegevens worden bijgehouden in één keer en op welke plaatsen.
- Transacties verwijzen naar alle lezen en schrijven bewerkingen met Azure Storage.
- Gegevens egress verwijst naar de gegevens die zijn verzonden uit een Azure regio. Als de gegevens in uw account opslag wordt geopend door een toepassing die niet wordt uitgevoerd in dezelfde regio, of die toepassing een cloudservice of een ander type van toepassing, wordt klikt u vervolgens geheven voor gegevens egress. (Voor Azure-services, kunt u stappen voor het groeperen van uw gegevens en services in de dezelfde datacenters naar minder of geen egress datakosten duren.)  

De pagina [Azure opslag prijzen](https://azure.microsoft.com/pricing/details/storage) vindt u gedetailleerde prijsinformatie voor de opslagcapaciteit, herhaling en transacties. De pagina [Gegevens overdrachten prijzen Details](https://azure.microsoft.com/pricing/details/data-transfers/) vindt u gedetailleerde prijsinformatie voor gegevens egress.

Zie voor meer informatie over de opslag account capaciteit en prestaties doelen [Azure opslag schaalbaarheid en prestaties doelen](storage-scalability-targets.md).

> [AZURE.NOTE] Wanneer u een Azure virtuele machines maakt, wordt een opslag-account u automatisch gemaakt op de locatie van de implementatie als u nog een account opslagruimte op die locatie. Het is dus niet nodig is om te volgen van de onderstaande stappen voor het maken van een account opslagruimte voor uw VM schijven. De opslagaccountnaam gebaseerd op de naam van de virtuele machine. Zie de [documentatie van Azure virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) voor meer informatie.

## <a name="create-a-storage-account"></a>Een opslag-account maken

1. Meld u aan bij de [Portal van Azure klassieke](https://manage.windowsazure.com).

2. Klik op **Nieuw** op de taakbalk onder aan de pagina. Kies **Gegevensservices** | **opslag**, en klik vervolgens op **Snelle maken**.

    ![NewStorageAccount](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)

3. Voer een naam voor uw account opslag in de **URL**.

    > [AZURE.NOTE] Opslag accountnamen moeten tussen 3 en 24 tekens lang en kunnen bevatten getallen en alleen van toepassing zijn op kleine letters.
    >  
    > De naam van uw opslag-account moet uniek zijn binnen Azure. De klassieke Portal Azure wordt aangegeven als de opslagaccountnaam die u selecteert wordt al gebruikt.

    Zie [opslagruimte account eindpunten](#storage-account-endpoints) hieronder voor meer informatie over hoe de opslagaccountnaam worden gebruikt om de objecten in Azure Storage.

4. Selecteer een locatie voor uw opslag-account dat dicht bij u of naar uw klanten in **De groep locatie/affiniteit**. Als gegevens in uw account opslag worden gebruikt in een andere Azure-service, zoals een Azure virtuele machines of cloudservice, wilt u mogelijk een groep affiniteit Selecteer in de lijst om te groeperen van uw account opslag in de dezelfde datacenter met andere Azure-services die u gebruikt om prestaties en lagere kosten te verbeteren.

    Houd er rekening mee dat u een groep affiniteit selecteren moet wanneer uw opslag-account is gemaakt. U kunt een bestaand account niet verplaatsen naar een groep affiniteit. Zie voor meer informatie over affiniteit groepen, [collega locatie van de Service met een groep affiniteit](#service-co-location-with-an-affinity-group) onderstaande.

    >[AZURE.IMPORTANT] Als u wilt bepalen welke locaties beschikbaar zijn voor uw abonnement, kunt u de bewerking [lijst van alle resource-providers](https://msdn.microsoft.com/library/azure/dn790524.aspx) bellen. [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx)bellen om providers van PowerShell te geven. Gebruik de methode [lijst](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) van de klasse ProviderOperationsExtensions in .NET.
    >
    >Zie ook [Azure regio's](https://azure.microsoft.com/regions/#services) voor meer informatie over welke services beschikbaar in welke regio zijn.


5. Als u meer dan één Azure abonnement hebt, wordt het veld **abonnement** weergegeven. Voer de Azure abonnement dat u wilt gebruiken het account opslagruimte met in- **abonnement**.

6. Selecteer het gewenste niveau van herhaling voor uw account opslagruimte in **herhaling**. De replicatieoptie aanbevolen is geografische-redundante herhaling, waarmee de maximale levensduur voor uw gegevens. Zie voor meer informatie over replicatieopties van Azure-opslag, [Azure Storage herhaling](storage-redundancy.md).

6. Klik op de **opslag-Account maken**.

    Duurt een paar minuten om uw account opslag te maken. U kunt u de status controleren door de meldingen onderaan in de klassieke Azure-Portal controleren. Nadat het opslag-account is gemaakt, wordt uw nieuwe opslag-account heeft **Online** status en gereed is voor gebruik.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)


### <a name="storage-account-endpoints"></a>Opslag account eindpunten

Elk object die u in de opslagruimte van de Azure opslaat heeft een unieke URL-adres. De opslagaccountnaam vormt het subdomein van dat adres. De combinatie van subdomein en domein, dat wil elke service-specifieke zeggen, vormt een *eindpunt* voor uw account opslag.

Als uw account opslag *mystorageaccount heet*, klikt u vervolgens zijn de Standaardeindpunten voor uw account opslag bijvoorbeeld:

- BLOB-service: http://*mystorageaccount*. blob.core.windows.net

- Tabel van service: http://*mystorageaccount*. table.core.windows.net

- Wachtrij service: http://*mystorageaccount*. queue.core.windows.net

- File-service: http://*mystorageaccount*. file.core.windows.net

Zodra het account is gemaakt, kunt u de eindpunten bekijken voor uw account opslagruimte op het dashboard opslag in de [Klassieke Azure-Portal](https://manage.windowsazure.com) .

De URL voor toegang tot een object in een opslag-account is gemaakt door de locatie van het object in het account opslag naar het eindpunt toe te voegen. Stel, dat een adres blob hebt deze indeling: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

U kunt ook een aangepaste domeinnaam wilt gebruiken met uw account opslag configureren. Zie [een aangepaste domeinnaam voor uw blob storage eindpunt configureren](storage-custom-domain-name.md) voor meer informatie.

### <a name="service-co-location-with-an-affinity-group"></a>Locatie van samen met een groep affiniteit

Een *groep affiniteit* is een geografische groepering van uw Azure services en VMs met uw account Azure opslag. Een groep affiniteit kunt serviceprestaties verbeteren door te zoeken van computer werkbelasting in het dezelfde Datacenter of nabij de doelgroep van de gebruiker. Ook geen facturering kosten verbonden zijn egress wanneer gegevens in een account voor de opslag van een andere service die deel uitmaakt van dezelfde groep affiniteit wordt geopend.

> [AZURE.NOTE]  Als u wilt een affiniteit groep hebt gemaakt, opent u het gedeelte <b>Instellingen</b> van de [Azure klassieke Portal](https://manage.windowsazure.com), klikt u op <b>affiniteit groepen</b>en klikt u op <b>een groep affiniteit toevoegen</b> of de knop <b>toevoegen</b> . U kunt ook groepen maken en beheren affiniteit met behulp van de Azure-API voor het beheer van Service. Zie <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">bewerkingen op affiniteit groepen</a> voor meer informatie.

## <a name="view-copy-and-regenerate-storage-access-keys"></a>Bekijken, kopiëren en opslag toegangstoetsen genereren

Wanneer u een account opslag maakt, genereert Azure twee 512 bits opslag toegangstoetsen, die worden gebruikt voor verificatie als de opslagruimte-account wordt geopend. Doordat twee opslag toegangstoetsen, kunt Azure u de toetsen met uw storage-service niet onderbroken of toegang tot deze service genereren.

> [AZURE.NOTE] Het is raadzaam dat u voorkomen dat uw opslagruimte toegangstoetsen delen met iemand anders. U kunt een *handtekening voor gedeelde access*gebruiken om toegang te verlenen tot opslagbronnen zonder het geven van uw toegangstoetsen. De handtekening van een gedeelde toegang biedt toegang tot een bron in uw account voor een interval die u definieert en met de machtigingen die u opgeeft. Zie [Gebruik van gedeelde Access handtekeningen (SA's)](storage-dotnet-shared-access-signature-part-1.md) voor meer informatie.

Klik in de [Klassieke Azure-Portal](https://manage.windowsazure.com)gebruik **Beheren** op het dashboard of de pagina **opslag** om te bekijken, kopiëren en de toegangstoetsen opslag die worden gebruikt voor toegang tot de services Blob, tabel en wachtrij genereren.

### <a name="copy-a-storage-access-key"></a>Kopieer een toegangstoets opslag  

**Sleutels beheren** kunt u een toegangstoets opslagruimte wilt gebruiken in een verbindingsreeks kopiëren. De verbindingsreeks is de naam van het opslag-account en een toets om te gebruiken in verificatie vereist. Voor informatie over het configureren van verbindingstekenreeksen met de voor toegang tot Azure opslagservices, raadpleegt u [Verbindingsreeks van de Azure-opslag configureren](storage-configure-connection-string.md).

1. Klik in de [Klassieke Azure-Portal](https://manage.windowsazure.com)op **opslag**en klik vervolgens op de naam van de opslag-account te openen van het dashboard.

2. Klik op **toetsen beheren**.

    **Toegangstoetsen beheren** wordt geopend.

    ![Managekeys](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)


3. Als u wilt kopiëren een toegangstoets opslag, selecteert u de belangrijkste tekst. Klik met de rechtermuisknop en klik op **kopiëren**.

### <a name="regenerate-storage-access-keys"></a>Toegangstoetsen opslag genereren
Het is raadzaam dat u de toegangstoetsen wijzigen bij uw account opslag regelmatig om uw verbindingen opslag te beveiligen. Twee toegangstoetsen zijn toegewezen, zodat u verbindingen met het account opslag onderhouden kunt met behulp van een access-toets ingedrukt terwijl u de andere access-sleutel opnieuw genereren.

> [AZURE.WARNING] Opnieuw genereren van uw toegangstoetsen kunt invloed op services in Azure, evenals uw eigen toepassingen die afhankelijk van het account opslag zijn. Alle clients die de access-toets gebruiken voor toegang tot het account opslag moeten worden bijgewerkt als de nieuwe sleutel wilt gebruiken.

**Mediaservices** - als u mediaservices die afhankelijk van uw opslag-account zijn hebt, u moet opnieuw synchroniseren de toegangstoetsen met uw service media nadat u de toetsen genereren.

**Toepassingen** - als u webtoepassingen of cloudservices met het opslag-account hebt, gaan verloren de verbindingen als u toetsen, tenzij u uw sleutels implementeren. 

**Opslag Explorers** - als u alle [toepassingen moeten worden opgeslagen explorer](storage-explorers.md)gebruikt, waarschijnlijk moet u de opslag-sleutel die wordt gebruikt door deze toepassingen bijgewerkt.

Dit is het proces voor het draaien van uw opslagruimte toegangstoetsen:

1. De verbindingstekenreeksen in uw toepassingscode als u verwijst naar de secundaire toegangstoets van het account opslag bijwerken.

2. De primaire toegangstoets voor uw account opslag genereren. Klik in de [Klassieke Azure-Portal](https://manage.windowsazure.com)van het dashboard of de pagina **configureren** , klikt u op **Sleutels beheren**. Klik op **toevoegen** onder de primaire toegangstoets, en klik vervolgens op **Ja** om te bevestigen dat u wilt een nieuwe sleutel genereren.

3. De verbindingstekenreeksen in uw code om te verwijzen naar de nieuwe primaire toegangstoets bijwerken.

4. De secundaire toegangstoets genereren.

## <a name="delete-a-storage-account"></a>Een account opslagruimte verwijderen

Als u wilt verwijderen van een opslag-account dat u niet langer gebruikt, gebruikt u **verwijderen** op het dashboard of de pagina **configureren** . **Verwijderen** Hiermee verwijdert u het hele opslag-account, inclusief alle de BLOB's, tabellen en wachtrijen in het account.

> [AZURE.WARNING] Dit is niet mogelijk is om te herstellen van een verwijderde opslag-account of een van de inhoud die vóór verwijdering bevat ophalen. Zorg ervoor dat u een back-up alles wat die u opslaan wilt voordat u het account verwijderen. Dit ook geldt voor alle resources in het account, nadat u een blob, tabel, wachtrij of bestand verwijdert, wordt deze permanent verwijderd.
>
> Als uw account opslag VHD-bestanden voor een Azure virtuele machine bevat, moet klikt u vervolgens u alle afbeeldingen en schijven dat deze bestanden VHD gebruikt voordat u het account opslag kunt verwijderen. Eerst de virtuele machine stoppen als deze wordt uitgevoerd en verwijder deze. Schijven verwijderd, Ga naar het tabblad **schijven** en er een schijven verwijderen. Afbeeldingen verwijderen, Ga naar het tabblad **afbeeldingen** en afbeeldingen die zijn opgeslagen in het account verwijderen.

1. Klik in de [Klassieke Azure-Portal](https://manage.windowsazure.com), op **opslag**.

2. Klik ergens in de opslagruimte post behalve de naam en klik vervolgens op **verwijderen**.

     - Of -

    Klik op de naam van het te openen van het dashboard opslag-account en klik vervolgens op **verwijderen**.

3. Klik op **Ja** om te bevestigen dat u wilt het opslag-account verwijderen.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de opslag van Azure, raadpleegt u de [documentatie van Azure-opslag](https://azure.microsoft.com/documentation/services/storage/).
- Ga naar de [teamblog Azure opslag](http://blogs.msdn.com/b/windowsazurestorage/).
- [Gegevens met het hulpprogramma AzCopy-opdrachtregel overbrengen](storage-use-azcopy.md)
