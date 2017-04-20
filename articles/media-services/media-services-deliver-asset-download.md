<properties 
    pageTitle="Media-activa downloaden" 
    description="Lees meer over activa naar uw computer downloaden. Voorbeelden van code worden geschreven in C# en gebruiken van de Media Services SDK voor .NET." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>

#<a name="how-to-deliver-an-asset-by-download"></a>Hoe u: geven activa via downloaden

In dit onderwerp wordt beschreven hoe de opties voor het afspelen van media-activa geüpload naar Media Services. In veel toepassing-scenario's kunt u Media Services inhoud leveren. U kunt media-activa downloaden of deze met behulp van een locator te openen. U kunt media-inhoud verzenden naar een andere toepassing of naar een andere provider voor inhoud. Voor betere prestaties en schaalbaarheid, kunt u ook inhoud leveren met behulp van een inhoud bezorging netwerk (CDN).

In dit voorbeeld ziet hoe u het media-activa naar uw lokale computer downloaden vanuit Media Services. De code van query's van de taken die zijn gekoppeld aan de Media Services-account door taak-ID en toegang tot de collectie **OutputMediaAssets** (dit is het instellen van een of meer uitvoer media-activa die het resultaat is van een taak wordt uitgevoerd). In dit voorbeeld wordt getoond hoe uitvoer media-activa downloaden van een taak, maar u kunt de dezelfde methode als u wilt downloaden van andere activa toepassen.

    
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 
    
        // Get a reference to the job. 
        IJob job = GetJob(jobId);
    
        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);
    
        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };
    
        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;
    
            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);
    
            Console.WriteLine("File download path:  " + localDownloadPath);
    
            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));
    
            outputFile.DownloadProgressChanged -= DownloadProgress;
        }
    
        Task.WaitAll(downloadTasks.ToArray());
    
        return outputAsset;
    }
    
    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

   
##<a name="see-also"></a>Zie ook 

[Streaming inhoud leveren](media-services-deliver-streaming-content.md)

