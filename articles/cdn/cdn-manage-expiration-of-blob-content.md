<properties
 pageTitle="Verlopen van inhoud van de opslag van Azure blob in Azure CDN beheerd | Microsoft Azure"
 description="Meer informatie over de opties voor het instellen van time to live voor BLOB's in caching van Azure CDN."
 services="cdn"
 documentationCenter=""
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="multiple"
 ms.topic="article"
 ms.date="09/15/2016"
 ms.author="casoper"/>


# <a name="manage-expiration-of-azure-storage-blob-content-in-azure-cdn"></a>Verlopen van inhoud van de opslag van Azure blob in Azure CDN beheerd

> [AZURE.SELECTOR]
- [Azure-Apps/Cloud webservices, ASP.NET of IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure blob Storage-service](cdn-manage-expiration-of-blob-content.md)

De [blob-service](../storage/storage-introduction.md#blob-storage) in [Azure Storage](../storage/storage-introduction.md) is een van de verschillende op basis van Azure oorsprong geïntegreerd met Azure CDN.  Openbaar blob inhoud kan worden opgeslagen in Azure CDN totdat de time to live (TTL) is verstreken.  De TTL wordt bepaald door de [ *Cache-Control* kop](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in het HTTP-antwoord van Azure-opslag.

>[AZURE.TIP] U kunt geen TTL instellen op een blob.  In dit geval de Azure CDN een standaard-TTL van zeven dagen automatisch toegepast.
>
>Zie voor meer informatie over de werking van Azure CDN om toegang tot BLOB's en andere bestanden sneller af te [Azure CDN overzicht](./cdn-overview.md).
>
>Zie voor meer informatie over de opslag van Azure blob-service, [Blob Service concepten](https://msdn.microsoft.com/library/dd179376.aspx). 

Deze zelfstudie toont verschillende manieren die u de TTL-waarde op een blob in Azure opslag instellen kunt.  

## <a name="azure-powershell"></a>Azure PowerShell

[Azure PowerShell](../powershell-install-configure.md) is een van de snelste, krachtigste manieren om uw Azure services te beheren.  Gebruik de `Get-AzureStorageBlob` cmdlet om een verwijzing naar de blob, stel de `.ICloudBlob.Properties.CacheControl` eigenschap. 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

>[AZURE.TIP] U kunt ook PowerShell gebruiken voor het [beheren van uw CDN profielen en de eindpunten](./cdn-manage-powershell.md).

## <a name="azure-storage-client-library-for-net"></a>Azure opslag clientbibliotheek voor .NET

Als u wilt instellen van een blob TTL met .NET, door de [Azure opslag clientbibliotheek voor .NET](../storage/storage-dotnet-how-to-use-blobs.md) te gebruiken voor het instellen van de eigenschap [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) .

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
        
        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

>[AZURE.TIP] Er zijn veel meer voorbeelden van .NET-code beschikbaar in de [Voorbeelden van Azure Blob Storage voor .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).

## <a name="other-methods"></a>Andere methoden

- [Azure opdrachtregel-Interface](../xplat-cli-install.md)

    Wanneer de blob te uploaden, de instellen *cacheControl* eigenschap met de `-p` schakelen.  In het volgende voorbeeld wordt de TTL-waarde op 1 uur (3600 seconden).

    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```

- [Azure opslagservices REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)

    Expliciet stelt u de eigenschap *x-ms-blob-cache-besturingselementen* op een verzoek om te [Zetten Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Bloklijst plaatsen](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)of [Blob-eigenschappen instellen](https://msdn.microsoft.com/library/azure/ee691966.aspx) .

- Hulpmiddelen voor projectbeheer van derden opslag

    Sommige beheerprogramma's van derden Azure opslag kunnen u de eigenschap *CacheControl* op BLOB's instellen. 

## <a name="testing-the-cache-control-header"></a>De koptekst *Cachebeheer* testen

U kunt eenvoudig controleren of de TTL-waarde van uw BLOB's.  Met van uw browser [speciale tools voor ontwikkelaars](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test u dat uw blob tekenarcering *Cachebeheer* is inclusief.  U kunt ook een hulpmiddel zoals **wget**, [Postman](https://www.getpostman.com/)of [Fiddler](http://www.telerik.com/fiddler) gebruiken om te bekijken van de koppen antwoord.

## <a name="next-steps"></a>Volgende stappen

- [Lees meer over de kop van de *Cache-besturingselementen*](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
- [Informatie over het beheren van de vervaldatum van een Cloudservice in Azure CDN](./cdn-manage-expiration-of-cloud-service-content.md)

