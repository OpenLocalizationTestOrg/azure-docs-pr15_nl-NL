<properties
    pageTitle="De Azure-blobopslag verbindingslijn in uw Apps logica toevoegen | Microsoft Azure"
    description="Overzicht van Azure-blobopslag verbindingslijn met REST API parameters"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="anneta"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-azure-blob-storage-connector"></a>Aan de slag met Azure blob storage connector
Azure-blobopslag is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens. Verschillende bewerkingen zoals uploaden, bijwerken en krijgen BLOB's in Azure-blobopslag verwijderen. 

Met Azure-blobopslag, u:

- Uw werkstroom maakt op uploaden van nieuwe projecten of aan de bestanden die onlangs zijn bijgewerkt.
- Acties gebruiken voor het ophalen van metagegevens bestand, verwijdert u een bestand, bestanden kopiëren en meer. Bijvoorbeeld wanneer een hulpmiddel in een Azure-website (een trigger) wordt bijgewerkt, klikt u vervolgens een bestand bijwerken in blobopslag (een actie). 

In dit onderwerp ziet u hoe u de blob storage verbindingslijn gebruiken in een app logica en worden ook de acties weergegeven.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica Apps algemene beschikbaarheid (GA). 

Meer informatie over de logica Apps, raadpleegt u [Wat zijn de logica apps](../app-service-logic/app-service-logic-what-are-logic-apps.md) en [een logica-app maakt](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-azure-blob-storage"></a>Verbinding maken met Azure-blobopslag

Voordat uw app logica toegang elke service tot, maakt u eerst een *verbinding* met de service. Een verbinding biedt connectiviteit tussen een logica-app en een andere service. Bijvoorbeeld als u wilt koppelen aan een account opslagruimte u eerst een blob opslag maken *verbinding*. Voer de referenties die u normaal gesproken gebruiken voor toegang tot de service die u verbinding met maakt een verbinding wilt maken. Met Azure opslag, voert u de referenties op bij uw account opslag om de verbinding te maken. 

#### <a name="create-the-connection"></a>De verbinding maken

>[AZURE.INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]
 
## <a name="use-a-trigger"></a>Gebruik een trigger

Deze verbindingslijn is geen eventuele triggers. Gebruik andere triggers de logica-app, zoals een terugkeerpatroon-trigger, een HTTP Webhook-trigger, triggers beschikbaar voor communicatie met andere verbindingslijnen en meer te starten. [Een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md) wordt een voorbeeld.

## <a name="use-an-action"></a>Gebruik een actie
    
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een app logica.

1. Selecteer het plusteken (+). Ziet u diverse opties selecteren: **een actie toevoegen**, **een voorwaarde toevoegen**of een van de **meer** opties.

    ![](./media/connectors-create-api-azureblobstorage/add-action.png)

2. Kies **een actie toevoegen**.

3. Typ "blob' voor een overzicht van alle beschikbare acties in het tekstvak.

    ![](./media/connectors-create-api-azureblobstorage/actions.png) 

4. Kies in ons voorbeeld **AzureBlob - bestand metagegevens met pad ophalen**. Als er al een verbinding bestaat, en selecteer vervolgens de **…** Knop (kiezer weergeven) om een bestand te selecteren.

    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)

    Als u wordt gevraagd om de verbindingsgegevens, voert u de details om de verbinding te maken. [De verbinding maken](connectors-create-api-azureblobstorage.md#create-the-connection) in dit onderwerp worden deze eigenschappen. 

    > [AZURE.NOTE] In dit voorbeeld wordt de metagegevens van een bestand ophalen. U wilt zien van de metagegevens, een andere actie die u maakt een nieuw bestand met een ander connector toevoegen. Bijvoorbeeld een OneDrive-actie die u een nieuw bestand maakt "testen" op basis van de metagegevens toevoegen. 

5. **Sla** uw wijzigingen (linkerbovenhoek van de werkbalk). Uw app logica is opgeslagen en mogelijk automatisch ingeschakeld.

> [AZURE.TIP] [Opslag Explorer](http://storageexplorer.com/) is een prima hulpmiddel voor het beheren van meerdere opslag-accounts.

## <a name="technical-details"></a>Technische Details

## <a name="storage-blob-actions"></a>Opslag Blob acties

|Actie|Beschrijving|
|--- | ---|
|[Bestand metagegevens ophalen](connectors-create-api-azureblobstorage.md#get-file-metadata)|Deze bewerking krijgt bestand metagegevens met bestand-id.|
|[Bestand bijwerken](connectors-create-api-azureblobstorage.md#update-file)|Een bestand door deze bewerking wordt bijgewerkt.|
|[Bestand verwijderen](connectors-create-api-azureblobstorage.md#delete-file)|Deze bewerking verwijdert een bestand.|
|[Metagegevens van bestand met pad ophalen](connectors-create-api-azureblobstorage.md#get-file-metadata-using-path)|Deze bewerking krijgt metagegevens van het bestand met het pad.|
|[Bestand met behoud van pad ophalen](connectors-create-api-azureblobstorage.md#get-file-content-using-path)|Deze bewerking wordt de inhoud van het bestand met het pad.|
|[Bestandsinhoud ophalen](connectors-create-api-azureblobstorage.md#get-file-content)|Deze bewerking wordt de inhoud van het bestand met-id.|
|[Bestand maken](connectors-create-api-azureblobstorage.md#create-file)|Een bestand wordt geüpload door deze bewerking.|
|[Bestand kopiëren](connectors-create-api-azureblobstorage.md#copy-file)|Deze bewerking wordt gekopieerd van een bestand met Azure-blobopslag.|
|[Extraheren archief naar map](connectors-create-api-azureblobstorage.md#extract-archive-to-folder)|Deze bewerking haalt een archiefbestand in een andere map (voorbeeld: .zip).|

### <a name="action-details"></a>Actiedetails

In deze sectie, de specifieke informatie over elke actie, inclusief eventuele verplicht of optioneel eigenschappen voor de invoer en alle bijbehorende uitvoer die is gekoppeld aan de verbindingslijn te bekijken.

#### <a name="get-file-metadata"></a>Bestand metagegevens ophalen
Deze bewerking krijgt bestand metagegevens met bestand-id.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|ID *|Bestand|Een bestand hebt geselecteerd|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Gegevens voor uitvoer
BlobMetadata

| Naam van eigenschap | Gegevenstype |
|---|---|
|ID|tekenreeks|
|Naam|tekenreeks|
|Weergavenaam|tekenreeks|
|Pad|tekenreeks|
|LastModified|tekenreeks|
|Grootte|geheel getal|
|MediaType|tekenreeks|
|IsFolder|Booleaanse waarde|
|ETag|tekenreeks|
|FileLocator|tekenreeks|


#### <a name="update-file"></a>Bestand bijwerken
Een bestand door deze bewerking wordt bijgewerkt.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|ID *|Bestand|Een bestand hebt geselecteerd|
|hoofdtekst *|De inhoud van bestand|Inhoud van het bestand wilt bijwerken|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Gegevens voor uitvoer
BlobMetadata

| Naam van eigenschap | Gegevenstype |
|---|---|
|ID|tekenreeks|
|Naam|tekenreeks|
|Weergavenaam|tekenreeks|
|Pad|tekenreeks|
|LastModified|tekenreeks|
|Grootte|geheel getal|
|MediaType|tekenreeks|
|IsFolder|Booleaanse waarde|
|ETag|tekenreeks|
|FileLocator|tekenreeks|


#### <a name="delete-file"></a>Bestand verwijderen
Deze bewerking verwijdert een bestand.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|ID *|Bestand|Een bestand hebt geselecteerd|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Gegevens voor uitvoer
Geen.


#### <a name="get-file-metadata-using-path"></a>Metagegevens van bestand met pad ophalen
Deze bewerking krijgt metagegevens van het bestand met het pad.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|pad *|Bestandspad|Een bestand hebt geselecteerd|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Gegevens voor uitvoer
BlobMetadata

| Naam van eigenschap | Gegevenstype |
|---|---|
|ID|tekenreeks|
|Naam|tekenreeks|
|Weergavenaam|tekenreeks|
|Pad|tekenreeks|
|LastModified|tekenreeks|
|Grootte|geheel getal|
|MediaType|tekenreeks|
|IsFolder|Booleaanse waarde|
|ETag|tekenreeks|
|FileLocator|tekenreeks|


#### <a name="get-file-content-using-path"></a>Bestand met behoud van pad ophalen
Deze bewerking wordt de inhoud van het bestand met het pad.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|pad *|Bestandspad|Een bestand hebt geselecteerd|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Gegevens voor uitvoer
Geen.


#### <a name="get-file-content"></a>Bestandsinhoud ophalen
Deze bewerking wordt de inhoud van het bestand met-id.  

|Naam van eigenschap| Gegevenstype|Beschrijving|
| ---|---|---|
|ID *|tekenreeks|Een bestand hebt geselecteerd|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Gegevens voor uitvoer
Geen.


#### <a name="create-file"></a>Bestand maken
Een bestand wordt geüpload door deze bewerking.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|Mappad *|Pad naar de map|Selecteer een map|
|naam *|Bestandsnaam|Naam van bestand uploaden|
|hoofdtekst *|De inhoud van bestand|Inhoud van het bestand te uploaden|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Gegevens voor uitvoer
BlobMetadata

| Naam van eigenschap | Gegevenstype | 
|---|---|
|ID|tekenreeks|
|Naam|tekenreeks|
|Weergavenaam|tekenreeks|
|Pad|tekenreeks|
|LastModified|tekenreeks|
|Grootte|geheel getal|
|MediaType|tekenreeks|
|IsFolder|Booleaanse waarde|
|ETag|tekenreeks|
|FileLocator|tekenreeks|


#### <a name="copy-file"></a>Bestand kopiëren
Deze bewerking wordt gekopieerd van een bestand met Azure-blobopslag.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|bron *|Bron-url|Geef de Url naar bronbestand|
|bestemming *|Bestandspad voor bestemming|Het bestandspad bestemming, met inbegrip van doel-bestandsnaam opgeven|
|overschrijven|Overschrijven?|Een bestaand doelbestand overschreven moet worden (waar/onwaar)?  |

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Gegevens voor uitvoer
BlobMetadata

| Naam van eigenschap | Gegevenstype |
|---|---|
|ID|tekenreeks|
|Naam|tekenreeks|
|Weergavenaam|tekenreeks|
|Pad|tekenreeks|
|LastModified|tekenreeks|
|Grootte|geheel getal|
|MediaType|tekenreeks|
|IsFolder|Booleaanse waarde|
|ETag|tekenreeks|
|FileLocator|tekenreeks|

#### <a name="extract-archive-to-folder"></a>Extraheren archief naar map
Deze bewerking haalt een archiefbestand in een andere map (voorbeeld: .zip).  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|bron *|Bestandspad voor bron archief|Selecteer een archiefbestand|
|bestemming *|Bestemming mappad|De inhoud kunt ophalen selecteren|
|overschrijven|Overschrijven?|Een bestaand doelbestand overschreven moet worden (waar/onwaar)?|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Gegevens voor uitvoer
BlobMetadata

| Naam van eigenschap | Gegevenstype |
|---|---|
|ID|tekenreeks|
|Naam|tekenreeks|
|Weergavenaam|tekenreeks|
|Pad|tekenreeks|
|LastModified|tekenreeks|
|Grootte|geheel getal|
|MediaType|tekenreeks|
|IsFolder|Booleaanse waarde|
|ETag|tekenreeks|
|FileLocator|tekenreeks|


## <a name="http-responses"></a>HTTP-antwoorden

Wanneer u oproepen naar de verschillende acties, krijgt u mogelijk bepaalde antwoorden. De volgende tabel bevat een overzicht van de antwoorden en de bijbehorende beschrijvingen:  

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|

## <a name="next-steps"></a>Volgende stappen

[Een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md). Verken de andere beschikbare connectors in logica-Apps op onze [lijst API's](apis-list.md).



