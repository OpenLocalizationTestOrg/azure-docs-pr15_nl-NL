<properties 
    pageTitle="Media Services bijwerken na opslag toegangstoetsen schuivend | Microsoft Azure" 
    description="In dit artikel krijgt u richtlijnen voor het bijwerken van Media Services na opslag toegangstoetsen schuivend." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="milangada;cenkdin;juliako"/>

#<a name="update-media-services-after-rolling-storage-access-keys"></a>Media Services bijwerken nadat het schuivend toegangstoetsen opslag

Wanneer u een nieuw Azure Media Services-account maakt, wordt u ook gevraagd om te selecteren van een Azure Storage-account dat is gebruikt voor het opslaan van uw media-inhoud. Houd er rekening mee dat u [meer dan één opslag-accounts toevoegen](meda-services-managing-multiple-storage-accounts.md) aan uw Media Services-account kunt.

Wanneer een nieuw account voor de opslag is gemaakt, genereert Azure twee 512 bits opslag toegangstoetsen, die worden gebruikt om te verifiëren toegang tot uw account opslag. Uw opslag-verbindingen meer om veilig te houden, wordt het aanbevolen regelmatig genereren en uw opslagruimte toegangstoets draaien. Twee toegangstoetsen (primaire en secundaire) worden gegeven zodat u voor het behoud van verbindingen met de opslag-account met behulp van een access-toets ingedrukt terwijl u de andere access-sleutel opnieuw genereren. Deze procedure wordt ook 'lopende toegangstoetsen' genoemd.

Media-Services, is afhankelijk van een sleutel opslagruimte toe. De Locator die worden gebruikt om te streamen of downloaden van uw activa is specifiek, afhankelijk van de opgegeven opslag access-toets. Wanneer een AMS-account is gemaakt duurt een afhankelijkheid op de primaire opslag toegangstoets al dan niet standaard maar u kunt de opslagruimte-toets AMS met bijwerken als een gebruiker. U moet controleren om te laten weten welke toets gebruiken door de stappen in dit onderwerp beschreven Media-Services. Wanneer de toegangstoetsen opslag schuivend, moet u ook om te controleren of uw Locator dus bijwerken wordt niet onderbroken in uw streaming service (deze stap is ook in het onderwerp beschreven).

>[AZURE.NOTE]Als u meerdere opslag-accounts hebt, kunt u deze procedure met elk account opslagruimte wilt uitvoeren.
>
>Zorg dat u deze op een account in preproductie testen voordat u de stappen in dit onderwerp wordt beschreven in een productierekening uitvoert.


## <a name="step-1-regenerate-secondary-storage-access-key"></a>Stap 1: Genereren secundaire opslag toegangstoets

