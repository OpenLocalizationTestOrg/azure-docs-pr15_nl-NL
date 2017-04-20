
<properties 
    pageTitle="Activa en gerelateerde entiteiten met mediaservices .NET SDK beheren" 
    description="Informatie over het beheren van activa en gerelateerde entiteiten met de Media Services SDK voor .NET." 
    authors="juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016"
    ms.author="juliako"/>


#<a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Activa en gerelateerde entiteiten met mediaservices .NET SDK beheren


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-manage-entities.md)
- [REST](media-services-rest-manage-entities.md)


Dit onderwerp wordt uitgelegd hoe u de volgende Media Services management taken uitvoeren:

- Een verwijzing activa ophalen 
- Een taakverwijzing ophalen 
- Lijst van alle activa 
- Lijst taken en activa 
- Een lijst met alle clienttoegangsbeleid 
- Een lijst met alle Locator
- Opsommen tot en met grote verzamelingen van entiteiten
- Activa verwijderen 
- Een taak verwijderen 
- Een access-beleid verwijderen 

##<a name="prerequisites"></a>Vereisten voor 

Zie [uw omgeving instellen](media-services-set-up-computer.md)

##<a name="get-an-asset-reference"></a>Een verwijzing activa ophalen

Een veelgebruikte taak is om een verwijzing naar een bestaand activum in Media Services. Het volgende voorbeeld ziet u hoe u kunt krijgen een verwijzing activa van de verzameling activa op de server contextobject, op basis van de activa Id.
Het volgende voorbeeld wordt een query Linq gebruikt om een verwijzing naar een bestaand IAsset-object.

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();
    
        return asset;
    }

##<a name="get-a-job-reference"></a>Een taakverwijzing ophalen

Wanneer u werkt met het verwerken van taken in Media Services-code, moet vaak u een verwijzing naar een bestaande taak op basis van een Id. Het volgende voorbeeld ziet u hoe u een verwijzing naar een object IJob uit de verzameling taken.
Mogelijk moet WarningWarning u een overzicht van de taak bij het starten van een taak van langdurige-codering, en moeten de taakstatus bekijken op een thread. In gevallen strekking wanneer de methode geeft als van een lijn resultaat, moet u een vernieuwde verwijzing naar een taak ophalen.

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();
    
        return job;
    }

##<a name="list-all-assets"></a>Lijst van alle activa

Als het aantal elementen die u opgeslagen hebt in omvang groeit, is het handig voor een overzicht van uw activa. Het volgende voorbeeld ziet u hoe u de collectie activa op de server contextobject doorlopen. Met elk activum schrijft in het voorbeeld ook aantal van de eigenschapswaarden naar de console. Elk activum kan bijvoorbeeld veel mediabestanden bevatten. Het voorbeeld schrijft alle bestanden die zijn gekoppeld aan elke activa.



    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");
    
            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-jobs-and-assets"></a>Lijst taken en activa

Een belangrijke verwante taak is activa van de lijst met hun bijbehorende taak in Media Services. Het volgende voorbeeld ziet u hoe u elk object IJob en voor elke taak, klikt u vervolgens eigenschappen over de taak wordt weergegeven, alle gerelateerde taken, alle activa en alle uitvoer activa invoer. De code in dit voorbeeld zijn handig voor groot aantal andere taken. Bijvoorbeeld als u wilt voor een overzicht van de activa uitvoer van een of meer codering taken die u eerder hebt uitgevoerd, ziet deze code u het openen van de activa uitvoer. Wanneer u een verwijzing naar een actief uitvoer hebt, kunt u de inhoud vervolgens leveren aan andere gebruikers of toepassingen op downloaden of URL's leveren. 

