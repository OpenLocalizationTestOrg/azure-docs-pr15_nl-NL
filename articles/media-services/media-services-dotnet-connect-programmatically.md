<properties 
    pageTitle="Verbinding maken met Media Services-Account met behulp van .NET" 
    description="Dit onderwerp wordt beschreven hoe u verbinding maken met Media Services uisng .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a>Verbinding maken met Media Services-Account met behulp van Media Services SDK voor .NET

> [AZURE.SELECTOR]
- [REST](media-services-rest-connect-programmatically.md)
- [.NET](media-services-dotnet-connect-programmatically.md)


In dit onderwerp wordt beschreven hoe aanvragen van een programma verbinding met Microsoft Azure Media Services tijdens het programmeren met de Media Services SDK voor .NET.


## <a name="connecting-to-media-services"></a>Verbinding maken met mediaservices

Verbinding Media Services via programmacode wordt moet u hebben een Azure-account eerder hebt ingesteld, Media Services voor dat account hebt geconfigureerd en klikt u vervolgens een Visual Studio project instellen voor ontwikkeling met de Media Services SDK voor .NET. Zie Setup voor ontwikkeling met de Media Services SDK voor .NET voor meer informatie.

Aan het einde van de installatieprocedure voor Media Services-account, moet u de volgende waarden in de vereiste verbinding verkregen. Gebruik deze programma verbindingen maken met Media-Services.

- De naam van uw Media Services-account.

- Uw accountsleutel Media Services.

U vindt deze waarden, Ga naar de Portal van Azure Managment, selecteer uw Media-Service-account en klik op het pictogram "**Sleutels beheren**" vanaf de onderkant van de portal-venster. De waarde te klikken op het pictogram naast elk tekstvak worden gekopieerd naar het systeemklembord.


## <a name="creating-a-cloudmediacontext-instance"></a>Een exemplaar CloudMediaContext maken

Als u wilt beginnen met programmeren ten opzichte van de Media Services die u moet maken van een exemplaar van **CloudMediaContext** die aangeeft van de servercontext. De **CloudMediaContext** bevat verwijzingen naar belangrijk verzamelingen inclusief taken, activa, bestanden, clienttoegangsbeleid en Locator.

>[AZURE.NOTE] De **CloudMediaContext** -klasse is niet thread-veilig. Maak een nieuwe CloudMediaContext per thread of per set bewerkingen.


CloudMediaContext heeft vijf constructor overbelastingen. Het wordt aanbevolen constructors die **MediaServicesCredentials** als een parameter gebruiken. Zie de **Hergebruiken Access besturingselement Service Tokens** die volgt voor meer informatie. 

Het volgende voorbeeld wordt de openbare CloudMediaContext(MediaServicesCredentials credentials) constructor:

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Access besturingselement Service Tokens hergebruiken

In dit gedeelte ziet u hoe tokens Access Control-Service opnieuw met behulp van CloudMediaContext constructors die MediaServicesCredentials als een parameter te gebruiken.


