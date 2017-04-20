
<properties 
    pageTitle="API Swashbuckle gegenereerde definities aanpassen" 
    description="Informatie over het aanpassen van Swagger API definities die zijn gegenereerd met Swashbuckle voor een API-app in Azure App-Service." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/29/2016" 
    ms.author="rachelap"/>

# <a name="customize-swashbuckle-generated-api-definitions"></a>API Swashbuckle gegenereerde definities aanpassen 

## <a name="overview"></a>Overzicht

In dit artikel wordt uitgelegd hoe u het aanpassen van Swashbuckle om af te handelen veelvoorkomende scenario's waarin u kunt het standaardgedrag te wijzigen:

* Swashbuckle genereert dubbele bewerking-id's voor overbelastingen van controller methoden
* Swashbuckle wordt ervan uitgegaan dat het alleen geldig antwoord van een methode HTTP 200 (OK) 
 
## <a name="customize-operation-identifier-generation"></a>Bewerking ID generatie aanpassen

Swashbuckle genereert Swagger bewerking-id's voor het samenvoegen van de naam van de controller en de methodenaam van de. Dit patroon Hiermee maakt u een probleem wanneer er meerdere overbelastingen van een methode: Swashbuckle genereert dubbele bewerking-id's, namelijk ongeldige Swagger JSON.

De volgende controller-code wordt bijvoorbeeld Swashbuckle met semiselectie drie Contact_Get bewerking-id's wilt maken.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

U kunt het probleem handmatig oplossen door middel van een unieke naam, zoals de volgende onderwerpen voor dit voorbeeld de methoden:

* Toevoegen
* GetById
* GetPage

Het alternatief is om uit te breiden Swashbuckle om het selectievakje Automatisch schrijfinstructies unieke bewerking-id's te maken.

De volgende stappen hoe Swashbuckle aanpassen met behulp van het *SwaggerConfig.cs* -bestand dat is opgenomen in het project door de project-sjabloon voor Visual Studio API Apps Preview.  U kunt ook Swashbuckle aanpassen in een project Web API die u voor implementatie als een app-API configureren.

1. Maken van een aangepaste `IOperationFilter` implementatie 

    De `IOperationFilter` interface biedt een uitbreidingspunt voor Swashbuckle-gebruikers die u wilt aanpassen van verschillende aspecten van het proces van de metagegevens Swagger. De volgende code ziet een methode van het wijzigen van het gedrag van de bewerking-id-generatie. De code voegt parameternamen aan de naam van de bewerking-id.  

        using Swashbuckle.Swagger;
        using System.Web.Http.Description;
        
        namespace ContactsList
        {
            public class MultipleOperationsWithSameVerbFilter : IOperationFilter
            {
                public void Apply(
                    Operation operation,
                    SchemaRegistry schemaRegistry,
                    ApiDescription apiDescription)
                {
                    if (operation.parameters != null)
                    {
                        operation.operationId += "By";
                        foreach (var parm in operation.parameters)
                        {
                            operation.operationId += string.Format("{0}",parm.name);
                        }
                    }
                }
            }
        }

2. In *App_Start\SwaggerConfig.cs* -bestand, belt u het `OperationFilter` Swashbuckle gebruik van de nieuwe hierbij `IOperationFilter` implementatie.

        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();

    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)

    De *SwaggerConfig.cs* -bestand dat is verwijderd uit door het pakket Swashbuckle NuGet bevat veel toegelicht-out voorbeelden van uitbreiden wordt verwezen. De extra opmerkingen worden hier niet weergegeven. 

    Nadat u deze wijziging hebt aangebracht uw `IOperationFilter` -implementatie wordt gebruikt en zorgt ervoor dat de bewerking unieke id's moet worden gegenereerd.
 
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>
    
## <a name="allow-response-codes-other-than-200"></a>Antwoord codes dan 200 toestaan

