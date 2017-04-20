<properties
pageTitle="Azure-blobopslag met Azure Search indexeren"
description="Meer informatie over het indexeren van Azure-blobopslag en tekst extraheren uit documenten met Azure zoeken"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="10/16/2016"
ms.author="eugenesh" />

# <a name="indexing-documents-in-azure-blob-storage-with-azure-search"></a>Documenten in Azure-blobopslag met Azure Search indexeren

In dit artikel wordt uitgelegd hoe u Azure zoeken naar index documenten (zoals PDF-bestanden, Microsoft Office-documenten en verschillende andere veelgebruikte indelingen) die zijn opgeslagen in Azure-blobopslag. De nieuwe zoeken Azure blob indexering gaat dit snel en naadloze. 

> [AZURE.IMPORTANT] Deze functie is momenteel in de proefversie. Dit is alleen beschikbaar in de REST API gebruik versie **2015-02-28-Preview**. Neem onthouden, voorbeeld API's zijn bedoeld voor het testen en evaluatie en mag niet worden gebruikt in productieomgevingen.

## <a name="supported-document-formats"></a>Ondersteunde documentindelingen

De indexering blob kunt tekst extraheren uit de volgende documentindelingen:

- PDF-BESTAND
- Microsoft Office-indelingen: DOCX/DOC, XLSX/XLS, PPTX/PPT, MSG (Outlook e-mailberichten)  
- HTML
- XML
- POSTCODE
- EML
- Bestanden voor tekst zonder opmaak  
- JSON (Zie [indexeren JSON BLOB's](search-howto-index-json-blobs.md) voor meer informatie)
- CSV (Zie [indexeren CSV-BLOB's](search-howto-index-csv-blobs.md) voor meer informatie)

## <a name="setting-up-blob-indexing"></a>Bij het instellen van blob indexeren

U kunt instellen dat een Azure-blobopslag indexering gebruiken:

- [Azure-portal](https://ms.portal.azure.com)
- Azure zoeken [REST API](https://msdn.microsoft.com/library/azure/dn946891.aspx)
- Azure zoeken .NET SDK [versie 2.0-voorbeeld](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) 

> [AZURE.NOTE] Sommige functies (bijvoorbeeld veldtoewijzingen) nog niet beschikbaar in de portal en via programmacode worden gebruikt. 

In dit artikel wordt de REST-API gebruiken door drie stappen te volgen voor een indexering stelt: een gegevensbron maken, een index maken en configureren van de indexering.

### <a name="step-1-create-a-data-source"></a>Stap 1: Een gegevensbron maken

Welke gegevens worden indexeert, referenties die nodig zijn voor toegang tot de gegevens, en het beleid efficiënt identificeren wijzigingen in de gegevens (nieuwe, gewijzigde of verwijderde rijen) Hiermee geeft u een gegevensbron. Een gegevensbron kan worden gebruikt door meerdere indexeerfuncties in de dezelfde search-service.

Voor blob indexeren, moet de gegevensbron de volgende vereiste eigenschappen hebben: 

- **de naam** is de unieke naam van de gegevensbron binnen uw search-service. 

- **type** moet `azureblob`. 

- **referenties** biedt de verbindingsreeks van de opslag-account als de `credentials.connectionString` parameter. U kunt de verbindingsreeks ophalen uit de Azure-portal door te schuiven naar het gewenste opslag account blad > **Instellingen** > **sleutels** en gebruik de waarde "Primaire verbindingsreeks" of "Secundaire verbindingsreeks". 

- **container** Hiermee geeft u een container in uw account opslag. Standaard zijn alle BLOB's in de container worden opgehaald. Als u index BLOB's in een bepaalde virtuele map alleen wilt, kunt u die map met de parameter optioneel **query** opgeven. 

Het volgende voorbeeld ziet u een definitie van de gegevensbron:

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Zie voor meer informatie over de gegevensbron maken API [Gegevensbron maken](search-api-indexers-2015-02-28-preview.md#create-data-source).

### <a name="step-2-create-an-index"></a>Stap 2: Een index maken 

De index Hiermee geeft u de velden in een document, kenmerken en andere constructies die het zoeken van vorm ervaring.  

Blob indexeren, moet u ervoor zorgen dat uw index een doorzoekbare heeft `content` veld voor het opslaan van de blob.

    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-target-index",
        "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
        ]
    }

Zie voor meer informatie over de Index maken API [Index maken](https://msdn.microsoft.com/library/dn798941.aspx)

### <a name="step-3-create-an-indexer"></a>Stap 3: Een indexering maken 

Een indexering gegevensbronnen maakt een verbinding met doel zoekindexen en planning informatie bevat, zodat u het vernieuwen van gegevens kunt automatiseren. Nadat de index en de gegevensbron zijn gemaakt, worden het relatief eenvoudig te maken van een indexering dat verwijst naar de gegevensbron en een doel-index. Bijvoorbeeld:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Deze indexering uitgevoerd om twee uur (schema-interval is ingesteld op "PT2H"). Als u wilt uitvoeren van een indexering instellen elke 30 minuten, het interval aan de 'PT30M'. Kortste ondersteunde interval is 5 minuten. Planning is optioneel - indien weggelaten, een indexering slechts één keer wordt uitgevoerd wanneer gemaakt. U kunt echter een indexering op aanvraag uitvoeren op elk gewenst moment.   

Voor meer informatie over de API voor indexering maken, raadpleegt u [Indexering maken](search-api-indexers-2015-02-28-preview.md#create-indexer).


## <a name="document-extraction-process"></a>De extractie van voor een documentenset

Azure zoeken indexeert elk document (blob) als volgt:

- De inhoud van de hele tekst van het document wordt geëxtraheerd in een tekenreeksveld met de naam `content`. Wij bieden momenteel geen ondersteuning voor het extraheren van meerdere documenten van een enkel blob:
    - Een CSV-bestand wordt bijvoorbeeld geïndexeerd als één document. Als u elke regel in een CSV als een afzonderlijk document moeten worden behandeld moet, stemmen op [de suggestie voor deze UserVoice](https://feedback.azure.com/forums/263029-azure-search/suggestions/13865325-please-treat-each-line-in-a-csv-file-as-a-separate).
    - Een samengestelde of ingesloten document (zoals een ZIP-archief of een Word-document met ingesloten de e-mail van Outlook met bijlagen) wordt ook als één document geïndexeerd.

- Gebruiker opgegeven metagegevenseigenschappen presenteren op de blob, worden indien van toepassing, geëxtraheerd woordelijk. De eigenschappen van metagegevens kunnen ook worden gebruikt om te bepalen van bepaalde aspecten van het document de extractie van proces: Bekijk van [Aangepaste-metagegevens aan de extractie van het Document van besturingselement gebruiken](#CustomMetadataControl) voor meer informatie.

- Standaard blob metagegevenseigenschappen worden uitgepakt in de volgende velden:

    - **metagegevens\_opslag\_naam** (Edm.String) - bestandsnaam van de blob. Als er een /my-container/my-folder/subfolder/resume.pdf blob, de waarde van dit veld is bijvoorbeeld `resume.pdf`.

    - **metagegevens\_opslag\_pad** (Edm.String) - de volledige URI van de blob, inclusief de opslag-account. Bijvoorbeeld:`https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`

    - **metagegevens\_opslag\_inhoud\_type** (Edm.String) - inhoudstype volgens de code die u hebt gebruikt voor het uploaden van de blob opgegeven. Bijvoorbeeld `application/octet-stream`.

    - **metagegevens\_opslag\_laatste\_gewijzigd** (Edm.DateTimeOffset) - tijdstempel voor de blob laatst is gewijzigd. Azure zoeken wordt dit tijdstempel gewijzigde BLOB's, om te voorkomen dat alles opnieuw indexeren na de eerste indexering.

    - **metagegevens\_opslag\_grootte** (Edm.Int64) - blob-grootte in bytes.

    - **metagegevens\_opslag\_inhoud\_md5** (Edm.String) - MD5-hash van de inhoud van de blob, indien beschikbaar.

- Eigenschappen van metagegevens specifiek zijn voor elke documentindeling zijn uitgepakt in de velden vermelden [hier](#ContentSpecificMetadata).

U hoeft niet te definiëren van velden voor alle bovenstaande eigenschappen in de zoekindex - alleen de eigenschappen die u nodig voor uw toepassing hebt vastleggen. 

> [AZURE.NOTE] Vaak zullen de veldnamen in de bestaande index afwijken van de veldnamen die tijdens de extractie van het document gegenereerd. **Veldtoewijzingen** kunt u de namen van de eigenschappen verstrekt door Azure zoeken naar de veldnamen in de zoekindex toewijzen. Hier ziet u een voorbeeld van het veld toewijzingen gebruik onder. 

## <a name="picking-the-document-key-field-and-dealing-with-different-field-names"></a>Het sleutelveld van het document te kiezen en omgaan met verschillende veldnamen

In Azure zoeken aanduiding de toets document een unieke voor een document. Elke zoekindex moet precies één sleutelveld van het type Edm.String hebben. Het veld sleutel is vereist voor elk document dat wordt toegevoegd aan de index (dit is dat is wel het enige vereist veld).  
   
Overweeg welke opgehaalde veld moet worden toegewezen aan het sleutelveld voor de Zijtabs met letter. De kandidaten zijn:

- **metagegevens\_opslag\_naam** - dit is een handige candidate mogelijk, maar notitie dat 1) de namen niet uniek zijn mogelijk, zoals u BLOB's met dezelfde naam in aparte mappen en de naam van 2) de wellicht tekens die ongeldig in document toetsen, zoals streepjes zijn kan bevatten. U kunt ongeldige tekens verwerken met behulp van de `base64Encode` [veldtoewijzing functie](search-indexer-field-mappings.md#base64EncodeFunction) - als u dit doet, moet u document toetsen coderen wanneer deze worden doorgegeven in API zoals opzoeken belt. (Bijvoorbeeld in .NET kunt u de [methode UrlTokenEncode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) daartoe).

- **metagegevens\_opslag\_pad** - het volledige pad, zorgt u ervoor uniekheid, maar het pad bevat groeiende `/` tekens die [in een documentsleutel ongeldig](https://msdn.microsoft.com/library/azure/dn857353.aspx)zijn.  Als hierboven, hebt u de optie voor het coderen van de toetsen gebruiken van de `base64Encode` [functie](search-indexer-field-mappings.md#base64EncodeFunction).

- Als geen van de bovenstaande opties voor u werkt, kunt u een aangepaste metagegevenseigenschap toevoegen aan de BLOB's. Deze optie is, het proces voor het uploaden van blob die metagegevenseigenschap toevoegen aan alle BLOB's echter vereist. Aangezien de sleutel een eigenschap vereist is, worden alle BLOB's waarvoor geen die eigenschap, mislukt moeten worden geïndexeerd.

> [AZURE.IMPORTANT] Als er geen expliciete toewijzing voor het veld sleutel in de index, wordt automatisch Azure zoeken gebruikt `metadata_storage_path` als de sleutel en base 64 gecodeerd sleutelwaarden (de tweede optie hierboven).

In dit voorbeeld kiest u laten we de `metadata_storage_name` veld als de documentsleutel. Stel ook dat uw index heeft een sleutelveld met de naam `key` en een veld `fileSize` voor het opslaan van de grootte van het document. Als u wilt wire wat naar wens, geeft u het volgende veldtoewijzingen bij het maken of bijwerken van uw indexering:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

Om dit weer te geven alle vormen, hier ziet u hoe u kunt veldtoewijzingen toevoegen en base 64-codering van sleutels voor een bestaande indexering:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }

> [AZURE.NOTE] Zie voor meer informatie over het toewijzen van een veld, [in dit artikel](search-indexer-field-mappings.md).

## <a name="incremental-indexing-and-deletion-detection"></a>Incrementele detectie van indexering en verwijderen

Wanneer u een indexering blob ingesteld om uit te voeren op een planning, het opnieuw indexeren van alleen de gewijzigde BLOB's, afhankelijk van de van de blob `LastModified` tijdstempel.

> [AZURE.NOTE] U hoeft niet te geven van een wijziging detectie beleid: incrementele indexeren automatisch voor u is ingeschakeld.

Als u wilt verwijderen documenten wordt ondersteund, gebruikt u een 'vloeiende verwijderen'-methode. Als u de rechtstreekse BLOB's verwijdert, bijbehorende documenten niet verwijderd uit de zoekindex. Gebruik in plaats daarvan de volgende stappen uit:  

1. Een aangepaste metagegevenseigenschap toevoegen aan de blob om aan te geven aan Azure zoeken dat deze logisch is verwijderd
2. Een beleid voor vloeiende verwijdering-detectie configureren op de gegevensbron
3. Zodra de indexering de blob (zoals u door de status indexering API) verwerkt heeft, kunt u fysiek de blob verwijderen

Bijvoorbeeld het volgende beleid een blob worden verwijderd als er een metagegevenseigenschap acht `IsDeleted` met de waarde `true`:

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",   
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }   

<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>Eigenschappen van metagegevens voor specifieke inhoud

De volgende tabel bevat een overzicht van verwerking klaar voor elke documentindeling en wordt de eigenschappen van metagegevens uitgepakt met zoeken in Azure beschreven.

Documentindeling / inhoudstype | Specifieke metagegevenseigenschappen van inhoudstype | Verwerking details
-------------------------------|-------------------------------------------|-------------------
HTML (`text/html`) | `metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` | HTML-opmaak verwijderen en tekst extraheren
PDF (`application/pdf`) | `metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title`| Tekst, inclusief ingesloten documenten (exclusief afbeeldingen) extraheren
DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` | Tekst, inclusief ingesloten documenten extraheren
DOC (toepassing/msword) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` | Tekst, inclusief ingesloten documenten extraheren
XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` | Tekst, inclusief ingesloten documenten extraheren
XLS (toepassing/vnd.ms-excel) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` | Tekst, inclusief ingesloten documenten extraheren
PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` | Tekst, inclusief ingesloten documenten extraheren
In PowerPoint (toepassing/vnd.ms-powerpoint) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` | Tekst, inclusief ingesloten documenten extraheren
MSG (toepassing/vnd.ms-outlook) | `metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` | Tekst, inclusief bijlagen extraheren
ZIP (toepassing/zip) | `metadata_content_type` | Tekst extraheren uit alle documenten in het archief
XML (toepassing/xml) | `metadata_content_type`</br>`metadata_content_encoding`</br> | Verwijderen van XML-opmaak en tekst extraheren
JSON (toepassing/json) | `metadata_content_type`</br>`metadata_content_encoding` | Tekst extraheren<br/>Opmerking: Als u nodig hebt voor het uitpakken van meerdere documentvelden uit een blob JSON, raadpleegt u [JSON indexeren BLOB's](search-howto-index-json-blobs.md) voor meer informatie
EML (bericht/rfc822) | `metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` | Tekst, inclusief bijlagen extraheren
Tekst zonder opmaak (tekst/zonder opmaak) | `metadata_content_type`</br>`metadata_content_encoding`</br> | 

<a name="CustomMetadataControl"></a>
## <a name="using-custom-metadata-to-control-document-extraction"></a>Het gebruik van aangepaste metagegevens naar de extractie van het document van besturingselement

U kunt de eigenschappen van metagegevens toevoegen aan een blob bepaalde aspecten van het proces blob indexeren en document extractie te bepalen. De volgende eigenschappen worden momenteel ondersteund:

Naam van eigenschap | Waarde onroerend goed | Uitleg
--------------|----------------|------------
AzureSearch_Skip | "true" | Hiermee geeft u voor de indexering blob volledig kunt overslaan de blob; de extractie van metagegevens noch inhoud wordt geprobeerd. Dit is handig als u wilt overslaan bepaalde typen inhoud of wanneer een bepaalde blob herhaaldelijk mislukt en de indexing proces wordt onderbroken.
AzureSearch_SkipContent | "true" | Hiermee geeft u voor de indexering blob moeten worden geïndexeerd alleen de metagegevens en slaat u uitgepakt inhoud van de blob. Dit is handig als de blob-inhoud niet interessant is, maar u nog steeds wilt indexeren de metagegevens die zijn bijgevoegd bij de blob.

<a name="IndexerParametersConfigurationControl"></a>
## <a name="using-indexer-parameters-to-control-document-extraction"></a>Indexering parameters gebruiken om te bepalen de extractie van het document

Er zijn verschillende indexering configuratie parameters zijn beschikbaar om te bepalen welke BLOB's en welke onderdelen van de inhoud en de metagegevens van een blob geïndexeerd. 

### <a name="index-only-the-blobs-with-specific-file-extensions"></a>Alleen de BLOB's met de specifieke bestandsextensie indexeren

U kunt alleen de BLOB's met de bestandsextensies die u met behulp van opgeeft de index de `indexedFileNameExtensions` indexering configuratieparameter. De waarde is een tekenreeks met een door komma's gescheiden lijst met bestandsextensies (met een voorafgaande punt). Bijvoorbeeld, moeten worden geïndexeerd alleen de. PDF-bestand en. DOCX BLOB's, als volgt: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions-from-indexing"></a>BLOB's met de specifieke bestandsextensie uitsluiten van het indexeren

U kunt BLOB's met specifieke bestandsnaamextensies uitsluiten van het indexeren met behulp van de `excludedFileNameExtensions` configuratieparameter. De waarde is een tekenreeks met een door komma's gescheiden lijst met bestandsextensies (met een voorafgaande punt). Bijvoorbeeld, moeten worden geïndexeerd alle BLOB's behalve de met de. PNG en. JPEG extensies, als volgt: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Als beide `indexedFileNameExtensions` en `excludedFileNameExtensions` parameters aanwezig zijn, Azure zoeken eerst kijkt naar `indexedFileNameExtensions`, vervolgens op `excludedFileNameExtensions`. Dit betekent dat als dezelfde bestandsextensie in beide lijsten aanwezig is, het bestandstype wordt uitgesloten van indexering. 

### <a name="index-storage-metadata-only"></a>Alleen index opslag-metagegevens

U kunt alleen de metagegevens van de opslagruimte indexeren en overslaan het document de extractie van proces gebruikt de `indexStorageMetadataOnly` configuratie-eigenschap. Dit is handig wanneer u niet nodig hebt voor de documentinhoud, moet u een van de eigenschappen van inhoud type / regiospecifieke metagegevens. Klik hiertoe instellen de `indexStorageMetadataOnly` eigenschap `true`: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexStorageMetadataOnly" : true } }
    }

### <a name="index-both-storage-and-content-type-metadata-but-skip-content-extraction"></a>Index zowel opslag als inhoudstype metagegevens, maar de extractie van inhoud overslaan

Als u alle de metagegevens uitpakken maar overslaan extractie van de inhoud voor alle BLOB's wilt, kunt u dit probleem met de configuratie indexering, in plaats van hoeft toe te voegen aanvragen `AzureSearch_SkipContent` metagegevens op elk afzonderlijk blob. Klik hiertoe instellen de `skipContent` indexering configuratie-eigenschap naar `true`: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "skipContent" : true } }
    }

## <a name="help-us-make-azure-search-better"></a>Help ons te verbeteren Azure zoeken

Als u functieverzoeken verzenden of ideeën voor de verbeteringen hebt, neem contact maken met ons op onze [UserVoice-site](https://feedback.azure.com/forums/263029-azure-search/).
