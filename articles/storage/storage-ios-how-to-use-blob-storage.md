<properties
    pageTitle="Het gebruik van Azure-blobopslag van iOS | Microsoft Azure"
    description="Ongestructureerde gegevens opslaan in de cloud met Azure-blobopslag (object opslag)."
    services="storage"
    documentationCenter="ios"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-ios"></a>Het gebruik van iOS-blobopslag

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

In dit artikel wordt uitgelegd hoe u veelvoorkomende scenario's met Microsoft Azure-blobopslag uitvoeren. In de voorbeelden in de doel-C zijn geschreven en gebruikt u de [Bibliotheek van Azure opslag Client voor iOS](https://github.com/Azure/azure-storage-ios). De scenario's waarvoor zijn **geüpload**, **vermelding**, **downloaden**en BLOB's **verwijderen** . Zie voor meer informatie over BLOB's, de sectie van de [Volgende stappen](#next-steps) . U kunt ook de [steekproef app](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) snel overzicht van het gebruik van Azure-opslag in een iOS-toepassing downloaden.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>De opslag van Azure iOS-bibliotheek in uw toepassing importeren

U kunt de opslag van Azure iOS-bibliotheek importeren in uw toepassing met behulp van de [Azure opslag CocoaPod](https://cocoapods.org/pods/AZSClient) of door de **Framework** -bestand te importeren.

## <a name="cocoapod"></a>CocoaPod

1. Als u dit nog niet hebt gedaan, [CocoaPods installeren](https://guides.cocoapods.org/using/getting-started.html#toc_3) op uw computer door een terminal-venster te openen en de volgende opdracht

        sudo gem install cocoapods

2. Vervolgens gaat u naar de projectmap (de map waarop uw `.xcodeproj` bestand), maak een nieuw bestand genoemd `Podfile`(zonder extensie). Voeg de volgende naar `Podfile` en opslaan

        pod 'AZSClient'

3. Klik in het venster terminal Ga naar de projectmap en voer de volgende opdracht

        pod install

4. Als uw `.xcodeproj` is geopend in Xcode, sluit u deze. Open het nieuwe project-bestand dat in het telefoonboek van uw project de `.xcworkspace` extensie. Dit is het bestand werkt u uit voor nu aan.

## <a name="framework"></a>Framework
Om te kunnen gebruiken de opslag van Azure iOS-bibliotheek, moet u eerst het framework-bestand maken.

1. Eerst downloaden of het [azure-opslag-ios cessies‑retrocessies](https://github.com/azure/azure-storage-ios)klonen.

2. Ga naar *azure-opslag-ios* -> *bibliotheek* -> *Azure opslag clientbibliotheek*en open `AZSClient.xcodeproj` in Xcode.

3. Wijzig de actieve schema vanuit 'Azure opslag clientbibliotheek' naar "Framework" bij de linkerbovenhoek van Xcode.

4. Maken van het project (⌘ + B). Hiermee maakt u een `AZSClient.framework` bestand op uw bureaublad.

Vervolgens kunt u het bestand framework importeren in uw toepassing als volgt:

1. Maak een nieuw project of open het bestaande project Xcode.

2. Klik op uw project in het linkernavigatievenster en klik op het tabblad *Algemeen* boven aan het project-editor.

3. Klik onder de sectie *gekoppelde kaders en bibliotheken* op de knop toevoegen (+).

4. Klik op *andere... toevoegen*. Ga naar en toevoegen de `AZSClient.framework` bestand dat u zojuist hebt gemaakt.

5. Klik onder de sectie *gekoppelde kaders en bibliotheken* klikt u nogmaals op de knop toevoegen (+).

6. Zoekt in de lijst met bibliotheken al opgegeven `libxml2.2.dylib` en voeg deze toe aan uw project.

7. Klik op het tabblad *Instellingen maken* boven aan het project-editor.

8. *Framework zoekpaden* Dubbelklik onder de sectie *Zoekpaden* en toevoegen van het pad naar uw `AZSClient.framework` bestand.

## <a name="import-statement"></a>Instructie importeren
U moet de volgende importeren-instructie in het bestand waarin u wilt de API Azure opslag roepen opnemen.

    // Include the following import statement to use blob APIs.
    #import <AZSClient/AZSClient.h>

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Asynchrone bewerkingen
> [AZURE.NOTE] Alle methoden die een verzoek om ten opzichte van de service uitvoeren zijn asynchrone bewerkingen. In de voorbeelden van de code vindt u dat deze methoden een handler voltooid hebben. Code binnen de voltooiing-handler uitgevoerd **nadat** die de aanvraag is voltooid. Code na de voltooiing-handler wordt uitgevoerd **terwijl** het verzoek wordt gesteld.

## <a name="create-a-container"></a>Een container maken
Elke blob in Azure opslag moet zich bevinden in een container. Het volgende voorbeeld ziet u hoe u maakt een zogenaamd, *newcontainer*, in uw account opslag als dit nog niet bestaat. Bij het kiezen van een naam voor de container, worden Houd ook rekening met de hierboven genoemde regels voor naamgeving.

    -(void)createContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

      // Create container in your Storage account if the container doesn't already exist
      [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
         if (error){
             NSLog(@"Error in creating container.");
         }
      }];
    }

U kunt bevestigen dat dit werkt door te kijken naar de [Microsoft Azure opslag Explorer](http://storageexplorer.com) en bevestigt dat *newcontainer* is in de lijst met containers voor uw account opslag.

## <a name="set-container-permissions"></a>Container machtigingen instellen
Een container machtigingen zijn geconfigureerd voor **privé** toegang al dan niet standaard. Containers hebben echter een aantal verschillende opties voor access container:

- **Privé**: Container en blob gegevens kunnen worden gelezen door alleen de accounteigenaar van het.

- **BLOB**: Blob-gegevens in deze container kunnen worden gelezen via anonieme aanvraag, maar de gegevens van de container is niet beschikbaar. Clients kunnen niet opsommen BLOB's in de container via anonieme mailaanvraag.

- **Container**: Container en blob gegevens kunnen worden gelezen via anonieme mailaanvraag. Clients BLOB's in de container via anonieme aanvraag kunnen opsommen, maar kunnen niet opsommen containers in het opslag-account.

Het volgende voorbeeld ziet u hoe u een container maken met de **Container** toegangsmachtigingen waarmee openbare, alleen-lezen access voor alle gebruikers op Internet:

    -(void)createContainerWithPublicAccess{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create container in your Storage account if the container doesn't already exist
        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
            if (error){
                NSLog(@"Error in creating container.");
            }
        }];
    }

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container
Zoals vermeld in de sectie [Blob service concepten](#blob-service-concepts) , Blob Storage biedt drie verschillende soorten BLOB's: blokkeren BLOB's en pagina's BLOB's BLOB's toevoegen. Op dit moment, biedt blok BLOB's alleen ondersteuning voor de opslag van Azure iOS-bibliotheek. In de meeste gevallen is blok blob het aanbevolen dat u wilt gebruiken.

Het volgende voorbeeld ziet u hoe u een blok blob uit een NSString uploaden. Als er al een blob met dezelfde naam in deze container bestaat, wordt de inhoud van deze blob overschreven.

    -(void)uploadBlobToContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
         {
             if (error){
                 NSLog(@"Error in creating container.");
             }
             else{
                 // Create a local blob object
                 AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                 // Upload blob to Storage
                 [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                     if (error){
                         NSLog(@"Error in creating blob.");
                     }
                 }];
             }
         }];
    }

