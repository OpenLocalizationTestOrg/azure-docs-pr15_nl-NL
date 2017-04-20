<properties 
    pageTitle="Bestanden uploaden naar een Media Services-account met behulp van .NET | Microsoft Azure" 
    description="Leer hoe u media-inhoud van Media Services door te maken en uploaden van activa." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="juliako"/>



# <a name="upload-files-into-a-media-services-account-using-net"></a>Bestanden uploaden naar een Media Services-account met behulp van .NET

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [REST](media-services-rest-upload-files.md)
 - [Portal](media-services-portal-upload-files.md)

In Media Services u uploaden (of nemen) de digitale bestanden naar een actief. De **activa** entity kunt video, audio, afbeeldingen, miniaturen verzamelingen, tekst nummers en bevatten ondertiteling bestanden (en de metagegevens over deze bestanden.)  Nadat de bestanden die zijn geüpload, wordt uw inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming.

De bestanden in de activa worden **Activa bestanden**genoemd. Het exemplaar **AssetFile** en het werkelijke mediabestand zijn in twee afzonderlijke objecten. Het exemplaar AssetFile bevat metagegevens over het mediabestand, terwijl het mediabestand de werkelijke media-inhoud bevat.

>[AZURE.NOTE]De volgende overwegingen bij het kiezen van een bestandsnaam van de activa van toepassing:
>
>- Media Services gebruikt de waarde van de eigenschap IAssetFile.Name bij het maken van URL's voor de streaming inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Daarom is percentagecodering niet toegestaan. De waarde van de eigenschap **Name** geen van de volgende [tekens percentage-codering-gereserveerd](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Bovendien kunnen alleen er een '.' voor de bestandsnaamextensie.
>
>- De lengte van de naam mag niet langer zijn dan 260 tekens.

Wanneer u activa maakt, kunt u de volgende versleutelingsopties opgeven. 

- **Geen** : geen versleuteling wordt gebruikt. Dit is de standaardwaarde. Houd er rekening mee dat wanneer u deze optie gebruikt de inhoud niet is beveiligd tijdens overdracht of op rest in opslag.
Als u van plan bent om uit te voeren een MP4 geleidelijke downloaden, gebruikt u deze optie gebruikt. 
- **CommonEncryption** - Gebruik deze optie als u bezig bent met het uploaden van inhoud die al is versleuteld en beveiligd met algemene versleuteling of PlayReady DRM (bijvoorbeeld vloeiende Streaming beveiligd met DRM PlayReady).
- **EnvelopeEncrypted** – Gebruik deze optie als u bezig bent met het uploaden van HLS versleuteld met AES. Houd er rekening mee dat de bestanden moeten codering en versleuteld door Manager transformeren.
- **StorageEncrypted** - versleutelt uw wissen met behoud van lokaal bit AES-256 versleuteling en klik vervolgens op deze met Azure Storage waar deze zijn opgeslagen versleuteld uploads in rust. Activa die zijn beveiligd met opslag versleuteling zijn automatisch niet versleuteld en geplaatst in een versleutelde bestandssysteem vóór codering en desgewenst opnieuw worden versleuteld voordat u het uploadt weer als een nieuwe uitvoer actief. De primaire use-case voor opslag-versleuteling is wanneer u wilt beveiligen van uw hoge kwaliteit voor de invoer mediabestanden met codering in rust op schijf.

    Media Services biedt versleuteling op schijf opslagruimte voor uw activa, niet over-het-kabel zoals Digital Rights Manager (DRM).

    Als uw activa versleuteld opslag is, moet u activa bezorging beleid configureren. Zie [configureren activa bezorging beleid](media-services-dotnet-configure-asset-delivery-policy.md)voor meer informatie.

Als u voor uw activa worden versleuteld met een optie voor **CommonEncrypted** of een optie **EnvelopeEncypted** opgeeft, moet u uw activa koppelen aan een **ContentKey**. Lees [hoe u het maken van een ContentKey](media-services-dotnet-create-contentkey.md)voor meer informatie. 

Als u voor uw activa worden versleuteld met een optie **StorageEncrypted** opgeeft, wordt de Media Services SDK voor .NET een **StorateEncrypted** **ContentKey** voor uw activa maken.


In dit onderwerp wordt uitgelegd hoe u Media Services .NET SDK, evenals Media Services .NET SDK uitbreidingen voor het uploaden van bestanden in een activum Media Services.

 
## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Een bestand met Media Services .NET SDK uploaden 

Het voorbeeld hieronder wordt gebruikt .NET SDK de volgende taken uitvoeren: 

- Hiermee maakt u een lege actief.
- Hiermee maakt u een exemplaar van AssetFile die we wilt koppelen aan het activum.
- Hiermee maakt u een exemplaar van AccessPolicy die welke machtigingen en de duur van access naar het activum bepaalt.
- Hiermee maakt u een exemplaar Locator die toegang tot de activa biedt.
- Een enkel mediabestand wordt geüpload naar Media Services. 

        
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions); 

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            var policy = _context.AccessPolicies.Create(
                                    assetName,
                                    TimeSpan.FromDays(30),
                                    AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            locator.Delete();
            policy.Delete();

            return inputAsset;
        }

##<a name="upload-multiple-files-with-media-services-net-sdk"></a>Meerdere bestanden met Media Services .NET SDK uploaden 

De volgende code ziet hoe u een actief maken en meerdere bestanden uploaden.

De code, gebeurt het volgende:
    
-   Hiermee maakt u een lege actief met behulp van de methode CreateEmptyAsset is gedefinieerd in de vorige stap.
    