Begin met het genereren van secundaire opslag-toets. Standaard de secundaire sleutel niet wordt gebruikt door Services Media.  Zie voor informatie over het implementeren van opslag toetsen [hoe: weergave, kopiëren en opnieuw genereren opslag toegangstoetsen](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
  
##<a id="step2"></a>Stap 2: Update Media Services om met de nieuwe secundaire opslag-toets

Media-Services als u wilt gebruiken de secundaire opslag toegangstoets bijwerken. U kunt een van de volgende twee methoden voor het synchroniseren van de toets geregenereerde opslagruimte met Media-Services.

- Gebruik van de Azure-portal: om de naam en de toets waarden zoeken, gaat u naar de Azure-portal en selecteer uw account. Het venster instellingen wordt weergegeven aan de rechterkant. Selecteer in het venster instellingen van toetsen. Afhankelijk van welke opslag-toets voor de Media-Services synchroniseren met de gewenste, selecteer de primaire sleutel synchroniseren of de secundaire belangrijke knop synchroniseren. In dit geval gebruikt de secundaire sleutel.

- Media Services management REST API gebruiken.

Het volgende voorbeeld ziet u hoe u kunt de https://endpoint/***subscriptionId/services/mediaservices/Accounts/accountnaam*/StorageAccounts/**storageAccountName/Key verzoek om te synchroniseren van de opgegeven opslag-toets met Media Services gebruiken. In dit geval wordt de secundaire opslag-sleutelwaarde gebruikt. Zie voor meer informatie [hoe: Gebruik Media Services Management REST API](http://msdn.microsoft.com/library/azure/dn167656.aspx).
    
    public void UpdateMediaServicesWithStorageAccountKey(string mediaServicesAccount, string storageAccountName, string storageAccountKey)
    {
        var clientCert = GetCertificate(CertThumbprint);
        
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(string.Format("{0}/{1}/services/mediaservices/Accounts/{2}/StorageAccounts/{3}/Key",
        Endpoint, SubscriptionId, mediaServicesAccount, storageAccountName));
        request.Method = "PUT";
        request.ContentType = "application/json; charset=utf-8";
        request.Headers.Add("x-ms-version", "2011-10-01");
        request.Headers.Add("Accept-Encoding: gzip, deflate");
        request.ClientCertificates.Add(clientCert);
        
        
        using (var streamWriter = new StreamWriter(request.GetRequestStream()))
        {
            streamWriter.Write("\"");
            streamWriter.Write(storageAccountKey);
            streamWriter.Write("\"");
            streamWriter.Flush();
        }
        
        using (var response = (HttpWebResponse)request.GetResponse())
        {
            string jsonResponse;
            Stream receiveStream = response.GetResponseStream();
            Encoding encode = Encoding.GetEncoding("utf-8");
            if (receiveStream != null)
            {
                var readStream = new StreamReader(receiveStream, encode);
                jsonResponse = readStream.ReadToEnd();
            }
        }
    }

Bijwerken na deze stap, bestaande Locator (die afhankelijkheid hebben op de oude opslag-sleutel) zoals wordt weergegeven in de volgende stap.

>[AZURE.NOTE]Wacht totdat 30 minuten voordat er bewerkingen met Media-Services (bijvoorbeeld maken nieuwe Locator) om te voorkomen dat een impact op openstaande taken.

##<a name="step-3-update-locators"></a>Stap 3: Update Locator

>[AZURE.NOTE]Wanneer de toegangstoetsen opslag schuivend, moet u om te controleren of uw bestaande Locator bijwerken zodat er niet onderbroken in uw streaming-service wordt.

Wacht ten minste 30 minuten na het synchroniseren van de nieuwe sleutel in de opslagruimte met AMS. Vervolgens kunt u opnieuw maken uw Locator OnDemand zodat zij afhankelijkheid ondernemen in de opgegeven opslag-toets en voor het behoud van de bestaande URL.

Houd er rekening mee dat wanneer u bijwerken (of maak opnieuw) een locator SA's, de URL wordt altijd wijzigen.

>[AZURE.NOTE] Om ervoor te zorgen dat u de bestaande URL's van uw Locator OnDemand behouden, moet u de bestaande locator verwijderen en maak een nieuwe met dezelfde-ID.

De onderstaande .NET-voorbeeld wordt getoond hoe opnieuw maken van een locator met dezelfde-ID.

privé statische ILocator RecreateLocator(CloudMediaContext context, ILocator locator) {/ / eigenschappen van bestaande locator opslaan.
var activa = locator. Actief; var accessPolicy = locator. AccessPolicy; var locatorId = locator. -ID; var startDate = locator. Starttijd; var locatorType = locator. Typ; var locatorName = locator. Naam;

Verwijder de oude locator.
Locator. Delete();

Als (locator. ExpirationDateTime < = DateTime.UtcNow) {nieuwe uitzondering (String.Format (' niet opnieuw kunt maken locator Id = {0} omdat de verlooptijd locator is in het verleden ', locator. ID)); }
    
        // Create new locator using saved properties.
        var newLocator = context.Locators.CreateLocator(
            locatorId,
            locatorType,
            asset,
            accessPolicy,
            startDate,
            locatorName);
    
    
    
        return newLocator;
    }


##<a name="step-5-regenerate--primary-storage-access-key"></a>Stap 5: Genereren primaire opslag toegangstoets

De primaire opslag toegangstoets genereren. Zie voor informatie over het implementeren van opslag toetsen [hoe: weergave, kopiëren en opnieuw genereren opslag toegangstoetsen](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

##<a name="step-6-update-media-services-to-use-the-new-primary-storage-key"></a>Stap 6: Update Media Services om met de nieuwe primaire opslag-toets
    
Gebruik de procedure zoals is beschreven in [stap 2](media-services-roll-storage-access-keys.md#step2) alleen op dit moment de nieuwe primaire opslag-toegangstoets worden gesynchroniseerd met de Media Services-account.

>[AZURE.NOTE]Wacht totdat 30 minuten voordat er bewerkingen met Media-Services (bijvoorbeeld maken nieuwe Locator) om te voorkomen dat een impact op openstaande taken.

##<a name="step-7-update-locators"></a>Stap 7: Update Locator  

U kunt opnieuw maken uw Locator OnDemand zodat zij afhankelijkheid voor de nieuwe sleutel in de primaire opslag uitvoeren en onderhouden van de bestaande URL na 30 minuten.

Gebruik de procedure zoals is beschreven in [stap 3](media-services-roll-storage-access-keys.md#step-3-update-locators).


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



###<a name="acknowledgments"></a>Bevestigingen 

Willen we Bevestig het volgen van mensen die bijgedragen tot het maken van dit document: Cenk Dingiloglu, Milaan Gada, Seva Titov.