U kunt bevestigen dat dit werkt door te kijken naar de [Microsoft Azure opslag Explorer](http://storageexplorer.com) en bevestigt dat de container, *containerpublic*, de blob, *sampleblob*bevat. In dit voorbeeld wordt een openbare container gebruikt, zodat u ook controleren kunt of dit heeft gewerkt door te gaan naar de BLOB URI's:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Naast het uploaden van een blok blob uit een NSString, worden dezelfde methoden voor NSData, NSInputStream of een lokaal bestand bestaan.

## <a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container
Het volgende voorbeeld ziet u hoe u alle BLOB's in een container. Wanneer u deze bewerking uitvoert, worden Houd ook rekening met de volgende parameters:     

- **continuationToken** - het token voortgezet aangeeft waar de bewerking vermelding moet beginnen. Als er geen token wordt geleverd, deze wordt een lijst met BLOB's vanaf het begin. Een willekeurig aantal BLOB's kan worden weergegeven, van nul tot een set maximum. Zelfs als deze methode geeft als een getal zonder resultaten resultaat als `results.continuationToken` is niet nul is, kunnen er meer BLOB's op de service niet goed doornemen.
- **voorvoegsel** - kunt u het voorvoegsel gebruiken voor de aanbieding blob opgeven. Alleen de BLOB's die met dit voorvoegsel beginnen worden, vermeld.
- **useFlatBlobListing** - zoals vermeld in de sectie [Naming en naar verwijst containers en BLOB's](#naming-and-referencing-containers-and-blobs) , hoewel de Blob-service een kleurenschema plat opslag is, kunt u een virtuele hiërarchie maken door de naam van BLOB's met informatie over het pad. Niet-platte vermelding is echter momenteel niet ondersteund; Dit is binnenkort beschikbaar. Nu u deze waarde moet`YES`
- **blobListingDetails** - kunt u opgeven welke artikelen waarop de vermelding van BLOB 's
    - `AZSBlobListingDetailsNone`: Alleen vastgelegde BLOB's bevatten en geen blob metagegevens retourneren.
    - `AZSBlobListingDetailsSnapshots`: Lijst vastgelegde BLOB's en blob momentopnamen.
    - `AZSBlobListingDetailsMetadata`: Ophalen blob metagegevens voor elke blob als resultaat gegeven in de aanbieding.
    - `AZSBlobListingDetailsUncommittedBlobs`: Een lijst met vastgelegde en niet-doorgevoerde BLOB's.
    - `AZSBlobListingDetailsCopy`: Eigenschappen kopiëren in de vermelding opnemen.
    - `AZSBlobListingDetailsAll`: Een lijst met alle beschikbare vastgelegde BLOB's, niet-doorgevoerde BLOB's en momentopnamen en status van alle metagegevens en kopiëren voor deze BLOB's te retourneren.
- **maxResults** - het maximum aantal weer te geven voor deze bewerking resultaten. Gebruik -1 niet een limiet instellen.
- **completionHandler** - de blokkering van programmacode wordt uitgevoerd met de resultaten van de vermelding-bewerking.

In dit voorbeeld een helpmethode wordt gebruikt voor recursief gesprek de lijst BLOB's methode telkens wanneer een token voortgezet wordt geretourneerd.

    -(void)listBlobsInContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        //List all blobs in container
        [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
            if (error != nil){
                NSLog(@"Error in creating container.");
            }
        }];
    }

    //List blobs helper method
    -(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
    {
        [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
            if (error)
            {
                completionHandler(error);
            }
            else
            {
                for (int i = 0; i < results.blobs.count; i++) {
                    NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
                }
                if (results.continuationToken)
                {
                    [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
                }
                else
                {
                    completionHandler(nil);
                }
            }
        }];
    }


## <a name="download-a-blob"></a>Een blob downloaden

Het volgende voorbeeld wordt getoond hoe een blob downloaden op object NSString.

    -(void)downloadBlobToString{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

        // Download blob
        [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
            if (error) {
                NSLog(@"Error in downloading blob");
            }
            else{
                NSLog(@"%@",text);
            }
        }];
    }

