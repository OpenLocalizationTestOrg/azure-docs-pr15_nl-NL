<properties 
    pageTitle="Hoe u het coderen van activa met Media Encoder standaard | Microsoft Azure" 
    description="Informatie over het gebruik van de Media Encoder standaard voor het coderen van de mediainhoud op Media Services. Voorbeelden van code REST API gebruiken." 
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
    ms.date="09/19/2016"
    ms.author="juliako"/>


#<a name="how-to-encode-an-asset-using-media-encoder-standard"></a>Hoe u het coderen van activa met Media Encoder standaard


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
- [REST](media-services-rest-encode-asset.md)
- [Portal](media-services-portal-encode.md)

##<a name="overview"></a>Overzicht
Voor het leveren van digitale video via internet, moet u de media comprimeren. Digitale videobestanden erg groot zijn en mogelijk te groot is om uit te voeren via internet of voor uw klanten apparaten goed weer te geven. Coderen, is het proces van het comprimeren van video en audio, zodat uw klanten uw media kunnen zien.

Tekstcodering taken zijn een van de meest voorkomende verwerking in Media Services. U maken codering taken als u wilt converteren media-bestanden van één codering naar een andere. Wanneer u coderen, kunt u de ingebouwde Services Media encoder die (Media Encoder standaard). U kunt ook een encoder verstrekt door een partner Media Services; derde partij encoders zijn beschikbaar via de Azure Marketplace. U kunt de details van taken coderen met behulp van vooraf ingestelde tekenreeksen die zijn gedefinieerd voor uw encoder of met behulp van vooraf ingestelde configuratiebestanden opgeven. De soorten vooraf ingestelde die beschikbaar zijn, raadpleegt u [Taak standaardinstellingen voor Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).

Elke taak kan hebben een of meer taken afhankelijk van het type verwerking die u wilt uitvoeren. Via de REST API, kunt u taken en hun verwante taken op twee manieren maken:

- Taken kunnen worden gedefinieerd inline tot en met de eigenschap navigatie van taken op taak entiteiten, of
- via OData batchbestand.