-   Hiermee maakt u een exemplaar van **AccessPolicy** die welke machtigingen en de duur van access naar het activum bepaalt.
    
-   Hiermee maakt u een exemplaar **Locator** die toegang tot de activa biedt.
    
-   Hiermee maakt u een exemplaar **BlobTransferClient** . In dit type vertegenwoordigt een client die wordt toegepast op de Azure BLOB's. In dit voorbeeld gebruiken we de client naar de upload-voortgang bewaken. 
    
-   Bestanden in de opgegeven map wordt middels en maakt een instantie **AssetFile** voor elk bestand.
    
-   De bestanden uploadt naar Media-Services met behulp van de methode **UploadAsync** . 
    
>[AZURE.NOTE] Gebruik de methode UploadAsync om ervoor te zorgen dat niet worden geblokkeerd door de oproepen en de bestanden worden geüpload parallel.
    
    
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }
    
    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



Wanneer u een groot aantal activa uploadt, moet u rekening houden met het volgende.

- Maak een nieuw object **CloudMediaContext** per thread. De **CloudMediaContext** -klasse is niet thread-veilig.
 
- Vergroten NumberOfConcurrentTransfers uit de standaardwaarde van 2 naar een hogere waarde is zoals 5. Deze instelling is van invloed op alle exemplaren van **CloudMediaContext**. 
 
- Houd ParallelTransferThreadCount de standaardwaarde van 10.
 
##<a id="ingest_in_bulk"></a>Ingesting activa bulksgewijs met Media Services .NET SDK 

Grote activa bestanden uploaden, kan een vertraging veroorzaken tijdens het maken van de activa zijn. Ingesting activa in bulksgewijs of 'Bulksgewijs Ingesting', heeft betrekking op ontkoppeling activa maken van het uploadproces. Als u wilt gebruiken een groot aantal ingesting aanpak, maken een manifest (IngestManifest) waarmee de activa en de bijbehorende bestanden wordt beschreven. De bijbehorende bestanden uploaden naar van het manifest blob container gebruikt u de uploadmethode van uw keuze. Microsoft Azure Media Services toekijkt de blob container het manifest is gekoppeld. Wanneer een bestand wordt geüpload naar de container blob, is het activum maken op basis van de configuratie van de activa in het manifest (IngestManifestAsset) in Microsoft Azure Media Services voltooid.


Een nieuwe IngestManifest-oproep de methode Create die worden aangeboden door de verzameling IngestManifests op de CloudMediaContext maken. Deze methode maakt een nieuwe IngestManifest met de naam van de manifest die u opgeeft.

    IIngestManifest manifest = context.IngestManifests.Create(name);

De activa die gekoppeld aan de bulksgewijs IngestManifest zijn maken. Stel de gewenste versleutelingsopties op het activum voor het bulksgewijs ingesting.

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

Activa koppelt een IngestManifestAsset aan een groot aantal IngestManifest voor het bulksgewijs ingesting. Ook wordt de AssetFiles waaruit alle activa gekoppeld. Als u wilt een IngestManifestAsset maken, gebruikt u de methode van het maken van de servercontext.

Het volgende voorbeeld bevat toe te voegen twee nieuwe IngestManifestAssets die koppelen van de twee activa eerder hebt gemaakt naar het bulksgewijs nemen manifest. Elke IngestManifestAsset koppelt een set bestanden die worden geüpload voor elk activum ook tijdens het bulksgewijs ingesting.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;
    
    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
    
U kunt een hoge snelheid client-toepassing kan de activa-bestanden uploaden naar de blob storage container URI verstrekt door de eigenschap **IIngestManifest.BlobStorageUriForUpload** van de IngestManifest gebruiken. Een aantal aanzienlijke hoge snelheid upload-service is [Aspera op aanvraag voor Azure-toepassing](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). U kunt ook code als u wilt uploaden de bestanden activa zoals wordt weergegeven in het volgende voorbeeld schrijven.
    
    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);
    
            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);
    
            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);
    
            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });
    
        copytask.Start();
    }

De code voor het uploaden van bestanden van de activa voor de steekproef gebruikt in dit onderwerp wordt weergegeven in het volgende voorbeeld.
    
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
    

U kunt de voortgang van het bulksgewijs ingesting voor alle activa die is gekoppeld aan een **IngestManifest** door de eigenschap statistieken van de **IngestManifest**polling vaststellen. Om bij te werken voortgangsgegevens, moet u een nieuwe **CloudMediaContext** telkens wanneer die u de eigenschap statistieken opvragen.

Het volgende voorbeeld wordt een IngestManifest met een **Id**polling.
    
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();
    
          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);
    
                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }
    
             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
    


##<a name="upload-files-using-net-sdk-extensions"></a>Upload bestanden met behulp van .NET SDK extensies 

Het volgende voorbeeld ziet hoe u met behulp van .NET SDK extensies één bestand uploaden. In dit geval de methode **CreateFromFile** wordt gebruikt, maar de asynchroon versie is ook beschikbaar (**CreateFromFileAsync**). De methode **CreateFromFile** kunt u de bestandsnaam, de versleutelingsoptie en een terugbellen om de voortgang van het uploaden van het bestand rapporteren opgeven.


    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });
    
        Console.WriteLine("Asset {0} created.", inputAsset.Id);
    
        return inputAsset;
    }

Het volgende voorbeeld UploadFile functie belt en Hiermee geeft u opslag-codering als de optie voor het maken van activa.  


    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-step"></a>Volgende stap

Nu dat u hebt activa geüpload naar Media Services, gaat u naar het onderwerp [Hoe kom ik aan een Processor Media][] .

[Hoe kom ik aan een Mediaprocessor]: media-services-get-media-processor.md
 