## <a name="delete-a-blob"></a>Een blob verwijderen

Het volgende voorbeeld ziet u hoe u een blob verwijdert.

    -(void)deleteBlob{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

        // Delete blob
        [blockBlob deleteWithCompletionHandler:^(NSError *error) {
            if (error) {
                NSLog(@"Error in deleting blob.");
            }
        }];
    }

## <a name="delete-a-blob-container"></a>Een container blob verwijderen

Het volgende voorbeeld ziet u hoe u een container verwijdert.

    -(void)deleteContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

      // Delete container
      [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
         if(error){
             NSLog(@"Error in deleting container");
         }
      }];
    }

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe Blob Storage gebruik van iOS, volgt u deze koppelingen voor meer informatie over de iOS-bibliotheek en de Storage-service.

- [Azure opslag Client-bibliotheek voor iOS](https://github.com/azure/azure-storage-ios)
- [Azure opslag iOS naslagmateriaal](http://azure.github.io/azure-storage-ios/)
- [Azure opslagservices REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure opslag teamblog](http://blogs.msdn.com/b/windowsazurestorage)

Je mag rustig posten naar onze [Azure MSDN-forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) of [Stapel overloop](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files)als u vragen over deze bibliotheek hebt.
Als u suggesties van de functie voor Azure opslagruimte hebt, post u [Azure opslag Feedback](https://feedback.azure.com/forums/217298-storage/).
