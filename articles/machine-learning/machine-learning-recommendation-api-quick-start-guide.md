<properties 
    pageTitle="Aan de slag met: Machine Learning aanbevelingen API | Microsoft Azure" 
    description="Azure Machine Learning-aanbevelingen - aan de slag met" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-machine-learning-recommendations-api"></a>Snel aan de slag-handleiding voor de Machine Learning aanbevelingen-API

>[AZURE.NOTE]U moet beginnen met de Service van aanbevelingen-API-cognitieve in plaats van deze versie. De aanbevelingen cognitieve Service wordt deze service worden vervangen, en de nieuwe functies worden er worden ontwikkeld. Er worden nieuwe mogelijkheden, zoals batchen ondersteuning, een betere API Explorer, een duidelijkere API oppervlak, consistenter registratie/facturering ervaring, enzovoort.
> Meer informatie over het [migreren naar de nieuwe cognitieve Service](http://aka.ms/recomigrate)


In dit document wordt beschreven hoe naar ingebouwde uw service of de toepassing Microsoft Azure Machine Learning aanbevelingen gebruiken. U vindt meer informatie over de aanbevelingen-API in de [Galerie](http://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="general-overview"></a>Algemeen overzicht

Als u wilt gebruiken Azure Machine Learning aanbevelingen, moet u de volgende stappen uit:

* Een model maken: een model is een container met uw gegevens over zoekgebruik, catalogusgegevens en het model aanbeveling.
* Catalogus importgegevens - een catalogus bevat metagegevens informatie over de items. 
* Gebruik importgegevens - gebruiksgegevens kunnen worden geüpload in 2 manieren (of beide):
    * Door het uploaden van een bestand met de gebruiksgegevens.
    * Door te sturen gegevens acquisition gebeurtenissen.
    U uploaden meestal een gebruik-bestand om te kunnen maken een eerste aanbeveling-model (bootstrap) en deze gebruiken totdat het systeem voldoende gegevens worden verzameld met behulp van de overname gegevensindeling.
* Maken van een model aanbeveling: dit is een asynchrone bewerking waarin het systeem aanbeveling duurt de gebruiksgegevens en maakt een model aanbeveling. Deze bewerking kan enkele minuten of enkele uren afhankelijk van de grootte van de gegevens en de parameters van de configuratie opbouwen nemen. Wanneer het bouwen activeert, krijgt u een opbouwen-ID. Gebruik deze om te controleren wanneer het proces opbouwen voordat u begint met het gebruiken van aanbevelingen is beëindigd.
* Gebruik aanbevelingen - Get-aanbevelingen voor een specifiek item of de lijst met items.

De bovenstaande stappen wordt uitgevoerd via de API Azure Machine Learning aanbevelingen.  U kunt een voorbeeldtoepassing die wordt geïmplementeerd elk van deze stappen uit downloaden de [galerie ook.](http://1drv.ms/1xeO2F3)

##<a name="limitations"></a>Beperkingen

* Het maximum aantal modellen per abonnement is 10.
* Het maximum aantal items dat een catalogus kan bevatten is 100.000.
* Het maximum aantal gebruik punten die worden gehouden is ~ 5,000,000. De oudste worden verwijderd als nieuwe bestanden worden geüpload of gerapporteerd.
* De maximale grootte van de gegevens die kunnen worden verzonden in bericht (bijvoorbeeld catalogusgegevens importeren, gebruik importgegevens) is 200MB.
* Het aantal transacties per seconde voor een aanbeveling model opbouwen die is niet actief is ~ 2TPS. Een aanbeveling model opbouwen die actief is, kan bevatten omhoog naar 20TPS.

##<a name="integration"></a>Integratie

###<a name="authentication"></a>Verificatie
Gepland in Microsoft Azure Marketplace ondersteunt de Basic of OAuth verificatiemethode. U kunt de toetsen account eenvoudig vinden door te gaan met de toetsen op de markt onder [uw accountinstellingen](https://datamarket.azure.com/account/keys). 
####<a name="basic-authentication"></a>Basisverificatie
De koptekst autorisatie toevoegen:

    Authorization: Basic <creds>
               
    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  
    
Converteren naar Base64 (C#)

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);
    
Converteren naar Base64 (JavaScript)

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);
    



###<a name="service-uri"></a>Service-URI
Ga naar de hoofdsite service URI voor de API's van Azure Machine Learning aanbevelingen is [hier.](https://api.datamarket.azure.com/amla/recommendations/v2/)

De volledige URI-service wordt uitgedrukt met behulp van elementen van de OData-specificatie.

###<a name="api-version"></a>API-versie
Elke API-aanroep heeft, aan het einde, een queryparameter apiVersion die moet worden ingesteld op '1.0' genoemd.

###<a name="ids-are-case-sensitive"></a>Id's zijn hoofdlettergevoelig
Id's, die het resultaat van een van de API's, zijn hoofdlettergevoelig en als zodanig moeten worden gebruikt als als parameters doorgegeven in mislukken volgende API-oproepen. Bijvoorbeeld zijn model-id's en catalogus-id's hoofdlettergevoelig.

###<a name="create-a-model"></a>Een model maken
Een verzoek 'maken model' maken:

| HTTP-methode | URI |
|:--------|:--------|
|Verzenden     |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br>Voorbeeld:<br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27`|

|   Parameternaam  |   Geldige waarden                        |
|:--------          |:--------                              |
|   modelName   |   Alleen letters (A-Z, a-z), cijfers (0-9), koppeltekens (-) en onderstrepingsteken (_) zijn toegestaan.<br>Maximale lengte: 20 |
|   apiVersion      | 1.0 |
|||
| Hoofdtekst aanvragen | GEEN |


**Antwoord**:

HTTP-statuscode: 200

- `feed/entry/content/properties/id`-Bevat het model-ID.
**Opmerking**: model-ID is hoofdlettergevoelig.

XML-OData

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


###<a name="import-catalog-data"></a>Gegevens van de catalogus importeren

Als u verschillende catalogusbestanden naar hetzelfde model met verschillende oproepen uploaden, wordt we alleen de nieuwe catalogusitems ingevoegd. Bestaande items blijft met de oorspronkelijke waarden.

| HTTP-methode | URI |
|:--------|:--------|
|Verzenden     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Voorbeeld:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Parameternaam  |   Geldige waarden                        |
|:--------          |:--------                              |
|   Model |   Unieke id van het model (hoofdlettergevoelig)  |
| FileName | Tekstuele id van de catalogus.<br>Alleen letters (A-Z, a-z), cijfers (0-9), koppeltekens (-) en onderstrepingsteken (_) zijn toegestaan.<br>Maximale lengte: 50 |
|   apiVersion      | 1.0 |
|||
| Hoofdtekst aanvragen | Catalogus gegevens. Indeling:<br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th>Naam</th><th>Verplicht</th><th>Type</th><th>Beschrijving</th></tr><tr><td>Lijstitem-Id</td><td>Ja</td><td>Alfanumerieke, maximale lengte 50</td><td>Unieke id van een item</td></tr><tr><td>Itemnaam</td><td>Ja</td><td>Alfanumerieke, maximale lengte 255</td><td>Itemnaam</td></tr><tr><td>Item categorie</td><td>Ja</td><td>Alfanumerieke, maximale lengte 255</td><td>Categorie waarbij deze hoort (bijvoorbeeld koken boeken, Drama...)</td></tr><tr><td>Beschrijving</td><td>Nee</td><td>Alfanumerieke, maximale lengte 4000</td><td>Beschrijving van dit item</td></tr></table><br>Maximale bestandsgrootte is 200MB.<br><br>Voorbeeld:<br><pre>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan, adresboek<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, de ruimte worden niet: een fictie (Byzantium map), van het adresboek<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, adresboek<br>552a1940-21e4-4399-82bb-594b46d7ed54, beperking van beesten, adresboek</pre> |


**Antwoord**:

HTTP-statuscode: 200

- `Feed\entry\content\properties\LineCount`-Het aantal regels geaccepteerd.
- `Feed\entry\content\properties\ErrorCount`-Aantal regels die niet zijn ingevoegd vanwege een fout.

XML-OData

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
        <subtitle type="text">Import catalog file</subtitle>
        <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
     </entry>
    </feed>


###<a name="import-usage-data"></a>Gebruiksgegevens importeren

####<a name="uploading-a-file"></a>Een bestand uploaden
In dit gedeelte ziet hoe u gegevens over zoekgebruik met behulp van een bestand uploaden. U kunt deze API enkele malen met gegevens over zoekgebruik bellen. Alle gegevens over zoekgebruik worden, opgeslagen voor alle gesprekken.

| HTTP-methode | URI |
|:--------|:--------|
|Verzenden     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Voorbeeld:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Parameternaam  |   Geldige waarden                        |
|:--------          |:--------                              |
|   Model |   Unieke id van het model (hoofdlettergevoelig) |
| FileName | Tekstuele id van de catalogus.<br>Alleen letters (A-Z, a-z), cijfers (0-9), koppeltekens (-) en onderstrepingsteken (_) zijn toegestaan.<br>Maximale lengte: 50 |
|   apiVersion      | 1.0 |
|||
| Hoofdtekst aanvragen | Gegevens over zoekgebruik. Indeling:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Naam</th><th>Verplicht</th><th>Type</th><th>Beschrijving</th></tr><tr><td>Gebruikers-Id</td><td>Ja</td><td>Alfanumerieke</td><td>Unieke id van een gebruiker</td></tr><tr><td>Lijstitem-Id</td><td>Ja</td><td>Alfanumerieke, maximale lengte 50</td><td>Unieke id van een item</td></tr><tr><td>Tijd</td><td>Nee</td><td>Datum in de notatie: jjjj/MM/ddTUU (bijvoorbeeld 2013/06/20T10:00:00)</td><td>Tijdstip van gegevens</td></tr><tr><td>Gebeurtenis</td><td>Nee, als opgegeven datum ook moet plaatsen</td><td>Een van de volgende handelingen uit:<br>• Klik<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Aanschaffen</td><td></td></tr></table><br>Maximale bestandsgrootte is 200MB.<br><br>Voorbeeld:<br><pre>149452, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951, 1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Antwoord**:

HTTP-statuscode: 200

- `Feed\entry\content\properties\LineCount`-Het aantal regels geaccepteerd.
- `Feed\entry\content\properties\ErrorCount`-Aantal regels die niet zijn ingevoegd vanwege een fout.
- `Feed\entry\content\properties\FileId`-Id van het bestand.


XML-OData

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="using-data-acquisition"></a>Gebruik van gegevens oplevert
In dit gedeelte ziet hoe u stuurt gebeurtenissen in realtime naar Azure Machine Learning aanbevelingen, meestal vanuit uw website.

| HTTP-methode | URI |
|:--------|:--------|
|Verzenden     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Parameternaam  |   Geldige waarden                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
|Hoofdtekst aanvragen| Gegevensinvoer gebeurtenis voor elke gebeurtenis die u wilt verzenden. U moet verzenden voor dezelfde gebruiker of browser sessie dezelfde-ID in het veld sessie-id. (Zie een voorbeeld van een gebeurtenis hoofdtekst onderstaande.)|


- Voorbeeld voor gebeurtenis 'Klikt u op':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Voorbeeld voor gebeurtenis 'RecommendationClick':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RecommendationClick</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Voorbeeld voor gebeurtenis 'AddShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Voorbeeld voor gebeurtenis 'RemoveShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RemoveShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Voorbeeld voor gebeurtenis 'Aanschaffen':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>

- Voorbeeld '2 gebeurtenissen op' en 'AddShopCart' verzenden:

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        <ItemName>itemName</ItemName>
        <ItemDescription>item description</ItemDescription>
        <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
        </EventData>
        </Event>

**Antwoord**: HTTP-statuscode: 200

###<a name="build-a-recommendation-model"></a>Een model aanbeveling maken

| HTTP-methode | URI |
|:--------|:--------|
|Verzenden     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Voorbeeld:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|

|   Parameternaam  |   Geldige waarden                        |
|:--------          |:--------                              |
| Model | Unieke id van het model (hoofdlettergevoelig)  |
| userDescription | Tekstuele id van de catalogus. Houd er rekening mee dat als u spaties gebruikt u deze met 20% in plaats daarvan coderen moet. Zie de bovenstaande voorbeeld.<br>Maximale lengte: 50 |
| apiVersion | 1.0 |
|||
| Hoofdtekst aanvragen | GEEN |

**Antwoord**:

HTTP-statuscode: 200

Dit is een asynchrone API. Als een antwoord krijgt u een ID opbouwen. Als u wilt weten wanneer het bouwen is beëindigd, moet u de API 'Krijgen genereert Status van een Model' aanroepen en zoek deze opbouwen-ID in het antwoord. Houd er rekening mee dat een opbouwen vanuit minuten naar uren afhankelijk van de grootte van de gegevens uitvoeren kunt.

U kunt geen aanbevelingen tot het bouwen gebruiken eindigt.

Geldige opbouwen status:

- Maken-Model is gemaakt.
- In de wachtrij – Model opbouwen is geactiveerd en deze in de wachtrij is.
- Building – Model wordt opgebouwd.
- Success – opbouwen is beëindigd.
- Fout – opbouwen beëindigd met een fout.
- Geannuleerd – is opbouwen geannuleerd.
- Annuleren – opbouwen wordt geannuleerd.


Houd er rekening mee dat de ID van de build u onder het volgende pad vindt:`Feed\entry\content\properties\Id`

XML-OData

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
    </entry>
    </feed>

###<a name="get-build-status-of-a-model"></a>Opbouwen status van een model ophalen

| HTTP-methode | URI |
|:--------|:--------|
|Toevoegen     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Voorbeeld:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|



|   Parameternaam  |   Geldige waarden                        |
|:--------          |:--------                              |
|   Model         |   Unieke id van het model (hoofdlettergevoelig)    |
|   onlyLastBuild   |   Geeft aan of retourneren de opbouwen geschiedenis van het model of alleen de status van de meest recente versie. |
|   apiVersion      |   1.0                                 |


**Antwoord**:

HTTP-statuscode: 200

Het antwoord bevat één item per opbouwen. Elk item heeft de volgende gegevens:

- `feed/entry/content/properties/UserName`– De naam van de gebruiker.
- `feed/entry/content/properties/ModelName`– De naam van het model.
- `feed/entry/content/properties/ModelId`– Model unieke id.
- `feed/entry/content/properties/IsDeployed`– Of het bouwen (ook wordt geïmplementeerd actieve build).
- `feed/entry/content/properties/BuildId`– De unieke id maken.
- `feed/entry/content/properties/BuildType`-Type de opbouwen.
- `feed/entry/content/properties/Status`– Status maken. Een van de volgende handelingen uit: fout, bouwen, in wachtrij, Cancelling, opgezegd, succes
- `feed/entry/content/properties/StatusMessage`– Gedetailleerde statusbericht (geldt alleen voor specifieke Staten).
- `feed/entry/content/properties/Progress`– Bouwen voortgang (%).
- `feed/entry/content/properties/StartTime`– Begintijd van opbouwen.
- `feed/entry/content/properties/EndTime`– Eindtijd maken.
- `feed/entry/content/properties/ExecutionTime`– Duur maken.
- `feed/entry/content/properties/ProgressStep`– Details over de huidige fase die een opbouwen bezig is in.

Geldige opbouwen status:
- Gemaakt – is de vermelding van de Build-verzoek gemaakt.
- In de wachtrij – aanvraag voor het samenstellen is geactiveerd en deze in de wachtrij is.
- Building – opbouwen wordt uitgevoerd.
- Success – opbouwen is beëindigd.
- Fout – opbouwen beëindigd met een fout.
- Geannuleerd – is opbouwen geannuleerd.
- Annuleren – opbouwen wordt geannuleerd.

Geldige waarden voor opbouwen type:
- Rang - rang maken. (Voor rang maken details, raadpleegt u het document "Machine Learning aanbeveling API documentatie")
- Aanbeveling - aanbeveling opbouwen.
- FBT - vaak samen opbouwen hebt gekocht.

XML-OData

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


###<a name="get-recommendations"></a>Aanbevelingen krijgen

| HTTP-methode | URI |
|:--------|:--------|
|Toevoegen     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Voorbeeld:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|



|   Parameternaam  |   Geldige waarden                        |
|:--------          |:--------                              |
| Model | Unieke id van het model (hoofdlettergevoelig) |
| ItemID 's | Door komma's gescheiden lijst met de te aanraden voor items.<br>Maximale lengte: 1024 |
| numberOfResults | Het aantal vereiste resultaten |
| includeMetatadata | Toekomstig gebruik, altijd false |
| apiVersion | 1.0 |

**Antwoord:**

HTTP-statuscode: 200

Het antwoord bevat één item per aanbevolen item. Elk item heeft de volgende gegevens:

- `Feed\entry\content\properties\Id`-Aanbevolen item-ID.
- `Feed\entry\content\properties\Name`-Naam van het item.
- `Feed\entry\content\properties\Rating`-Classificatie van de aanbeveling; hogere waarde betekent dat er hogere betrouwbaarheid.
- `Feed\entry\content\properties\Reasoning`-Aanbeveling redeneren (bijvoorbeeld aanbeveling uitleg).

XML-OData

Het onderstaande voorbeeld-antwoord bevat 10 aanbevolen artikelen:

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="update-model"></a>Model bijwerken
U kunt bijwerken, de Modelbeschrijving van de of de actieve opbouwen-ID.
*Actieve opbouwen-ID* - elke opbouwen voor elk model heeft een opbouwen-ID. ID van de actieve build is de eerste succesvolle build van elk nieuw model. Nadat u een actieve opbouwen-ID hebt en u aanvullende genereert voor hetzelfde model, moet u deze expliciet instellen als de ID van de standaard-build als u wilt. Wanneer u aanbevelingen, gebruiken als u de ID van de build die u wilt gebruiken niet opgeeft, wordt standaard een automatisch worden gebruikt.

Deze methode kunt u - wanneer u een model aanbeveling in productie hebt-nieuwe modellen bouwt en test u ze voordat u ze tot productie promoveren.

| HTTP-methode | URI |
|:--------|:--------|
|PLAATSEN     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Voorbeeld:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|


|   Parameternaam  |   Geldige waarden                        |
|:--------          |:--------                              |
| ID | Unieke id van het model (hoofdlettergevoelig) |
| apiVersion | 1.0 |
|||
| Hoofdtekst aanvragen | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br>Houd er rekening mee dat de XML-codes beschrijving en ActiveBuildId optioneel zijn. Als u niet instellen wilt, beschrijving of ActiveBuildId, moet u de hele tag verwijderen. |

**Antwoord**:

HTTP-statuscode: 200

XML-OData

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Update an Existing Model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
    <rights type="text" />
     <updated>2014-10-05T10:27:17Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

##<a name="legal"></a>Juridische
In dit document is opgegeven "als-is '. Gegevens en inzichten die in dit document, inclusief URL's en andere verwijzingen naar websites, kunnen zonder kennisgeving worden gewijzigd. Hier beschreven voorbeelden zijn opgegeven voor alleen de afbeelding en zijn alle. Geen echte koppeling of de verbinding is bedoeld en moet worden afgeleid. In dit document biedt u geen geen juridische rechten op een intellectueel eigendom in een Microsoft-product. U mag kopiëren en gebruiken van dit document voor interne referentiedoeleinden. © Microsoft 2014. Alle rechten gereserveerd. 
 