[Azure Active Directory-toegangsbeheer](https://msdn.microsoft.com/library/hh147631.aspx) (ook wel bekend als Access Control-Service of ACS) is een cloudservice waarmee u een eenvoudige manier voor verificatie en machtiging gebruikers toegang te krijgen tot hun webtoepassingen. Microsoft Azure Media Services controleert de toegang tot de services via OAuth-protocol dat is vereist een token ACS. De ACS-tokens ontvangt Media Services van een vergunning-server.

Wanneer u met de Services Media SDK ontwikkelt, kunt u de tokens niet verwerken omdat de SDK code managers ze voor u. Echter leidt de SDK volledig beheer van de ACS-tokens laten tot onnodige token aanvragen. Aanvragen van een tokens duurt en gebruikt de client en server resources. De server ACS bandbreedte ook de aanvragen als het tarief weer dat te hoog is. De limiet is 30 aanvragen per seconde, [ACS servicebeperkingen](https://msdn.microsoft.com/library/gg185909.aspx) Zie voor meer informatie.

Beginnen met de Media Services SDK versie (3.0.0.0), kunt u de tokens ACS hergebruiken. De **CloudMediaContext** constructors die **MediaServicesCredentials** als een parameter inschakelen voor het delen van de ACS-tokens tussen verschillende contexten. De klasse MediaServicesCredentials omvat de referenties Media Services. Als een token ACS beschikbaar is en de verlooptijd bekend is, kunt u een nieuw exemplaar van de MediaServicesCredentials maken met het token en doorgegeven aan de constructor van CloudMediaContext. Houd er rekening mee dat de Media Services SDK tokens automatisch vernieuwd wanneer ze verlopen. Er zijn twee manieren ACS tokens, opnieuw te gebruiken zoals wordt weergegeven in de onderstaande voorbeelden.

- U kunt het object **MediaServicesCredentials** in het geheugen (bijvoorbeeld in een variabele statische klasse) cache. Vervolgens doorgeven u het object in de cache aan de constructor CloudMediaContext. Het object MediaServicesCredentials bevat een token ACS die opnieuw kan worden gebruikt als dit nog geldig is. Als het token niet geldig is, wordt deze door de Media Services SDK met de referenties die zijn opgegeven voor de constructor MediaServicesCredentials worden vernieuwd.

    Houd er rekening mee dat het object **MediaServicesCredentials** geldig token krijgt nadat de RefreshToken wordt genoemd. De **CloudMediaContext** roept de methode **RefreshToken** in de constructor. Als u van plan bent de token waarden opslaan in een externe opslag, zorg om te controleren of de waarde TokenExpiration geldig is voordat de token gegevens worden opgeslagen. Als deze niet geldig is, belt u RefreshToken voordat het in cache opslaan.

        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        
        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }
        
        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

- U kunt ook de tekenreeks AccessToken en de waarden TokenExpiration cache. De waarden kunnen later worden gebruikt om een nieuw MediaServicesCredentials-object maken met gegevens in de cache token.  Dit is vooral handig voor scenario's waar het token veilig tussen meerdere processen of computers kan worden gedeeld.

    De volgende codefragmenten belt u met de SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage en UpdateTokenDataInExternalStorageIfNeeded methoden die niet in dit voorbeeld zijn gedefinieerd. U kunt de volgende manieren om te slaan, te halen en token gegevens in een externe opslag bijwerken definiëren. 

        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
        
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
        
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
        
    Gebruik de opgeslagen token waarden MediaServicesCredentials maken.


        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;
        
        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);
        
        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };
        
        CloudMediaContext context2 = new CloudMediaContext(credentials);

    Werk de token kopie geval het token is bijgewerkt door de Media Services SDK. 
    
        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }
        

- Als u meerdere Media Services-accounts (bijvoorbeeld voor het laden delen doeleinden of geografische-verdeling) hebt, kunt u MediaServicesCredentials objecten met de System.Collections.Concurrent.ConcurrentDictionary verzameling (de ConcurrentDictionary collectie bevat een thread-veilige sleutel/waardeparen die kunnen worden geopend door meerdere threads gelijktijdig) kunt cache. U kunt de methode GetOrAdd vervolgens gebruiken om het in de cache opgeslagen referenties te verkrijgen. 

        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();
        

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;
        
            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));
        
            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);
        
            return cloudMediaContext;
        }
        
## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a>Verbinding met een Media Services-account zich bevindt in de regio Noord China

Als uw account in de regio Noord China bevindt zich, moet u de volgende constructor gebruiken:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Bijvoorbeeld:


    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>Verbinding waarden in de configuratie opslaan

Het wordt ten zeerste aanbevolen verbinding waarden, met name gevoelige waarden zoals uw naam van het account en wachtwoord opslaan in de configuratie. Het is ook aanbevolen gevoelige configuratiegegevens versleutelen. U kunt het hele configuratiebestand coderen met behulp van het Windows coderen File System (EFS). Als u wilt inschakelen EFS op een bestand, met de rechtermuisknop op het bestand, selecteer **Eigenschappen**en versleuteling op het tabblad **Geavanceerd** instellingen inschakelen. Of u een aangepaste oplossing voor het coderen van geselecteerde gedeelten van een configuratiebestand met beveiligde configuratie kunt maken. Zie [codering configuratie informatie gebruik beveiligde configuratie](https://msdn.microsoft.com/library/53tyfkaw.aspx).

Het volgende App.config-bestand bevat de vereiste verbinding waarden. De waarden in de <appSettings> element zijn de vereiste waarden die u hebt ontvangen van de installatieprocedure voor Media Services-account.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


U kunt verbinding om waarden te vinden van de configuratie, gebruikt u de klas **ConfigurationManager** en vervolgens de waarden toewijzen aan velden in uw code:
    
    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
