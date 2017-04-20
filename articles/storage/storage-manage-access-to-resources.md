<properties
    pageTitle="Anonieme alleen de toegang tot containers en BLOB's beheren | Microsoft Azure"
    description="Leer hoe u containers en BLOB's beschikbaar zijn voor anonieme toegang en hoe u deze via een programma te openen."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Anonieme alleen de toegang tot containers en BLOB's beheren

## <a name="overview"></a>Overzicht

Standaard kan alleen de eigenaar van het account opslag toegang tot opslagbronnen binnen dat account. Voor alleen-blobopslag, kunt u de machtigingen van een container anonieme alleen toegang tot de container en de BLOB's, instellen, zodat u toegang tot deze resources verlenen kunt zonder het delen van uw accountsleutel.

Anonieme toegang meest geschikt is voor scenario's waar u bepaalde BLOB's altijd beschikbaar zijn voor anonieme leestoegang. U kunt een gedeelde access-handtekening waarmee u naar de gemachtigde beperkte toegang tot een andere machtigingen en na verloop van een bepaald tijdsinterval maken fijnmaziger wilt instellen. Zie voor meer informatie over het maken van gedeelde toegang handtekeningen [Gebruiken gedeelde Access handtekeningen (SA's)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Anonieme gebruikersmachtigingen verlenen voor containers en BLOB 's

Standaard mag een container en eventuele BLOB's erin alleen door de eigenaar van het account opslag worden geopend. Als u wilt anonieme gebruikers leesmachtigingen voor een container en de BLOB's geven, kunt u de machtigingen van de container om toegang te krijgen openbare instellen. Anonieme gebruikers kunnen BLOB's binnen een openbaar container lezen zonder verificatie van het verzoek.

Containers bevatten de volgende opties voor het beheren van container:

- **Volledig openbare leestoegang:** Container en blob gegevens kunnen worden gelezen via anonieme mailaanvraag. Clients BLOB's in de container via anonieme aanvraag kunnen opsommen, maar kunnen niet opsommen containers in het opslag-account.

- **Openbare toegang voor BLOB's alleen lezen:** BLOB-gegevens in deze container kunnen worden gelezen via anonieme aanvraag, maar de gegevens van de container is niet beschikbaar. Clients kunnen niet opsommen BLOB's in de container via anonieme mailaanvraag.

- **Geen openbare leestoegang:** Container en blob gegevens kunnen worden gelezen door alleen de accounteigenaar van het.

U kunt de container machtigingen instellen in de volgende manieren:

- Vanuit de [Azure-Portal](https://portal.azure.com).
- Via een programma, via de bibliotheek van de client opslag of de REST API.
- Via PowerShell. Zie voor meer informatie over het instellen van machtigingen van de container van Azure PowerShell, [Azure PowerShell gebruiken met Azure opslagmedia](storage-powershell-guide-full.md#how-to-manage-azure-blobs).

### <a name="setting-container-permissions-from-the-azure-portal"></a>Container machtigingen instellen vanaf de Portal van Azure

Container machtigingen van de [Portal van Azure](https://portal.azure.com)stelt als volgt te werk:

1. Ga naar het dashboard voor uw account opslag.
2. Selecteer de containernaam van de in de lijst. De BLOB's in de door u gekozen container beschrijft te klikken op de naam
3. Selecteer **-beleid** op de werkbalk.
4. In het veld **type toegang** selecteert u het gewenste niveau van machtigingen, zoals in de onderstaande schermafbeelding.

    ![Container metagegevens dialoogvenster bewerken](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="setting-container-permissions-programmatically-using-net"></a>Container machtigingen programmacode met .NET instellen

Machtigingen voor een container met de bibliotheek van de client .NET stelt bestaande machtigingen van de container eerst worden opgehaald door de methode **GetPermissions** roepen. Stel de eigenschap **PublicAccess** voor het object **BlobContainerPermissions** die wordt geretourneerd door de methode **GetPermissions** . Ten slotte, belt u de methode **SetPermissions** met de bijgewerkte machtigingen.

Het volgende voorbeeld wordt de machtigingen van de container naar volledige openbare leestoegang. Voor het instellen van machtigingen om Stel openbare leestoegang voor BLOB's alleen de eigenschap **PublicAccess** op **BlobContainerPublicAccessType.Blob**. Als u wilt verwijderen alle machtigingen voor anonieme gebruikers, door de eigenschap in te stellen op **BlobContainerPublicAccessType.Off**.

    public static void SetPublicContainerPermissions(CloudBlobContainer container)
    {
        BlobContainerPermissions permissions = container.GetPermissions();
        permissions.PublicAccess = BlobContainerPublicAccessType.Container;
        container.SetPermissions(permissions);
    }

## <a name="access-containers-and-blobs-anonymously"></a>Anoniem containers en BLOB's openen

Een client die toegang heeft tot anoniem containers en BLOB's kan constructors waarvoor geen referenties gebruiken. De volgende voorbeelden wordt een aantal verschillende manieren om te verwijzen anoniem naar Blob serviceresources.

### <a name="create-an-anonymous-client-object"></a>Een anonieme clientobject maken

U kunt een nieuwe service client-object voor anonieme toegang maken met behulp van het eindpunt van de service Blob voor het account. U moet de naam van een container in dat account die beschikbaar zijn voor anonieme toegang is echter ook kennen.

    public static void CreateAnonymousBlobClient()
    {
        // Create the client object using the Blob service endpoint.
        CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

        // Read the container's properties. Note this is only possible when the container supports full public read access.
        container.FetchAttributes();
        Console.WriteLine(container.Properties.LastModified);
        Console.WriteLine(container.Properties.ETag);
    }

### <a name="reference-a-container-anonymously"></a>Een container anoniem verwijst naar

Als u de URL voor een container die anoniem beschikbaar is, kunt u deze naar de container rechtstreeks verwijzen.

    public static void ListBlobsAnonymously()
    {
        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

        // List blobs in the container.
        foreach (IListBlobItem blobItem in container.ListBlobs())
        {
            Console.WriteLine(blobItem.Uri);
        }
    }


### <a name="reference-a-blob-anonymously"></a>Een blob anoniem verwijst naar

Als u de URL voor een blob die beschikbaar is voor anonieme toegang hebt, kunt u verwijzen naar de blob rechtstreeks via deze URL:

    public static void DownloadBlobAnonymously()
    {
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
        blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
    }

## <a name="features-available-to-anonymous-users"></a>Functies die beschikbaar zijn voor anonieme gebruikers

De volgende tabel ziet u welke bewerkingen mag worden aangeroepen door anonieme gebruikers wanneer een container ACL is ingesteld op openbare toegang toestaan.

| REST-bewerking                                         | Machtiging met volledige openbare leestoegang | Machtiging met openbare leestoegang voor alleen BLOB 's |
|--------------------------------------------------------|-----------------------------------------|---------------------------------------------------|
| Lijst met Containers                                        | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Container maken                                       | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Containereigenschappen van de ophalen                               | Alle                                     | Alleen de eigenaar                                        |
| Container metagegevens ophalen                                 | Alle                                     | Alleen de eigenaar                                        |
| Metagegevens van de Container instellen                                 | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Container ACL ophalen                                      | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Container ACL instellen                                      | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Container verwijderen                                       | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Lijst BLOB 's                                             | Alle                                     | Alleen de eigenaar                                        |
| Blob plaatsen                                               | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Blob ophalen                                               | Alle                                     | Alle                                               |
| Eigenschappen van Blob ophalen                                    | Alle                                     | Alle                                               |
| Blob-eigenschappen instellen                                    | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Blob metagegevens ophalen                                      | Alle                                     | Alle                                               |
| Blob metagegevens instellen                                      | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Blok plaatsen                                              | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Lijst met geblokkeerde (alleen vastgelegde geblokkeerd) ophalen                 | Alle                                     | Alle                                               |
| Lijst met geblokkeerde (alleen doorgevoerde blokken of alle blokken) ophalen | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Lijst met geblokkeerde plaatsen                                         | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Blob verwijderen                                            | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Blob kopiëren                                              | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Momentopname Blob                                          | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Lease Blob                                             | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Pagina plaatsen                                               | Alleen de eigenaar                              | Alleen de eigenaar                                        |
| Get-paginabereik                                        | Alle                                     | Alle                                                  |
| Blob toevoegen                                            | Alleen de eigenaar                              | Alleen de eigenaar                                                  |


## <a name="see-also"></a>Zie ook

- [Verificatie voor de van Azure-opslagservices](https://msdn.microsoft.com/library/azure/dd179428.aspx)
- [Gedeelde toegang handtekeningen (SA's) gebruiken](storage-dotnet-shared-access-signature-part-1.md)
- [Delegeren van Access met een gedeelde Access-handtekening](https://msdn.microsoft.com/library/azure/ee395415.aspx)
