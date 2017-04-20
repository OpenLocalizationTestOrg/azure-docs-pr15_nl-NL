<properties
    pageTitle="App-Service API Apps - wat er veranderd | Microsoft Azure"
    description="Informatie over nieuwe functies voor API-Apps in Azure App-Service."
    services="app-service\api"
    documentationCenter=".net"
    authors="mohitsriv"
    manager="wpickett"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps---whats-changed"></a>App-Service API Apps - wat er veranderd

Aan de gebeurtenis connect () in November 2015 zijn een aantal verbeteringen aan Azure App Service [aangekondigd](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Deze verbeteringen bevatten onderliggende wijzigingen in API-Apps beter uitlijnen met mobiele en Web Apps, beperken concept tellen en implementatie en runtime prestaties te verbeteren. De nieuwe API-apps November 30 2015 verlengt, beginnend u maakt met behulp van de Azure management portal of de meest recente tooling worden deze wijzigingen doorgevoerd. In dit artikel worden deze wijzigingen, alsmede het implementeren van bestaande apps om te profiteren van de mogelijkheden.

## <a name="feature-changes"></a>Functiewijzigingen
De belangrijkste functies van API Apps – verificatie, CORS en API metagegevens – hebt rechtstreeks naar de App Service verplaatst. Met deze wijziging zijn de functies beschikbaar in Web, Mobile en API-Apps. Alle drie delen in feite hetzelfde **Microsoft.Web/sites** resourcetype in resourcemanager. De gateway API Apps is niet meer nodig of aangeboden met API-Apps. Dit ook vergemakkelijkt het gebruiken van Azure API Management, aangezien er alleen de één-API Management gateway worden.

![Overzicht van de API-Apps](./media/app-service-api-whats-changed/api-apps-overview.png)

Een ontwerpprincipe belangrijke met de API Apps-update is waarmee u om uw API zoals is, in de taal van keuze weer te geven.  Als uw API is al geïmplementeerd als in de browser of mobiele App, hoeft u niet opnieuw te implementeren van uw app om te profiteren van de nieuwe functies. Als u op dit moment API Apps preview, wordt de migratie richtlijnen hieronder beschreven.

### <a name="authentication"></a>Verificatie
De bestaande meteen API-Apps, Mobile-Services/Apps en -WebApps verificatiefuncties hebt is unified en beschikbaar zijn in een enkel Azure App Service verificatie blade in de beheerportal. Zie voor een inleiding tot verificatieservices in App Service, [verificatie van de App-Service gegevensniveaus uitvouwen / autorisatie](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

Er zijn een aantal relevante nieuwe mogelijkheden voor API-scenario's:

- **Ondersteuning voor het gebruik van de Azure Active Directory rechtstreeks**, zonder dat u moet het token AAD voor een sessietoken exchange clientcode: de klant kunt alleen de tokens AAD in de koptekst autorisatie opnemen op basis van de vruchtdragende token specificatie. Dit betekent ook dat geen SDK App Service / regiospecifieke aan de kant-client of server is vereist. 
- **Service-naar-service of "Interne" access**: als u een proces of enkele andere client toegang tot de API's zonder een interface dat u nodig hebt, kunt u een token met een AAD service hoofdsom aanvragen en doorgegeven aan de App Service voor verificatie met uw toepassing.
- **Uitgesteld autorisatie**: veel toepassingen hebben verschillende toegangsbeperkingen voor verschillende delen van de toepassing. Misschien wilt u enkele API's worden openbaar, terwijl de anderen Vereis aanmelding. De oorspronkelijke verificatie-functie is ofwel volledig, ofwel, met de hele site aanmelding vereisen. Deze optie nog bestaat, maar u kunt ook uw toepassingscode worden weergegeven door access beslissingen nadat App Service heeft de gebruiker geverifieerd.
 
Zie voor meer informatie over de nieuwe verificatiefuncties, [verificatie en machtiging voor API-Apps in Azure App-Service](app-service-api-authentication.md). Bestaande API apps uit het vorige model van de API-apps naar het nieuwe, Zie voor informatie over het migreren [migreren bestaande API apps](#migrating-existing-api-apps) verderop in dit artikel.
 
### <a name="cors"></a>CORS
In plaats van een CSV- **MS_CrossDomainOrigins** app instelling is er nu een blade in de portal Azure management voor het configureren van CORS. U kunt ook kan worden geconfigureerd met Resource Manager gereedschap zoals Azure PowerShell, CLI of [Resource Explorer](https://resources.azure.com/). De eigenschap **cors** instellen voor het type van de resource **Microsoft.Web/sites/config** voor uw ** &lt;sitenaam&gt;/web** resource. Bijvoorbeeld:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>API-metagegevens
Het API definitie blad is nu beschikbaar op Web, Mobile en API-Apps. In de beheerportal, kunt u een relatieve url of een absolute url die verwijst naar een eindpunt die hosts een Swagger 2.0-weergave van uw API opgeven. U kunt ook kan worden geconfigureerd met resourcemanager tooling. De eigenschap **apiDefinition** instellen voor het type van de resource **Microsoft.Web/sites/config** voor uw ** &lt;sitenaam&gt;/web** resource. Bijvoorbeeld:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

Op dit moment moet het eindpunt metagegevens openbaar zonder verificatie voor veel van de volgende clients (bijvoorbeeld Visual Studio REST API-client generatie en PowerApps 'Toevoegen API' stroom) om te kunnen maken. Dit betekent dat als u van App Service verificatie gebruikmaakt en wilt laten zien de API-definitie van binnen de app zelf, moet u de verificatie uitgesteld optie eerder wordt beschreven, zodat de route de metagegevens van uw Swagger openbare is te gebruiken.

## <a name="management-portal"></a>Management Portal
Selecteren **Nieuw > Web + Mobile > API App** maken in de portal API-apps die overeenkomen met de nieuwe mogelijkheden in het artikel beschreven. **Bladeren > API Apps** wordt alleen weergeven deze nieuwe API-apps. Zodra u naar een API-app zoeken, wordt het blad deelt de dezelfde indeling en mogelijkheden als die van Web en Mobile-Apps. De enige verschillen zijn quickstart-inhoud en de volgorde van instellingen.

Bestaande API-apps (of Marketplace-API-apps die zijn gemaakt op basis van logica Apps) met het vorige voorbeeld mogelijkheden nog steeds niet zichtbaar in de ontwerpfunctie voor logica Apps en tijdens het bladeren door alle resources in een resourcegroep.

## <a name="visual-studio"></a>Visual Studio

De meeste WebApps gereedschap werkt met nieuwe API-apps aangezien ze het dezelfde onderliggende **Microsoft.Web/sites** brontype delen. De Azure Visual Studio gereedschap, echter moeten bijgewerkte versie van punt 2.8.1 of later aangezien een aantal mogelijkheden voor specifieke API's worden getoond. Download de SDK van de [pagina Azure downloads](https://azure.microsoft.com/downloads/).

Publiceren met het soort optimalisatie van de App Service typen, wordt ook unified onder **publiceren > App-Service van Microsoft Azure**:

![API-Apps publiceren](./media/app-service-api-whats-changed/api-apps-publish.png)

Lees meer informatie over SDK punt 2.8.1, de aankondiging [van blogberichten](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).

U kunt ook handmatig het profiel publiceren importeren uit de beheerportal om in te schakelen publiceren. Echter Cloud Explorer, code genereren en API app selectie/maken SDK punt 2.8.1 moet of hoger.

## <a name="migrating-existing-api-apps"></a>Migreren van bestaande API-apps
Als uw aangepaste API wordt geïmplementeerd naar de vorige Preview-versie van API-Apps, kunt u het vergaderverzoek dat u naar het nieuwe model voor API-Apps met 31 December 2015 migreert. Aangezien zowel het oude en nieuwe model zijn gebaseerd op Web API's die zijn ingesloten in een App-Service, het grootste deel van een bestaande code opnieuw kan worden gebruikt.

### <a name="hosting-and-redeployment"></a>Hosten en opnieuw installeren
De stappen voor het opnieuw distribueren zijn hetzelfde als u een bestaande Web API implementatie in App Service. Stappen:

1. Een lege API-app maken. Dit kunt doen in de portal met nieuw > API-App in Visual Studio vanuit publiceren of resourcemanager tooling. Als gebruikt resourcemanager tooling of sjablonen, stelt u de waarde **type** tot **api** van het type van de resource **Microsoft.Web/sites** in de beheerportal gericht op API-scenario's de QuickStart en instellingen hebben.
2. Verbinding maken en implementeren van uw project naar de lege API-app met een van de implementatie-regelingen worden ondersteund door de App-Service. Lees de [documentatie over de implementatie van Azure App-Service](../app-service-web/web-sites-deploy.md) voor meer informatie. 
  
### <a name="authentication"></a>Verificatie
De verificatieservices App Service ondersteuning voor dezelfde mogelijkheden die beschikbaar zijn met het vorige API Apps model waren. Als u sessie tokens gebruikt en SDK's vereisen, gebruikt u de SDK van de volgende clients en servers's:

- Client: [Azure mobiele Client SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- Server: [Microsoft Azure mobiele App .NET verificatie-extensie](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Als u de App Service alfa SDK's in plaats daarvan hebt gebruikt, zijn deze nu afgeschaft:

- Client: [Microsoft Azure AppService SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)
- Server: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

Met name met Azure Active Directory is maar geen App Service / regiospecifieke vereist als u het token AAD rechtstreeks gebruikt.

### <a name="internal-access"></a>Toegang tot de binnenkant
Het vorige API Apps model opgenomen een ingebouwde interne toegangsniveau. Dit gebruik van de SDK nodig voor ondertekening aanvragen. Zoals eerder beschreven, met het nieuwe model API-Apps, kunnen AAD service principes worden gebruikt als alternatief voor verificatie van de service-naar-service zonder een App-Service-specifieke SDK. Lees meer in [Service-principal verificatie voor API-Apps in Azure App-Service](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Discovery
Het vorige API Apps model had API's voor andere apps API gedurende runtime in dezelfde resourcegroep achter dezelfde gateway ontdekken. Dit is vooral handig in architecturen die microservice patronen implementeren. Terwijl deze wordt niet rechtstreeks ondersteund, zijn een aantal opties beschikbaar:

1. Gebruik van de Azure resourcemanager-API voor detectie.
2. Azure API Management vóór uw App Service gehoste-API's plaatst. Azure API Management fungeert als een gevel en een stabiele externe omlaag url kan bieden, zelfs als u interne topologie verandert.
3. Uw eigen discovery-API-app maken en andere API-apps hebt geregistreerd met de app discovery bij het opstarten hebben.
4. Bij de implementatie, de instellingen voor de app van alle API-apps (en -clients) in te vullen met de eindpunten van de andere API-apps. Dit is beschikbaar in de sjabloon implementaties en aangezien API-Apps u nu kunt de besturing van de url.

## <a name="using-api-apps-with-logic-apps"></a>API-Apps gebruiken met logica Apps

Het nieuwe model van de API-apps werkt ook met de [Logica Apps schemaversie 2015-08-01](../app-service-logic/app-service-logic-schema-2015-08-01.md).

## <a name="next-steps"></a>Volgende stappen

Lees de artikelen in de [sectie API Apps-documentatie](https://azure.microsoft.com/documentation/services/app-service/api/)voor meer informatie. Ze zijn bijgewerkt zodat het nieuwe model voor API-Apps. Bovendien contact maken op de forums voor meer informatie of richtlijnen voor migratie:

- [MSDN-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
- [Stapel overloop](http://stackoverflow.com/questions/tagged/azure-api-apps)