Zie voor meer informatie over opties voor het afspelen van activa, [Activa geven met de Media Services SDK voor .NET](media-services-deliver-streaming-content.md).

    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");
    
    
            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }
    
            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {
    
                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-all-access-policies"></a>Een lijst met alle Clienttoegangsbeleid

U kunt een access-beleid definiëren op activa of de bestanden in Media Services. Een access-beleid bepaalt de machtigingen voor een bestand of een actief (welk type toegang en de duur). In uw Media Services-code definiëren u meestal een access-beleid door te maken van een object IAccessPolicy en klik vervolgens koppelen aan een bestaand activum. Maakt u een object ILocator, waarmee u de directe toegang tot activa in Media Services bieden. Het project Visual Studio waarop deze reeks documentatie bevat verschillende codevoorbeelden waarin wordt aangegeven hoe maken en -beleid en Locator toewijzen aan activa.

Het volgende voorbeeld ziet u hoe u een lijst met alle clienttoegangsbeleid op de server en ziet u de machtigingen die zijn gekoppeld aan elk type. Een andere handige manier om weer te geven clienttoegangsbeleid is voor een overzicht van alle ILocator objecten op de server en klik voor elke locator, u kunt een lijst met de bijbehorende-beleid met behulp van de eigenschap AccessPolicy.

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");
    
        }
    }

##<a name="list-all-locators"></a>Een lijst met alle Locator

Een locator is een URL waarmee een direct pad voor toegang tot een actief, samen met machtigingen om de activa zoals gedefinieerd door van de locator gekoppeld-beleid. Elke activa kan een verzameling ILocator objecten die zijn gekoppeld aan deze op de eigenschap Locator hebben. De servercontext, heeft ook een verzameling Locator waarin alle Locator.

Het volgende voorbeeld bevat alle Locator op de server. Voor elke locator wordt de Id voor de beleidsregel voor activa en access. Ook wordt het type machtigingen, de vervaldatum en het volledige pad naar de activa.

Houd er rekening mee dat een pad locator naar activa alleen een basis-URL naar het actief is. Als u wilt een pad naar afzonderlijke bestanden die een gebruiker of een toepassing kan Blader naar hebt gemaakt, moet uw code het specifieke bestandspad toevoegen aan het pad locator. Zie het onderwerp [Activa geven met de Media Services SDK voor .NET](media-services-deliver-streaming-content.md)voor meer informatie over hoe u dit wilt doen.

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a>Opsommen tot en met grote verzamelingen van entiteiten

Query's naar entiteiten, moet u er een limiet van 1000 entiteiten in één keer wordt geretourneerd, omdat openbare REST v2 queryresultaten tot 1000 resultaten beperkt is. U moet gebruiken overslaan en nemen bij het tot en met grote verzamelingen van entiteiten opsommen. 
    
De volgende functie doorlopen van alle taken in de opgegeven Media Services-Account. Media Services retourneert 1000-taken in taken siteverzameling. De functie gebruik maakt van overslaan en nemen om ervoor te zorgen dat alle taken worden weergegeven (als u meer dan 1000 taken in uw account hebt).
    
    static void ProcessJobs()
    {
        try
        {
    
            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;
    
            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }
    
                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

##<a name="delete-an-asset"></a>Activa verwijderen

Het volgende voorbeeld wordt een actief.

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();
    
        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");
    
    }

##<a name="delete-a-job"></a>Een taak verwijderen

Als u wilt verwijderen van een taak, moet u de status van de afdruktaak controleren, zoals aangegeven in de eigenschap staat. Taken die zijn voltooid of geannuleerd kunnen worden verwijderd, terwijl taken die in een bepaalde andere staat, zoals in de wachtrij, geplande of bewerken zijn, moeten eerst worden geannuleerd, en klik vervolgens ze kunnen worden verwijderd.
Het volgende voorbeeld ziet u een methode voor het verwijderen van een taak door controleren taakstatussen en vervolgens te verwijderen wanneer de status is voltooid of geannuleerd. Deze code is afhankelijk van de vorige sectie in dit onderwerp voor het verkrijgen van een verwijzing naar een taak: een taakverwijzing ophalen.

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;
    
        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);
    
            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }
    
        }
    }


##<a name="delete-an-access-policy"></a>Een Access-beleid verwijderen

Het volgende voorbeeld ziet u hoe u een verwijzing naar een access-beleid op basis van een beleid-Id en kiest u vervolgens het beleid verwijderen.

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();
    
        policy.Delete();
    
    }
    


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