Het wordt aanbevolen voor het coderen van altijd uw mezzanine-bestanden in een geavanceerde bitsnelheid MP4 instellen en vervolgens de set converteren naar de gewenste indeling met de [Dynamische verpakking](media-services-dynamic-packaging-overview.md). Om te profiteren van dynamische verpakking, moet u eerst ten minste één op aanvraag streaming eenheid ophalen voor het streaming eindpunt waaruit u wilt gaan bezorging van uw inhoud. Lees [hoe u de schaal Media Services](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

Als uw activa uitvoer versleuteld opslag is, moet u activa bezorging beleid configureren. Zie [configureren activa bezorging beleid](media-services-rest-configure-asset-delivery-policy.md)voor meer informatie.


>[AZURE.NOTE]Voordat u verwijst naar media-processors begint, Controleer of u de juiste media processor-ID. Zie voor meer informatie [Krijgen Media-Processors](media-services-rest-get-media-processor.md).

##<a name="create-a-job-with-a-single-encoding-task"></a>Een taak maken met een enkele codering taak

>[AZURE.NOTE] Tijdens het werken met de Media Services REST API, zijn de volgende punten van toepassing:
>
>Bij het openen van entiteiten in Media Services, moet u specifieke koptekstvelden en waarden instellen in uw HTTP-aanvragen. Zie [configuratie van voor de ontwikkeling van Media Services REST API](media-services-rest-how-to-use.md)voor meer informatie.

>Nadat de verbinding tot stand naar https://media.windows.net, ontvangt u een 301 omleiding precisie van een andere Media-URI voor Services. Mislukken volgende oproepen naar de nieuwe URI zoals is beschreven in [verbinding maken met Media-Services REST API gebruiken](media-services-rest-connect-programmatically.md), moet u ervoor.
>
>Wanneer JSON en als u wilt gebruiken het trefwoord **__metadata** in het verzoek te geven (bijvoorbeeld naar verwijst naar een gekoppelde object) moet u de koptekst **accepteren** instellen naar [JSON uitgebreide indeling](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): accepteren: toepassing/json; odata = uitgebreide.

Het volgende voorbeeld ziet u hoe maken en publiceren van een project met een die taak instellen voor het coderen van een video op een bepaalde resolutie en de kwaliteit. Als coderen met Media Encoder Standard, kunt u taak configuratie vooraf ingestelde opgegeven [hier](http://msdn.microsoft.com/library/mt269960).

Vraag:

BERICHT https://media.windows.net/API/Jobs HTTP/1.1 inhoudstype: toepassing/json; odata = uitgebreide accepteren: toepassing/json; odata = uitgebreide DataServiceVersion: 3.0 MaxDataServiceVersion: 3.0 x-ms-versie: 2.11 autorisatie: vruchtdragende <token value> 
 x-ms-client-aanvraag-id: 00000000-0000-0000-0000-000000000000 Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "H264 Multiple Bitrate 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Antwoord:
    
    HTTP/1.1 201 Created

    . . . 

###<a name="set-the-output-assets-name"></a>Naam van het element uitvoer instellen

Het volgende voorbeeld ziet u hoe het kenmerk assetName instellen:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

##<a name="considerations"></a>Overwegingen

- TaskBody eigenschappen moeten letterlijke XML gebruiken om te definiëren het aantal invoer of uitvoer van activa die worden gebruikt door de taak. Het onderwerp van de taak bevat de XML-Schema-definitie voor de XML.
- In de definitie TaskBody elke binnenste waarde van <inputAsset> en <outputAsset> JobInputAsset(value) of JobOutputAsset(value) moet worden ingesteld.
- Een taak kan meerdere uitvoer activa hebben. Één JobOutputAsset(x) kunnen slechts één keer worden gebruikt als een uitvoer van een taak in een taak.
- U kunt JobInputAsset of JobOutputAsset opgeven als invoer activa van een taak.
- Taken moeten een cyclus geen vormen.
- De parameter value die u aan JobInputAsset of JobOutputAsset doorgeven geeft de indexwaarde voor activa. De werkelijke activa zijn gedefinieerd in de eigenschappen van de navigatie InputMediaAssets en OutputMediaAssets op de taak entiteitsdefinitie. 
- Omdat Media Services is gebaseerd op OData v3 en de afzonderlijke activa in InputMediaAssets en OutputMediaAssets navigatie eigenschap verzamelingen wordt verwezen door een ' __metadata: uri "paar naam-waarde.
- InputMediaAssets koppelt aan een of meer activa die u hebt gemaakt in Media Services. OutputMediaAssets worden gemaakt door het systeem. Ze niet verwijzen naar een bestaand activum.
- OutputMediaAssets kan de naam met het kenmerk assetName. Als dit kenmerk niet aanwezig is, is de naam van de OutputMediaAsset altijd ongeacht de binnenste tekstwaarde van de <outputAsset> element is met een achtervoegsel van de taaknaam waarde of de taak-Id-waarde (in het geval waar de naam van eigenschap wordt niet gedefinieerd). Bijvoorbeeld als u een waarde voor assetName "Steekproef" hebt ingesteld, klikt u vervolgens de OutputMediaAsset naam eigenschap zou zijn ingesteld op "Voorbeeld". Als u een waarde voor assetName niet hebt ingesteld, maar de taaknaam op 'NewJob' ingesteld, zou klikt u vervolgens de naam van de OutputMediaAsset wel "JobOutputAsset (waarde) _NewJob". 


##<a name="create-a-job-with-chained-tasks"></a>Een taak maken met gekoppelde taken

In veel toepassing-scenario's wilt ontwikkelaars maken van een reeks taken te verwerken. In Media-Services, kunt u een reeks gekoppelde taken maken. Elke taak verschillende verwerkingsstappen uitvoert en andere media processors kunt gebruiken. De gekoppelde taken kunnen overdragen activa uit één taak naar een andere, een lineaire reeks taken op de activa uit te voeren. De taken die zijn uitgevoerd in een project hoeven echter niet in een reeks weergeven. Wanneer u een gekoppelde taak maakt, wordt het gekoppelde **ITask** objecten zijn gemaakt in één **IJob** object.

>[AZURE.NOTE] Er is momenteel een limiet van 30 taken per taak. Als u nodig hebt om te koppelen van meer dan 30 taken, maakt u meer dan één taak om de taken te bevatten.


    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


###<a name="considerations"></a>Overwegingen

Inschakelen taak koppelen:

- Een taak moet ten minste 2 taken
- Er moet ten minste één taak waarvan invoer uitvoer van een andere taak in de taak is.

## <a name="use-odata-batch-processing"></a>OData-batchbestand gebruiken 

Het volgende voorbeeld ziet u hoe u OData-batchbestand gebruikt om een taak en taken te maken. Voor informatie over batchbestand, Zie [(OData) Open Data Protocol Batch-verwerking](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).
 
    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net
    
    
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--
 


## <a name="create-a-job-using-a-jobtemplate"></a>Een taak met een taaksjabloon maken


Tijdens het verwerken van meerdere activa met een gemeenschappelijke set met taken, zijn JobTemplates handig voor de volgorde van taken, vooraf ingestelde taak standaard geven enzovoort.

Het volgende voorbeeld wordt getoond hoe een taaksjabloon maken met een inline TaskTemplate gedefinieerd. De TaskTemplate gebruikt de Media Encoder standaard als de MediaProcessor voor het coderen van het bestand activa. andere MediaProcessors kan echter ook worden gebruikt. 


    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }
 

>[AZURE.NOTE]In tegenstelling tot andere entiteiten Media Services, moet u een nieuwe GUID-id voor elke TaskTemplate definiëren en in de taskTemplateId en de eigenschap Id in de hoofdtekst van uw aanvraag plaatsen. Het kleurenschema van de identificatie van inhoud moet voldoen aan het kleurenschema dat wordt beschreven in Azure Media Services entiteiten identificeren. Ook kan niet JobTemplates worden bijgewerkt. In plaats daarvan, moet u een nieuwe record met de bijgewerkte wijzigingen maken.
 

Als dit lukt, wordt het volgende antwoord geretourneerd:
    
    HTTP/1.1 201 Created
    
    . . .


Het volgende voorbeeld ziet u hoe een taak die verwijst naar een Id taaksjabloon wilt maken:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}
     

Als dit lukt, wordt het volgende antwoord geretourneerd:
    
    HTTP/1.1 201 Created
    
    . . . 



##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Volgende stappen
Als u weet hoe u een taak te coderen van een assset maakt, gaat u naar het onderwerp [Hoe naar taakvoortgang controleren met Media-Services](media-services-rest-check-job-progress.md) .


##<a name="see-also"></a>Zie ook

[Media-Processors ophalen](media-services-rest-get-media-processor.md)