Standaard Swashbuckle wordt ervan uitgegaan dat een antwoord HTTP 200 (OK) het *alleen* legitieme antwoord uit een Web API-methode is. In sommige gevallen wilt u mogelijk andere codes antwoord retourneren zonder dat de client naar een uitzondering veroorzaken.  De volgende Web API-code ziet u bijvoorbeeld een scenario waarin u de client naar het accepteren van een 200 of een 404 als geldige antwoorden wilt.

    [ResponseType(typeof(Contact))]
    public HttpResponseMessage Get(int id)
    {
        var contacts = GetContacts();

        var requestedContact = contacts.FirstOrDefault(x => x.Id == id);

        if (requestedContact == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        else
        {
            return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
        }
    }

In dit scenario geeft de Swagger die Swashbuckle al dan niet standaard genereert slechts één legitiem HTTP-statuscode, HTTP 200.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Aangezien de definitie Swagger API Visual Studio gebruikt om code te genereren voor de client, maakt het clientcode die een uitzondering voor elk antwoord dan een HTTP-200 genereert. De onderstaande code is van een C#-client gegenereerd voor deze steekproef Web API-methode.

    if (statusCode != HttpStatusCode.OK)
    {
        HttpOperationException<object> ex = new HttpOperationException<object>();
        ex.Request = httpRequest;
        ex.Response = httpResponse;
        ex.Body = null;
        if (shouldTrace)
        {
            ServiceClientTracing.Error(invocationId, ex);
        }
        throw ex;
    } 

Swashbuckle zijn er twee manieren voor het aanpassen van de lijst met verwachte HTTP-antwoord codes die wordt gegenereerd met XML-opmerkingen of de `SwaggerResponse` kenmerk. Het kenmerk is gemakkelijker, maar het is alleen beschikbaar in Swashbuckle 5.1.5 of hoger. De API Apps preview nieuw project-sjabloon in Visual Studio 2013 bevat Swashbuckle versie 5.0.0, dus als u de sjabloon gebruikt en niet wilt bijwerken Swashbuckle, de enige optie is het gebruik van XML-opmerkingen. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>Verwachte antwoord codes met XML-opmerkingen aanpassen

Deze methode te gebruiken om op te geven antwoord codes als uw versie Swashbuckle ouder dan 5.1.5 is.

1. Eerst XML-documentatieopmerkingen toevoegen via de methoden die u wilt opgeven HTTP antwoord codes voor. Het duurt van de steekproef Web API resultaat actie hierboven en de XML-documentatie toe te passen code zoals in het volgende voorbeeld. 

        /// <summary>
        /// Returns the specified contact.
        /// </summary>
        /// <param name="id">The ID of the contact.</param>
        /// <returns>A contact record with an HTTP 200, or null with an HTTP 404.</returns>
        /// <response code="200">OK</response>
        /// <response code="404">Not Found</response>
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
        
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
        
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

1. Instructies toevoegen in het bestand *SwaggerConfig.cs* om te leiden Swashbuckle gebruiken van de XML-documentatiebestand.

    * Open *SwaggerConfig.cs* en maken van een methode op de klasse *SwaggerConfig* om op te geven het pad naar de documentatie XML-bestand. 

            private static string GetXmlCommentsPath()
            {
                return string.Format(@"{0}\XmlComments.xml", 
                    System.AppDomain.CurrentDomain.BaseDirectory);
            }

    * Schuif omlaag in het bestand *SwaggerConfig.cs* totdat u de regel toegelicht-out van de code die lijkt op de schermafbeelding die u hieronder ziet. 

        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
    
    * Verwijder de opmerkingen bij de lijn om in te schakelen van de XML-opmerkingen processing tijdens Swagger generatie. 
    
        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
    
1. Om te genereren de documentatie XML-bestand, gaat u naar de eigenschappen van het project en het XML-documentatiebestand zoals wordt weergegeven in de onderstaande schermafbeelding inschakelen. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

Nadat u deze stappen hebt uitgevoerd, worden de HTTP-antwoord-codes die u hebt opgegeven in de XML-opmerkingen in de Swagger JSON gegenereerd door Swashbuckle doorgevoerd. De volgende schermafbeelding ziet u deze nieuwe JSON-nettolading. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

Wanneer u Visual Studio gebruikt voor de clientcode genereren voor de REST API, wordt in de C#-code HTTP OK zowel niet gevonden statuscodes accepteert zonder een uitzondering, zodat uw code in beslag nemen om te bepalen hoe u omgaat met het rendement van een null contactpersoonrecord. 

        if (statusCode != HttpStatusCode.OK && statusCode != HttpStatusCode.NotFound)
        {
            HttpOperationException<object> ex = new HttpOperationException<object>();
            ex.Request = httpRequest;
            ex.Response = httpResponse;
            ex.Body = null;
            if (shouldTrace)
            {
                ServiceClientTracing.Error(invocationId, ex);
            }
                throw ex;
        }

De code voor deze demo vindt u in [deze opslagplaats GitHub](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse). Samen met de Web-API is gemarkeerd met opmerkingen van XML-documentatie project een Console Application-project met een gegenereerde client voor deze API. 

### <a name="customize-expected-response-codes-using-the-swaggerresponse-attribute"></a>Verwachte antwoord codes met het kenmerk SwaggerResponse aanpassen

Het kenmerk [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) is beschikbaar in Swashbuckle 5.1.5 en hoger. Als u een eerdere versie in uw project hebt, wordt deze sectie wordt uitgelegd hoe u kunt het pakket Swashbuckle NuGet bijwerken zodat u dit kenmerk kunt begint.

1. **Oplossingverkenner**met de rechtermuisknop op het Web API-project en klik op **NuGet-pakketten beheren**. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)

1. Klik op de knop *bijwerken* naast het *Swashbuckle* NuGet-pakket. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)

1. De kenmerken *SwaggerResponse* toevoegen aan de Web API actie methoden waarvoor u wilt opgeven van geldige HTTP antwoord codes. 

        [SwaggerResponse(HttpStatusCode.OK)]
        [SwaggerResponse(HttpStatusCode.NotFound)]
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();

            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

2. Toevoegen een `using` instructie voor de naamruimte van het kenmerk:

        using Swashbuckle.Swagger.Annotations;
        
1. Blader naar de */swagger/docs/v1* -URL van uw project en wordt de verschillende HTTP antwoord codes zichtbaar zijn in de JSON Swagger. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

De code voor deze demo vindt u in [deze opslagplaats GitHub](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes). Samen met de Web-API is project versierd met het kenmerk *SwaggerResponse* een Console Application-project met een gegenereerde client voor deze API. 

## <a name="next-steps"></a>Volgende stappen

In dit artikel heeft het aanpassen van de manier waarop die swashbuckle bewerking-id's en geldig antwoord codes genereert weergegeven. Zie [Swashbuckle op GitHub](https://github.com/domaindrivendev/Swashbuckle)voor meer informatie.
 
