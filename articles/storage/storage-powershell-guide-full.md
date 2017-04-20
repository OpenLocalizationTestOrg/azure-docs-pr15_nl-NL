<properties
    pageTitle="Via Azure PowerShell met Azure opslagmedia | Microsoft Azure"
    description="Informatie over het gebruik van de Azure PowerShell-cmdlets voor Azure opslag maken en beheren van opslag accounts; werken met BLOB's, tabellen, wachtrijen en bestanden. configureren en opslag analytics query en gedeelde toegang handtekeningen maken."
    services="storage"
    documentationCenter="na"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="using-azure-powershell-with-azure-storage"></a>Gebruik van Azure PowerShell met Azure opslag

## <a name="overview"></a>Overzicht

Azure PowerShell is een module die cmdlets uit om het beheren van Azure via Windows PowerShell biedt. Dit is een opdrachtregel shell op basis van een taak en de scripttaal speciaal ontworpen voor systeembeheer. Met PowerShell, kunt u eenvoudig bepalen en het beheer van uw Azure services en toepassingen automatiseren. U kunt bijvoorbeeld de cmdlets de dezelfde taken die u via de [Portal van Azure uitvoeren kunt](https://portal.azure.com)uitvoeren.

In deze handleiding, bekijken we het gebruik van de [Azure opslag Cmdlets](https://msdn.microsoft.com/library/azure/mt269418.aspx) voor het uitvoeren van tal van ontwikkeling en beheer van de taken met Azure opslagmedia.

Deze handleiding wordt ervan uitgegaan dat u vorige ervaring hebt met [Azure opslag](https://azure.microsoft.com/documentation/services/storage/) - en [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx). De gids biedt een aantal scripts om te laten zien van het gebruik van PowerShell met Azure opslagmedia. De scriptvariabelen op basis van uw configuratie vóór elke script uitvoeren, moet u bijwerken.

De eerste sectie in deze handleiding biedt een snelle blik op Azure opslagruimte en PowerShell. Start in de [vereisten voor het gebruik van Azure PowerShell met Azure-opslag](#prerequisites-for-using-azure-powershell-with-azure-storage)voor gedetailleerde informatie en instructies.


## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Aan de slag met Azure opslagruimte en PowerShell 5 minuten

In dit gedeelte ziet u hoe u toegang hebben tot Azure-opslag via PowerShell 5 minuten.

**Ervaring met Azure:** Krijg een Microsoft Azure-abonnement en een Microsoft-account dat is gekoppeld aan dit abonnement. Zie voor informatie over opties voor Azure aankoop [Gratis proefversie](https://azure.microsoft.com/pricing/free-trial/), [Opties aanschaffen](https://azure.microsoft.com/pricing/purchase-options/)en [Lid biedt](https://azure.microsoft.com/pricing/member-offers/) (voor leden van MSDN, Microsoft Partner Network, en BizSpark en andere Microsoft-programma's).

Zie [beheerdersrollen toewijzen in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) voor meer informatie over Azure abonnementen.

**Na het maken van een abonnement op Microsoft Azure en account:**

1.  Download en installeer [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409).
2.  Start Windows PowerShell geïntegreerde uitvoeren van scripts-omgeving (wissen): In uw lokale computer, gaat u naar het menu **Start** . Typ **Beheerprogramma's** en klik op uit te voeren. Klik in het venster **Beheerprogramma's** met de rechtermuisknop op **Windows filter wissen**, klikt u op **als Administrator uitvoeren**.
3.  In **Windows PowerShell wissen**, klikt u op **bestand** > **Nieuw** om een nieuwe scriptbestand te maken.
4.  Nu krijgt u een eenvoudig script met basisopdrachten PowerShell toegang hebben tot Azure opslag. Het script vraagt eerst uw Azure accountreferenties om toe te voegen uw Azure-account in de lokale PowerShell-omgeving. Het script wordt vervolgens de standaardkleuren Azure abonnement instellen en het maken van een nieuw account voor de opslag in Azure wordt aangegeven. Het script wordt vervolgens een nieuwe container maken in deze nieuwe opslag-account en een afbeeldingsbestand (blob) uploaden naar dat onderdeel. Nadat het script alle BLOB's in die container lijsten, wordt er een nieuwe doelmap maken in uw lokale computer en downloaden van het afbeeldingsbestand.
5.  Selecteer het script tussen de opmerkingen in de volgende codesectie **#begint** en **#end**. Druk op CTRL + C om deze naar het Klembord kopiëren.

        #begin
        # Update with the name of your subscription.
        $SubscriptionName = "YourSubscriptionName"

        # Give a name to your new storage account. It must be lowercase!
        $StorageAccountName = "yourstorageaccountname"

        # Choose "West US" as an example.
        $Location = "West US"

        # Give a name to your new container.
        $ContainerName = "imagecontainer"

        # Have an image file and a source directory in your local computer.
        $ImageToUpload = "C:\Images\HelloWorld.png"

        # A destination directory in your local computer.
        $DestinationFolder = "C:\DownloadImages"

        # Add your Azure account to the local PowerShell environment.
        Add-AzureAccount

        # Set a default Azure subscription.
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

        # Create a new storage account.
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location

        # Set a default storage account.
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

        # Create a new container.
        New-AzureStorageContainer -Name $ContainerName -Permission Off

        # Upload a blob into a container.
        Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

        # List all blobs in a container.
        Get-AzureStorageBlob -Container $ContainerName

        # Download blobs from the container:
        # Get a reference to a list of all blobs in a container.
        $blobs = Get-AzureStorageBlob -Container $ContainerName

        # Create the destination directory.
        New-Item -Path $DestinationFolder -ItemType Directory -Force  

        # Download blobs into the local destination directory.
        $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
        #end

5.  In **Windows PowerShell wissen**, drukt u op CTRL + V om het script kopiëren. Klik op **bestand** > **Opslaan**. Typ de naam van het scriptbestand, zoals 'mystoragescript' in het dialoogvenster **OpslaanAls** . Klik op **Opslaan**.

6.  U moet nu de scriptvariabelen op basis van uw configuratie-instellingen bijwerken. U moet de variabele **$SubscriptionName** bijwerken met uw eigen abonnement. U kunt u de andere variabelen zoals opgegeven in het script of bijgewerkt om ze naar wens.

    - **$SubscriptionName:** Met de naam van uw eigen abonnement, moet u deze variabele bijwerken. Voer een van de volgende drie manieren om te zoeken de naam van uw abonnement:

        een. In **Windows PowerShell wissen**, klikt u op **bestand** > **Nieuw** om een nieuwe scriptbestand te maken. Het volgende script naar het nieuwe scriptbestand kopiëren en klik op **Foutopsporing** > **uitvoeren**. Het volgende script vraagt eerst uw Azure accountreferenties uw Azure toevoegen naar de lokale PowerShell-omgeving en klikt u vervolgens alle abonnementen die met de lokale PowerShell-sessie verbonden zijn weergeven. Moet u een rekening van de naam van het abonnement dat u gebruiken wilt terwijl deze zelfstudie te volgen:

            Add-AzureAccount
                Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName

        b. Als u wilt zoeken en de naam van uw abonnement kopiëren in de [Portal van Azure](https://portal.azure.com), in het menu Hub aan de linkerkant, klikt u op **abonnementen**. Kopieer de naam van het abonnement dat u gebruiken wilt tijdens het uitvoeren van scripts in deze handleiding.

        ![Azure-Portal][Image2]

        c. Als u wilt zoeken en kopieer de naam van uw abonnement in de [Klassieke Azure-Portal](https://manage.windowsazure.com/), schuif omlaag en klik op **Instellingen** aan de linkerkant van de portal. Klik op **abonnementen** voor een overzicht van uw abonnementen. Kopieer de naam van het abonnement dat u gebruiken wilt tijdens het uitvoeren van scripts gegeven in deze handleiding.

        ![Azure klassieke Portal][Image1]

    - **$StorageAccountName:** Gebruik van de opgegeven naam in het script of voer een nieuwe naam voor uw account opslag. **Belangrijke:** De naam van de opslag-account moet uniek zijn in Azure wordt aangegeven. Dit moet kleine letters, ook!

    - **$Location:** Gebruik van de opgegeven "West VS" in het script of kies andere Azure plaatsen, zoals Oost US, Noord-Europa, enzovoort.

    - **$ContainerName:** Gebruik van de opgegeven naam in het script of typ een nieuwe naam voor de container.

    - **$ImageToUpload:** Geef een pad op naar een afbeelding op uw lokale computer, zoals: "C:\Images\HelloWorld.png".

    - **$DestinationFolder:** Geef een pad op naar een lokale map voor het opslaan van bestanden die zijn gedownload van Azure-opslag, zoals: "C:\DownloadImages".

7.  Nadat de scriptvariabelen in het bestand "mystoragescript.ps1" zijn bijgewerkt, klikt u op **bestand** > **Opslaan**. Klik vervolgens op **fouten opsporen in** > **uitvoeren** of druk op **F5** het script uitvoeren.  

Wanneer het script is uitgevoerd, kunt u een lokale doelmap waarin het gedownloade bestand nodig hebt. De volgende schermafbeelding ziet u een voorbeeld-uitvoer:

![BLOB's downloaden][Image3]


> [AZURE.NOTE] De sectie "Aan de slag met Azure opslagruimte en PowerShell 5 minuten" aangeboden een snelle introductie over het gebruik van Azure PowerShell met Azure opslagmedia. Voor uitgebreide informatie en instructies raden we u aan het lezen van de volgende secties.

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Vereisten voor het gebruik van Azure PowerShell met Azure Storage
U moet een Azure-abonnement en het account dat u wilt uitvoeren van de PowerShell-cmdlets gegeven in deze handleiding, zoals hierboven is beschreven.

Azure PowerShell is een module die cmdlets uit om het beheren van Azure via Windows PowerShell biedt. Zie voor informatie over het installeren en instellen van Azure PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Het is aan te raden wij u downloaden en installeren of naar de meest recente Azure PowerShell-module upgraden vóór het gebruik van deze handleiding.

U kunt de cmdlets uitvoeren in de standaard Windows PowerShell-console of de Windows PowerShell geïntegreerde uitvoeren van scripts-omgeving (wissen). Bijvoorbeeld **Windows filter wissen**, gaat u naar het menu Start, typ beheerprogramma's en klik op uit te voeren. Klik in het venster beheerprogramma's met de rechtermuisknop op Windows filter wissen, klikt u op als Administrator uitvoeren.

## <a name="how-to-manage-storage-accounts-in-azure"></a>Het beheren van opslag-accounts in Azure wordt aangegeven

### <a name="how-to-set-a-default-azure-subscription"></a>Het instellen van een standaard Azure-abonnement
Als u wilt beheren via Azure PowerShell Azure-opslag, moet u uw clientomgeving met Azure via Azure Active Directory of verificatie op basis van certificaten verifiëren. Zie zelfstudie voor [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor gedetailleerde informatie. Deze handleiding wordt gebruikt voor de verificatie van de Azure Active Directory.

1.  In Windows PowerShell wissen, typt u de volgende opdracht uit om toe te voegen uw Azure-account in de lokale PowerShell-omgeving:

    `Add-AzureAccount`

2.  Typ het e-mailadres en wachtwoord in voor uw account in het venster "Aanmelden in naar Microsoft Azure". Azure wordt geverifieerd door de referentiegegevens opslaat en sluit het venster.

3.  Voer de volgende opdracht uit om te bekijken van de Azure accounts in uw lokale PowerShell-omgeving en controleer of uw account wordt vermeld:

    `Get-AzureAccount`

4.  Vervolgens voert u de volgende cmdlet om weer te geven van alle abonnementen die met de lokale PowerShell-sessie verbonden zijn en controleer of uw abonnement wordt weergegeven:

    `Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`

5.  Als u wilt instellen op een standaard Azure-abonnement, voert u de cmdlet Selecteer-AzureSubscription:

        $SubscriptionName = 'Your subscription Name'
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

6.  Controleer of u de naam van de standaardabonnement door de cmdlet Get-AzureSubscription uit te voeren:

    `Get-AzureSubscription -Default`

7.  Als u wilt zien van alle beschikbare PowerShell-cmdlets voor Azure opslag, voert u het:

    `Get-Command -Module Azure -Noun *Storage*`

### <a name="how-to-create-a-new-azure-storage-account"></a>Het maken van een nieuw account voor de opslag van Azure
Als u wilt gebruiken Azure opslag, moet u een opslag-account. Nadat u verbinding maken met uw abonnement op de computer hebt geconfigureerd, kunt u een nieuw Azure opslag-account maken.

1.  Voer de cmdlet Get-AzureLocation als u wilt zoeken naar alle datacenter van de beschikbare locaties:

    `Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes`

2.  Voer de cmdlet New-AzureStorageAccount om een nieuwe opslag-account maken. Het volgende voorbeeld wordt een nieuw account voor de opslag in de 'West tabellen' in Word.

        $location = "West US"
        $StorageAccountName = "yourstorageaccount"
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location

> [AZURE.IMPORTANT] De naam van uw account opslag moet uniek zijn binnen Azure en moet kleine letters. Zie [Over Azure opslag Accounts](storage-create-storage-account.md) en [Naming en verwijst naar Containers, BLOB's, en metagegevens](http://msdn.microsoft.com/library/azure/dd135715.aspx)voor naamgevingsconventies en beperkingen.

### <a name="how-to-set-a-default-azure-storage-account"></a>Het instellen van een standaardaccount voor de opslag van Azure
U kunt meerdere opslag-accounts hebben in uw abonnement. U kunt kiest u een van deze en stel deze als het standaardaccount voor de opslag voor alle opdrachten in de opslagruimte in dezelfde PowerShell-sessie. Hiermee kunt u de opdrachten Azure PowerShell-opslag uitvoeren zonder op te geven van de context opslag expliciet.

1.  Als u wilt instellen op een standaardaccount voor de opslagruimte voor uw abonnement, kunt u de cmdlet Set-AzureSubscription uitvoeren.

        $SubscriptionName = "Your subscription name"
        $StorageAccountName = "yourstorageaccount"  
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

2.  Voer de cmdlet Get-AzureSubscription om te bevestigen dat de opslag-account gekoppeld aan uw abonnement standaardaccount is. Deze opdracht geeft eigenschappen van het abonnement op het huidige abonnement, met inbegrip van de huidige opslag-account.

        Get-AzureSubscription –Current

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a>Hoe u alle Azure opslag-accounts in een abonnement
Elk Azure abonnement kan maximaal 100 opslag-accounts hebben geïnstalleerd. Zie [Azure-abonnement en limieten van de Service, quota's en beperkingen](../azure-subscription-service-limits.md)voor de meest recente informatie over limieten.

De volgende cmdlet om vast te stellen de naam en de status van de opslag-accounts in het huidige abonnement uitvoeren:

    Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus

### <a name="how-to-create-an-azure-storage-context"></a>Het maken van een context Azure opslag
Azure opslag context is een object in PowerShell voor het verpakken van de referenties voor de opslag. Gebruik een context opslag terwijl eventuele volgende cmdlet uitvoeren, u de kunt verificatie van uw aanvraag kunt invullen zonder op te geven het opslag-account en de sleutel access expliciet. U kunt een context opslagruimte op tal van manieren, zoals het gebruik van opslag naam en access accountsleutel, handtekening (SA's) van gedeelde toegangstoken, verbindingsreeks, maken of anonieme. Zie [Nieuw AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx)voor meer informatie.  

Gebruik een van de volgende drie manieren om te maken van een context opslag:

- Voer de cmdlet [Get-AzureStorageKey](http://msdn.microsoft.com/library/azure/dn495235.aspx) om vast te stellen de toegangstoets primaire opslagruimte voor uw account Azure opslag. Vervolgens de cmdlet [New-AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx) als u wilt maken van een context opslag bellen:

        $StorageAccountName = "yourstorageaccount"
        $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
        $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary


- Een gedeelde toegangstoken met handtekening voor een container Azure opslag genereren en deze gebruiken om u te maken van een context opslag:

        $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
        $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken

    Zie [Nieuw AzureStorageContainerSASToken](http://msdn.microsoft.com/library/azure/dn806416.aspx) en [Gebruik van gedeelde Access handtekeningen (SA's)](storage-dotnet-shared-access-signature-part-1.md)voor meer informatie.

- In sommige gevallen wilt u mogelijk de service-eindpunten opgeven wanneer u een nieuwe context voor de opslag maakt. Dit kan het nodig zijn wanneer u een aangepaste domeinnaam voor uw account opslagruimte hebt geregistreerd met de service Blob of u een handtekening gedeelde access gebruiken wilt voor toegang tot opslag resources. De service-eindpunten instellen in een verbindingsreeks en deze gebruiken om u te maken van een nieuwe context voor de opslag, zoals hieronder wordt weergegeven:

        $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
        $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString

Zie voor meer informatie over het configureren van een verbindingsreeks opslag [Tekenreeksen met de verbinding configureren](storage-configure-connection-string.md).

Nu dat u hebt ingesteld op uw computer en hebt geleerd hoe beheert abonnementen en -opslag-accounts via Azure PowerShell, gaat u naar de volgende sectie voor informatie over het beheren van Azure BLOB's en momentopnamen blob.

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a>Kunt ophalen en Azure opslag toetsen genereren

Een opslag van Azure-account wordt geleverd met twee account toetsen. U kunt de volgende cmdlet gebruiken om op te halen uw sleutels.

    Get-AzureStorageKey -StorageAccountName "yourstorageaccount"

Gebruik de volgende cmdlet om op te halen van een bepaalde toets. Geldige waarden zijn primaire en secundaire.  

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary

Als u uw sleutels genereren wilt, gebruikt u de volgende cmdlet. Geldige waarden voor - KeyType zijn "Primaire" en "Secundaire"

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Primary”

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Secondary”

## <a name="how-to-manage-azure-blobs"></a>Het beheren van Azure BLOB 's
Azure-blobopslag is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens, die toegankelijk is vanuit een willekeurige plaats in de wereld via HTTP of HTTPS. In deze sectie wordt ervan uitgegaan dat u al bekend met de concepten Azure Blob Storage-Service bent. Zie [aan de slag met gebruik van .NET-blobopslag](storage-dotnet-how-to-use-blobs.md) en [Blob Service concepten](http://msdn.microsoft.com/library/azure/dd179376.aspx)voor gedetailleerde informatie.

### <a name="how-to-create-a-container"></a>Het maken van een container
Elke blob in Azure opslag moet zich in een container. U kunt een privé container met de cmdlet New-AzureStorageContainer maken:

    $StorageContainerName = "yourcontainername"
    New-AzureStorageContainer -Name $StorageContainerName -Permission Off

> [AZURE.NOTE] Er zijn drie toegangsniveaus anonieme lezen: **uitschakelen**, **Blob**en **Container**. Als u wilt voorkomen dat anonieme toegang BLOB's, door de parameter van de machtiging in te stellen op **uitschakelen**. Standaard is de nieuwe container privé is en kan alleen worden geopend door de eigenaar van het account. Anonieme toegang in de openbare gelezen blob-resources, maar niet in de metagegevens van de container of aan de lijst met BLOB's in de container, stelt u de parameter machtiging naar **Blob**toestaan. Container metagegevens en de lijst met BLOB's in de container, wordt de parameter machtiging u volledige openbare leestoegang voor blob resources, stellen aan **Container**. Zie [anonieme leestoegang tot containers en BLOB's beheren](storage-manage-access-to-resources.md)voor meer informatie.

### <a name="how-to-upload-a-blob-into-a-container"></a>Het uploaden van een blob in een container
Azure-blobopslag ondersteunt blok BLOB's en pagina BLOB's. Zie [lidmaatschap blok BLOB's, BLOB's toevoegen en BLOB van pagina's](http://msdn.microsoft.com/library/azure/ee691964.aspx)voor meer informatie.

Als u wilt uploaden BLOB's in naar een container, kunt u de cmdlet [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) . Standaard wordt met deze opdracht de lokale bestanden uploadt naar een blob blokkeren. Als u wilt opgeven van het type voor de blob, kunt u de parameter - BlobType.

Het volgende voorbeeld wordt de cmdlet [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) om alle bestanden in de opgegeven map uitgevoerd en vervolgens doorgeven naar de volgende cmdlet met behulp van de operator verkooppijplijn. De cmdlet [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) kunnen de lokale bestanden uploaden naar uw container:

    Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"

### <a name="how-to-download-blobs-from-a-container"></a>Het downloaden van BLOB's uit een container
Het volgende voorbeeld wordt het downloaden van BLOB's uit een container. Het voorbeeld eerst verbinding een maken met Azure Storage gebruik van de context van de account opslag, waaronder de naam van de opslag-account en de van primaire toegangssleutel. Klik in het voorbeeld wordt een blob verwijzing met de cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) opgehaald. Het voorbeeld wordt de cmdlet [Get-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806418.aspx) vervolgens gebruikt om te downloaden van BLOB's in de lokale doelmap.

    #Define the variables.
    $ContainerName = "yourcontainername"
    $DestinationFolder = "C:\DownloadImages"

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #List all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Download blobs from a container.
    New-Item -Path $DestinationFolder -ItemType Directory -Force
    $blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a>Eén opslag container BLOB's kopiëren naar een andere
U kunt asynchroon BLOB's over de opslag accounts en regio's kopiëren. Het volgende voorbeeld wordt één opslag container BLOB's kopiëren naar een andere in twee verschillende opslag-accounts. Het voorbeeld stelt eerst variabelen voor de bron- en doeltabellen opslag-accounts en maakt u een context opslag voor elk account. Klik vervolgens kopiëren in het voorbeeld BLOB's in de Broncontainer naar de bestemmingscontainer met de cmdlet [Start-AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) . Het voorbeeld wordt ervan uitgegaan dat de bron- en doeltabellen opslag-accounts en containers al aanwezig.

    #Define the source storage account and context.
    $SourceStorageAccountName = "yoursourcestorageaccount"
    $SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
    $SrcContainerName = "yoursrccontainername"
    $SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

    #Define the destination storage account and context.
    $DestStorageAccountName = "yourdeststorageaccount"
    $DestStorageAccountKey = "Storage key for yourdeststorageaccount"
    $DestContainerName = "destcontainername"
    $DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

    #Get a reference to blobs in the source container.
    $blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

    #Copy blobs from one container to another.
    $blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext

Houd er rekening mee dat dit voorbeeld wordt een kopie asynchroon uitgevoerd. U kunt de status van elk exemplaar controleren door de cmdlet [Get-AzureStorageBlobCopyState](http://msdn.microsoft.com/library/azure/dn806406.aspx) uit te voeren.

### <a name="how-to-copy-blobs-from-a-secondary-location"></a>BLOB's van een secundaire locatie kopiëren
U kunt BLOB's kopiëren van de secundaire locatie van een account AB GRS is ingeschakeld.

    #define secondary storage context using a connection string constructed from secondary endpoints.
    $SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
    Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext

### <a name="how-to-delete-a-blob"></a>Het verwijderen van een blob
U verwijdert een blob, eerst u blob verwijst en vervolgens de cmdlet verwijderen-AzureStorageBlob bellen op is geïnstalleerd. Het volgende voorbeeld Hiermee verwijdert u alle BLOB's in een bepaalde container. Het voorbeeld stelt eerst variabelen voor een account opslag, en maakt vervolgens een context opslag. Klik vervolgens in het voorbeeld haalt een blob verwijzing met de cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) en wordt de cmdlet [Verwijderen-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806399.aspx) als u wilt BLOB's verwijderen uit een container van Azure opslag uitgevoerd.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "containername"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to all the blobs in the container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Delete blobs in a specified container.
    $blobs| Remove-AzureStorageBlob

## <a name="how-to-manage-azure-blob-snapshots"></a>Het beheren van Azure blob momentopnamen
Azure kunt u een momentopname van een blob maken. Een momentopname is een alleen-lezen-versie van een blob dat wordt uitgevoerd op een punt in tijd. Wanneer u een momentopname hebt gemaakt, kan deze worden gelezen, gekopieerd, of verwijderd, maar niet gewijzigd. Momentopnamen zelf een formule back-up van een blob zoals deze wordt weergegeven op een moment. Zie [een momentopname van een Blob maken](http://msdn.microsoft.com/library/azure/hh488361.aspx)voor meer informatie.

### <a name="how-to-create-a-blob-snapshot"></a>Het maken van een momentopname blob
Als u een snaphot van een blob hebt gemaakt, wilt u eerst blob verwijst en vervolgens bellen de `ICloudBlob.CreateSnapshot` methode op is geïnstalleerd. Het volgende voorbeeld stelt eerst variabelen voor een account opslag, en maakt vervolgens een context opslag. Klik vervolgens in het voorbeeld haalt een blob verwijzing met de cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) en wordt uitgevoerd de methode [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) als een momentopname wilt maken.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "yourcontainername"
    $BlobName = "yourblobname"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

    #Create a snapshot of the blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

### <a name="how-to-list-a-blobs-snapshots"></a>Hoe u van een blob momentopnamen
U kunt zo veel momentopnamen als u voor een blob wilt maken. U kunt een lijst met de momentopnamen die is gekoppeld aan uw blob voor het bijhouden van uw huidige momentopnamen. Het volgende voorbeeld wordt gebruikt een vooraf gedefinieerde blob en de cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) u kunt de momentopnamen van die blob belt.  

    #Define the blob name.
    $BlobName = "yourblobname"

    #List the snapshots of a blob.
    Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }

### <a name="how-to-copy-a-snapshot-of-a-blob"></a>Een momentopname van een blob kopiëren
U kunt een momentopname van een blob herstellen van de momentopname kopiëren. Zie [een momentopname van een Blob maken](http://msdn.microsoft.com/library/azure/hh488361.aspx)voor gedetailleerde informatie en beperkingen. Het volgende voorbeeld stelt eerst variabelen voor een account opslag, en maakt vervolgens een context opslag. Klik vervolgens in het voorbeeld wordt de container en blob naam variabelen gedefinieerd. Het voorbeeld een blob verwijzing met de cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) zijn opgehaald en wordt uitgevoerd de methode [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) als een momentopname wilt maken. Klik in het voorbeeld de cmdlet [Start-AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) als u wilt kopiëren van de momentopname van een blob met behulp van het object ICloudBlob voor de bron-blob wordt uitgevoerd. Zorg ervoor dat de variabelen op basis van uw configuratie voordat u het voorbeeld. Houd er rekening mee dat het volgende voorbeeld wordt aangenomen dat de bron en bestemming containers en de bron-blob al aanwezig.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Define the variables.
    $SrcContainerName = "yoursourcecontainername"
    $DestContainerName = "yourdestcontainername"
    $SrcBlobName = "yourblobname"
    $DestBlobName = "CopyBlobName"

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

    #Create a snapshot of a blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

    #Copy the snapshot to another container.
    Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName

U hebt geleerd hoe voor het beheren van Azure BLOB's en momentopnamen met PowerShell Azure blob, gaat u naar de volgende sectie voor meer informatie over het beheren van tabellen, wachtrijen en bestanden.

## <a name="how-to-manage-azure-tables-and-table-entities"></a>Het beheren van Azure tabellen en tabel entiteiten
Azure tabel storage-service is een gegevensopslag NoSQL, waarin u kunt op te slaan en grote gegevenssets gestructureerde, niet-relationele query. De belangrijkste onderdelen van de service zijn tabellen, entiteiten en eigenschappen. Een tabel is een verzameling entiteiten. Een entiteit is een set met eigenschappen. Elke entiteit mag maximaal 252 eigenschappen, die alle naam-waardeparen zijn bevatten. In deze sectie wordt ervan uitgegaan dat u al bekend met de concepten Azure tabel Storage-Service bent. Zie [informatie over het gegevensmodel van de tabel-Service](http://msdn.microsoft.com/library/azure/dd179338.aspx) en [aan de slag met Azure-tabelopslag met .NET](storage-dotnet-how-to-use-tables.md)voor gedetailleerde informatie.

In de volgende subsecties leert u hoe u voor het beheren van Azure tabel storage-service via Azure PowerShell. De scenario's waarvoor opnemen **maken**, **verwijderen**en **ophalen van** **tabellen**, evenals **toevoegen**, **query's uitvoeren**en **tabel entiteiten verwijderen**.

### <a name="how-to-create-a-table"></a>Het maken van een tabel
Elke tabel moet zich bevinden in een Azure opslag-account. Het volgende voorbeeld ziet u hoe u een tabel maakt in Azure Storage. Het voorbeeld eerst verbinding een maken met Azure Storage gebruik van de context van de account opslag, waaronder de naam van het opslag-account en de access-sleutel. Vervolgens wordt de cmdlet [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) naar een tabel maken in Azure Storage.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Create a new table.
    $tabName = "yourtablename"
    New-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-retrieve-a-table"></a>Het ophalen van een tabel
U kunt de query en ophalen van een of alle tabellen in een opslag-account. Het volgende voorbeeld wordt het ophalen van een bepaalde tabel met de cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) .

    #Retrieve a table.
    $tabName = "yourtablename"
    Get-AzureStorageTable –Name $tabName –Context $Ctx

Als u de cmdlet Get-AzureStorageTable zonder parameters belt, wordt deze alle opslag tabellen voor een opslag-account.

### <a name="how-to-delete-a-table"></a>Hoe u een tabel verwijderen
Met behulp van de cmdlet [Verwijderen-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806393.aspx) , kunt u een tabel verwijderen uit een opslag-account.  

    #Delete a table.
    $tabName = "yourtablename"
    Remove-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-manage-table-entities"></a>Het beheren van de tabel entiteiten
Azure PowerShell biedt momenteel geen cmdlets uit om de tabel entiteiten rechtstreeks beheren. Als u wilt uitvoeren op tabel entiteiten, kunt u de klassen gegeven in de [Bibliotheek van Azure opslag Client voor .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).

#### <a name="how-to-add-table-entities"></a>Hoe u de tabel entiteiten toevoegen
Als u wilt een entiteit aan een tabel toevoegt, moet u eerst een object dat de entiteitseigenschappen van uw definieert maken. Een entiteit kan maximaal 255 eigenschappen, inclusief 3 Systeemeigenschappen hebben: **PartitionKey**, **RowKey**en **tijdstempel**. U bent verantwoordelijk voor het invoegen en de waarden van **PartitionKey** en **RowKey**bijwerken. De server worden beheerd door de waarde van een **tijdstempel**dat kan niet worden gewijzigd. Samen identificatie de **PartitionKey** en **RowKey** unieke elke entiteit binnen een tabel.

-   **PartitionKey**: bepaalt de partition die de entiteit worden opgeslagen in.
-   **RowKey**: deze identificeert op de entiteit binnen de partition.

U kunt maximaal 252 aangepaste eigenschappen voor een entiteit definiëren. Zie [informatie over het gegevensmodel van de tabel-Service](http://msdn.microsoft.com/library/azure/dd179338.aspx)voor meer informatie.

Het volgende voorbeeld ziet u hoe u entiteiten toevoegt aan een tabel. Het voorbeeld ziet hoe u het ophalen van de werknemerstabel en verschillende entiteiten erin toevoegen. Eerst verbinding deze een maken met Azure Storage gebruik van de context van de account opslag, waaronder de naam van het opslag-account en de access-sleutel. Vervolgens wordt de opgegeven tabel met de cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) opgehaald. Als de tabel niet bestaat, wordt de cmdlet [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) wordt gebruikt voor het maken van een tabel in Azure Storage. Klik vervolgens in het voorbeeld wordt een aangepaste functie toevoegen-entiteit entiteiten toevoegen aan de tabel door op te geven van elke entiteit partition en rijsleutel gedefinieerd. De functie toevoegen-entiteit oproepen de cmdlet [New-Object](http://technet.microsoft.com/library/hh849885.aspx) op de klas [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) maken een entiteitsobject. Het voorbeeld oproepen later de methode [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) op dit entiteitsobject toe te voegen aan een tabel.

    #Function Add-Entity: Adds an employee entity to a table.
    function Add-Entity() {
        [CmdletBinding()]
        param(
           $table,
           [String]$partitionKey,
           [String]$rowKey,
           [String]$name,
           [Int]$id
        )

      $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
      $entity.Properties.Add("Name", $name)
      $entity.Properties.Add("ID", $id)

      $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
    }

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Retrieve the table if it already exists.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

    #Create a new table if it does not exist.
    if ($table -eq $null)
    {
       $table = New-AzureStorageTable –Name $TableName -Context $Ctx
    }

    #Add multiple entities to a table.
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4

#### <a name="how-to-query-table-entities"></a>Hoe u query tabel entiteiten
Als u wilt een tabel een query uitvoert, gebruikt u de klasse [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) . Het volgende voorbeeld wordt ervan uitgegaan dat u het script in het opgegeven al hebt uitgevoerd om toe te voegen entiteiten sectie van deze handleiding. Het voorbeeld eerst verbinding een maken met Azure Storage gebruik van de context opslag, waaronder de naam van het opslag-account en de access-sleutel. Vervolgens wordt geprobeerd om op te halen de eerder gemaakte "Werknemers"-tabel met de cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) . De cmdlet [New-Object](http://technet.microsoft.com/library/hh849885.aspx) in de klas Microsoft.WindowsAzure.Storage.Table.TableQuery bellen, maakt een nieuwe queryobject. Het voorbeeld Hiermee wordt gezocht naar de entiteiten die een '-ID' kolom waarvan de waarde 1 als de opgegeven in een tekenreeks filter is hebben. Zie [query's uitvoeren tabellen en entiteiten](http://msdn.microsoft.com/library/azure/dd894031.aspx)voor gedetailleerde informatie. Wanneer u deze query uitvoert, wordt alle entiteiten die overeenkomen met de filtercriteria als resultaat gegeven.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Get a reference to a table.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx

    #Create a table query.
    $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

    #Define columns to select.
    $list = New-Object System.Collections.Generic.List[string]
    $list.Add("RowKey")
    $list.Add("ID")
    $list.Add("Name")

    #Set query details.
    $query.FilterString = "ID gt 0"
    $query.SelectColumns = $list
    $query.TakeCount = 20

    #Execute the query.
    $entities = $table.CloudTable.ExecuteQuery($query)

    #Display entity properties with the table format.
    $entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize

#### <a name="how-to-delete-table-entities"></a>Tabel entiteiten verwijderen
U kunt een entiteit met de toetsen partition en rij verwijderen. Het volgende voorbeeld wordt ervan uitgegaan dat u het script in het opgegeven al hebt uitgevoerd om toe te voegen entiteiten sectie van deze handleiding. Het voorbeeld eerst verbinding een maken met Azure Storage gebruik van de context opslag, waaronder de naam van het opslag-account en de access-sleutel. Vervolgens wordt geprobeerd om op te halen de eerder gemaakte "Werknemers"-tabel met de cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) . Als de tabel bestaat, wordt in het voorbeeld de methode [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) om op te halen een entiteit op basis van de sleutelwaarden partition en rij oproepen. Klik, geeft u de entiteit aan de methode [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) verwijderen.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the table.
    $TableName = "Employees"
    $table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

    #If the table exists, start deleting its entities.
    if ($table -ne $null) {
       #Together the PartitionKey and RowKey uniquely identify every  
       #entity within a table.
       $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
       $entity = $tableResult.Result
    if ($entity -ne $null)
    {
       #Delete the entity.
       $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
    }

## <a name="how-to-manage-azure-queues-and-queue-messages"></a>Het beheren van Azure wachtrijen en berichten in wachtrij plaatsen
Azure wachtrij opslag is een service voor het opslaan van grote aantallen van berichten die toegankelijk is vanuit een willekeurige plaats in de wereld via geverifieerde oproepen via HTTP of HTTPS. In deze sectie wordt ervan uitgegaan dat u al bekend met de concepten Azure wachtrij Storage-Service bent. Zie [aan de slag met Azure wachtrij opslagruimte met .NET](storage-dotnet-how-to-use-queues.md)voor gedetailleerde informatie.

In deze sectie wordt uitgelegd hoe u voor het beheren van Azure wachtrij storage-service via Azure PowerShell. De scenario's waarvoor opnemen **Invoegen** en **verwijderen van** berichten, evenals **maken**, **verwijderen**en **wachtrijen ophalen**.

### <a name="how-to-create-a-queue"></a>Het maken van een wachtrij
Het volgende voorbeeld eerst verbinding een maken met Azure Storage gebruik van de context van de account opslag, waaronder de naam van het opslag-account en de access-sleutel. Vervolgens wordt aangeroepen [Nieuw AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806382.aspx) cmdlet om een wachtrij met de naam 'wachtrijnaam' te maken.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $QueueName = "queuename"
    $Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx

Zie voor informatie over naamgevingsconventies voor Azure wachtrij-Service, [naamgevingsconventies wachtrijen en metagegevens](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-to-retrieve-a-queue"></a>Het ophalen van een wachtrij
U kunt de query en ophalen van een specifieke wachtrij of een lijst met alle wachtrijen in een opslag-account. Het volgende voorbeeld wordt het ophalen van een opgegeven wachtrij met de cmdlet [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) .

    #Retrieve a queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx

Als u de cmdlet [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) zonder parameters belt, ontvangt deze een lijst met alle wachtrijen.

### <a name="how-to-delete-a-queue"></a>Het verwijderen van een wachtrij
Als u wilt een wachtrij en alle bijbehorende berichten die dit verwijderen, belt u met de cmdlet verwijderen-AzureStorageQueue. Het volgende voorbeeld ziet u hoe u het verwijderen van een opgegeven wachtrij met de cmdlet verwijderen-AzureStorageQueue.

    #Delete a queue.
    $QueueName = "yourqueuename"
    Remove-AzureStorageQueue –Name $QueueName –Context $Ctx

#### <a name="how-to-insert-a-message-into-a-queue"></a>Het invoegen van een bericht in wachtrij
U een bericht in een bestaande wachtrij invoegt, moet u eerst een nieuw exemplaar van de klas [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) maken. Vervolgens de methode [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) aanroepen. Een CloudQueueMessage kan worden gemaakt van een tekenreeks (in de indeling UTF-8) of een matrix van bytes.

Het volgende voorbeeld wordt het bericht toevoegen aan een wachtrij. Het voorbeeld eerst verbinding een maken met Azure Storage gebruik van de context van de account opslag, waaronder de naam van het opslag-account en de access-sleutel. Vervolgens wordt de opgegeven wachtrij met de cmdlet [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) opgehaald. Als de wachtrij bestaat, wordt de cmdlet [Nieuw Object](http://technet.microsoft.com/library/hh849885.aspx) wordt gebruikt voor het maken van een exemplaar van de klasse [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) . Het voorbeeld oproepen later de methode [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) op dit berichtobject aan een wachtrij toe te voegen. Hier ziet u code die een wachtrij zijn opgehaald en Hiermee voegt u het bericht 'MessageInfo':

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    #If the queue exists, add a new message.
    if ($Queue -ne $null) {
       # Create a new message using a constructor of the CloudQueueMessage class.
       $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

       #Add a new message to the queue.
       $Queue.CloudQueue.AddMessage($QueueMessage)
    }


#### <a name="how-to-de-queue-at-the-next-message"></a>Hoe u uit de wachtrij om het volgende bericht
Uw code wachtrijen uit een bericht van een wachtrij in twee stappen. Als u de methode [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) belt, krijgt u het volgende bericht in een wachtrij. Er wordt een bericht dat uit **GetMessage** geretourneerd onzichtbare naar een andere code lezen van berichten van deze wachtrij. Als u klaar bent met het bericht verwijderen uit de wachtrij, moet u ook de methode [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) bellen. In dit proces van het verwijderen van een bericht zorgt ervoor dat als uw code mislukt verwerkingstijd van een bericht vanwege hardware of software, een ander exemplaar van uw code kunt het bericht verschijnt en probeer het opnieuw. Uw code oproepen **DeleteMessage** rechts nadat het bericht is verwerkt.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    $InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

    #Get the message object from the queue.
    $QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
    #Delete the message.
    $Queue.CloudQueue.DeleteMessage($QueueMessage)

## <a name="how-to-manage-azure-file-shares-and-files"></a>Het beheren van Azure bestandsshares en bestanden
Azure bestandsopslag biedt gedeelde opslag voor toepassingen via het standaard SMB-protocol. Microsoft Azure virtuele machines en cloudservices kunnen gegevens uit een bestand delen in toepassingsonderdelen via gekoppelde waarden voor aandelen en on-premises implementatie toepassingen hebben toegang tot gegevens uit een bestand in een delen via de API voor opslag van bestand of Azure PowerShell.

Zie [aan de slag met Azure bestandsopslag op Windows](storage-dotnet-how-to-use-files.md) en [Bestand Service REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx)voor meer informatie over Azure bestandsopslag.

## <a name="how-to-set-and-query-storage-analytics"></a>Het instellen en analyses van query-opslag
U kunt [Azure opslag Analytics](storage-analytics.md) gebruiken voor het verzamelen van de doelstellingen voor uw Azure opslag accounts en logboekgegevens over aanvragen die zijn verzonden naar uw account opslag. Metrische opslaggegevens kunt u de status van een opslag-account en opslag vastleggen om automatisch opsporen en oplossen van problemen met uw account opslag controleren.
Standaard is de metrische opslaggegevens niet ingeschakeld voor uw opslagservices. U kunt inschakelen via de Portal van Azure of Windows PowerShell of via een programma met behulp van de bibliotheek van de client opslag cmdlets voor controle. Opslag logboekregistratie aan de clientzijde gebeurt en kunt u details van de records voor geslaagde en mislukte aanvragen in uw account opslag. Deze logboeken kunt u de details van lezen, schrijven en bewerkingen op tabellen, wachtrijen, en BLOB's, evenals de redenen om mislukte aanvragen verwijderen.

Als u wilt weten hoe u inschakelen en weergeven van metrische opslaggegevens gegevens via PowerShell, raadpleegt u [het inschakelen van opslageenheden via PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Als u wilt weten hoe u inschakelen en via PowerShell opslag logboekregistratie-gegevens op te halen, raadpleegt u [het inschakelen van opslag logboekregistratie via PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) en [uw logboekgegevens opslag logboekregistratie zoeken](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
Zie voor gedetailleerde informatie over het gebruik van metrische opslaggegevens and opslag Logging opslag oplossen, [controle, diagnose, en probleemoplossing van Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a>Het beheren van gedeelde Access handtekening (SA's) en opgeslagen-beleid
Gedeelde toegang handtekeningen zijn een belangrijk onderdeel van het beveiligingsmodel voor een toepassing met Azure opslag. Zijn handig voor het leveren van beperkte machtigingen voor uw account opslagruimte aan clients die door de accountsleutel niet nodig hebt. Standaard kan alleen de eigenaar van het account opslag BLOB's, tabellen en wachtrijen binnen dat account openen. Als uw service of toepassing deze resources beschikbaar maken voor andere clients moet zonder het delen van uw toegangstoets, hebt u drie opties:

- De machtigingen van een container anonieme alleen toegang tot de container en de BLOB's instellen. Dit is niet toegestaan voor tabellen of wachtrijen.
- Gebruik een gedeelde access-handtekening of verleent toegangsrechten voor containers, BLOB's, wachtrijen en tabellen voor een bepaald tijdsinterval beperkt.
- Met een opgeslagen-beleid kunt u een extra niveau van de controle over gedeelde toegang handtekeningen voor een container of de BLOB's, voor een wachtrij of voor een tabel. Het beleid opgeslagen access kunt u de begintijd, verlooptijd of machtigingen voor een handtekening wijzigen of om in te trekken deze daarachter, is verleend.

Een handtekening gedeelde toegang kan worden in een van twee vormen:

- **Ad-hoc SA's**: wanneer u een ad-hoc SA's, de begintijd, verlooptijd, maken en machtigingen voor de SA's zijn opgegeven naar de SAS-URI. Dit soort SA's worden gemaakt in een container, blob, tabel of wachtrij en deze niet-revokable is.
- **SA's opgeslagen-beleid**: een opgeslagen-beleid is gedefinieerd op een resource container een container blob, tabel of wachtrij - en u deze kunt gebruiken voor het beheren van de beperkingen voor een of meer gedeelde toegang handtekeningen. Wanneer u een SA's aan een opgeslagen-beleid koppelen, neemt de SA's over de beperkingen - de begintijd, verlooptijd en machtigingen - gedefinieerd voor het opgeslagen-beleid. Dit soort SA's is revokable.

Zie [Gebruik van gedeelde Access handtekeningen (SA's)](storage-dotnet-shared-access-signature-part-1.md) en [anonieme leestoegang tot containers en BLOB's beheren](storage-manage-access-to-resources.md)voor meer informatie.

In de volgende secties leert u hoe u een gedeelde-handtekening token en opgeslagen toegangsbeleid voor Azure tabellen maakt. Azure PowerShell biedt vergelijkbare cmdlets voor containers, BLOB's en wachtrijen ook. Als wilt uitvoeren de scripts in deze sectie, downloadt u de [Azure PowerShell versie 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) of hoger.

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a>Het maken van een Access-handtekening gedeeld beleid gebaseerde token
Gebruik de cmdlet New-AzureStorageTableStoredAccessPolicy een nieuwe opgeslagen-beleid maken. Klik, belt u de cmdlet [New-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) als u wilt maken van een nieuw beleid gebaseerde gedeelde handtekening toegangstoken voor een Azure Storage-tabel.

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
    New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx

### <a name="how-to-create-an-ad-hoc-non-revokable-shared-access-signature-token"></a>Het maken van een ad-hoc (niet-revokable) gedeeld Access handtekening token
Gebruik de cmdlet [New-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) om het maken van een nieuwe ad-hoc (niet-revokable) gedeeld Access token handtekening voor een tabel Azure Storage:

    New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx

### <a name="how-to-create-a-stored-access-policy"></a>Het maken van een opgeslagen-beleid
Gebruik de cmdlet New-AzureStorageTableStoredAccessPolicy om het maken van nieuw toegangsbeleid opgeslagen voor een tabel Azure Storage:

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx

### <a name="how-to-update-a-stored-access-policy"></a>Het bijwerken van een opgeslagen-beleid
Gebruik de cmdlet Set-AzureStorageTableStoredAccessPolicy bij een bestaand opgeslagen access-beleid voor een tabel Azure Storage:

    Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx

### <a name="how-to-delete-a-stored-access-policy"></a>Een opgeslagen-beleid verwijderen
Gebruik de cmdlet verwijderen-AzureStorageTableStoredAccessPolicy om het verwijderen van een opgeslagen-beleid op een tabel Azure Storage:

    Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx


## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a>Het gebruik van Azure-opslag voor Amerikaanse overheid en Azure China
Een Azure omgeving is een onafhankelijke implementatie van Microsoft Azure, zoals [Azure overheid voor Amerikaanse overheid](https://azure.microsoft.com/features/gov/), [AzureCloud voor globale Azure](https://portal.azure.com)en [AzureChinaCloud voor Azure beheerd door 21Vianet in China](http://www.windowsazure.cn/). U kunt nieuwe Azure omgevingen voor Amerikaanse overheid en Azure China implementeren.

Als u wilt gebruiken Azure opslagruimte met AzureChinaCloud, moet u een opslag context die is gekoppeld aan AzureChinaCloud maken. Volg deze stappen om u aan de slag:

1.  Voer de cmdlet [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) als u wilt zien van de beschikbare Azure omgevingen:

    `Get-AzureEnvironment`

2.  Een China Azure-account toevoegen aan de Windows PowerShell:

    `Add-AzureAccount –Environment AzureChinaCloud`

3.  Maak een context opslagruimte voor een AzureChinaCloud-account:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud

Als u wilt gebruiken Azure opslagruimte met [Azure overheid](https://azure.microsoft.com/features/gov/), moet u definiëren van een nieuwe omgeving en maakt vervolgens een nieuwe context voor de opslag met deze omgeving:

1.  Voer de cmdlet [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) als u wilt zien van de beschikbare Azure omgevingen:

    `Get-AzureEnvironment`

2.  Een Azure ons Government-account toevoegen aan de Windows PowerShell:

    `Add-AzureAccount –Environment AzureUSGovernment`

3.  Maak een context opslagruimte voor een AzureUSGovernment-account:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment

Zie voor meer informatie:

- [Handleiding voor ontwikkelaars van Microsoft Azure overheid](../azure-government-developer-guide.md).
- [Overzicht van de verschillen bij het maken van een toepassing in China-Service](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Volgende stappen
In deze handleiding, hebt u geleerd hoe Azure-opslag met Azure PowerShell beheren. Hier volgen enkele verwante artikelen en bronnen voor meer informatie over deze.

- [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)
- [Azure opslag PowerShell-Cmdlets](http://msdn.microsoft.com/library/azure/dn806401.aspx)
- [Naslaggids voor Windows PowerShell](https://msdn.microsoft.com/library/ms714469.aspx)

[Image1]: ./media/storage-powershell-guide-full/Subscription_currentportal.png
[Image2]: ./media/storage-powershell-guide-full/Subscription_Previewportal.png
[Image3]: ./media/storage-powershell-guide-full/Blobdownload.png


[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
