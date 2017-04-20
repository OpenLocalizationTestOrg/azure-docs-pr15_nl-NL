<properties
    pageTitle="De verbindingslijn vak toevoegen aan uw Apps logica | Microsoft Azure"
    description="Overzicht van het vak verbindingslijn met REST API parameters"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-box-connector"></a>Aan de slag met de verbindingslijn vak
Verbinding maken met het vak en maak bestanden, bestanden verwijderen en meer. 

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2015-08-01-schema voorbeeldversie.

Met vak kunt u het volgende doen:

- De bedrijfswerkstroom van uw op basis van de gegevens die u via het vak openen maken. 
- Gebruik triggers wanneer een bestand wordt gemaakt of bijgewerkt.
- Acties die een bestand kopiëren en verwijderen van een bestand, en meer gebruiken. Deze acties bent u een reactie en klikt u vervolgens de uitvoer beschikbaar maken voor andere acties. Bijvoorbeeld wanneer een bestand wordt gewijzigd in vak, kunt u uitvoeren van dat bestand en e-mailen met behulp van Office 365.

Als u wilt een bewerking in logica apps toevoegen, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties
Vak bevat de volgende trigger en acties.

| Triggers | Acties|
| --- | --- |
|<ul><li>Wanneer een bestand is gemaakt</li><li>Wanneer een bestand wordt gewijzigd</li></ul> | <ul><li>Bestand maken</li><li>Wanneer een bestand is gemaakt</li><li>Bestand kopiëren</li><li>Bestand verwijderen</li><li>Extraheren archief naar map</li><li>Bestand met behoud van id ophalen</li><li>Bestand met behoud van pad ophalen</li><li>Metagegevens van bestand met-id ophalen</li><li>Metagegevens van bestand met pad ophalen</li><li>Bestand bijwerken</li><li>Wanneer een bestand wordt gewijzigd</li></ul>

Alle verbindingslijnen ondersteuning voor gegevens in JSON en XML-indelingen.

## <a name="create-a-connection-to-box"></a>Een verbinding maken met het vak
Wanneer u deze connector aan uw apps logica toevoegt, moet u logica apps verbinding maken met het vak toestaan.

>[AZURE.INCLUDE [Steps to create a connection to box](../../includes/connectors-create-api-box.md)]

Nadat u de verbinding hebt gemaakt, kunt u de eigenschappen van het vak invoeren. De **REST API verwijzing** in dit onderwerp worden deze eigenschappen.

>[AZURE.TIP] U kunt deze dezelfde vak verbinding gebruiken in andere apps logica.

## <a name="swagger-rest-api-reference"></a>Swagger REST API verwijzing
Dit geldt voor versie: 1.0.

### <a name="create-file"></a>Bestand maken
Een bestand naar het vak geüpload.  
```POST: /datasets/default/files```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Mappad|tekenreeks|Ja|query|Geen |Pad naar het bestand uploaden naar het vak|
|naam|tekenreeks|Ja|query|Geen |Naam van het bestand te maken in vak|
|hoofdtekst|String(Binary) |Ja|hoofdtekst|Geen |Inhoud van het bestand uploaden naar het vak|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


### <a name="when-a-file-is-created"></a>Wanneer een bestand is gemaakt
De gebeurtenis een stroom wanneer een nieuw bestand is gemaakt in een map vak.  
```GET: /datasets/default/triggers/onnewfile```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|map-id|tekenreeks|Ja|query|Geen |Unieke id van de map in vak|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


### <a name="copy-file"></a>Bestand kopiëren
Een bestand kopiëren naar vak.  
```POST: /datasets/default/copyFile```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|bron|tekenreeks|Ja|query|Geen |URL naar bronbestand|
|bestemming|tekenreeks|Ja|query| Geen|Het bestandspad bestemming in vak, met inbegrip van doel-bestandsnaam|
|overschrijven|Booleaanse waarde|Nee|query| Geen|Het doelbestand overschrijft als ingesteld op 'waar'|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


### <a name="delete-file"></a>Bestand verwijderen
Hiermee verwijdert u een bestand in het vak.  
```DELETE: /datasets/default/files/{id}```


| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad|Geen |Unieke id van het bestand verwijderen uit het vak|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


### <a name="extract-archive-to-folder"></a>Extraheren archief naar map
Een archiefbestand haalt in een andere map in het vak (voorbeeld: .zip).  
```POST: /datasets/default/extractFolderV2```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|bron|tekenreeks|Ja|query| |Pad naar het archiefbestand|
|bestemming|tekenreeks|Ja|query| |Pad in vak om de inhoud archief extraheren|
|overschrijven|Booleaanse waarde|Nee|query| |Overschrijft de doelbestanden als ingesteld op 'waar'|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


### <a name="get-file-content-using-id"></a>Bestand met behoud van id ophalen
Bestandsinhoud opgehaald uit vak met-id.  
```GET: /datasets/default/files/{id}/content```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad|Geen |Unieke id van het bestand in vak|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


### <a name="get-file-content-using-path"></a>Bestand met behoud van pad ophalen
Bestandsinhoud opgehaald uit vak pad.  
```GET: /datasets/default/GetFileContentByPath```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|pad|tekenreeks|Ja|query|Geen |Unieke pad naar het bestand in vak|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


### <a name="get-file-metadata-using-id"></a>Metagegevens van bestand met-id ophalen
Bestand metagegevens opgehaald uit vak met bestand-id.  
```GET: /datasets/default/files/{id}```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad| Geen|Unieke id van het bestand in vak|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


### <a name="get-file-metadata-using-path"></a>Metagegevens van bestand met pad ophalen
Bestand metagegevens opgehaald uit vak pad.  
```GET: /datasets/default/GetFileByPath```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|pad|tekenreeks|Ja|query|Geen |Unieke pad naar het bestand in vak|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


### <a name="update-file"></a>Bestand bijwerken
Een bestand in het vak worden bijgewerkt.  
```PUT: /datasets/default/files/{id}```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad| Geen|Unieke id van het bestand wilt bijwerken in het vak|
|hoofdtekst|String(Binary) |Ja|hoofdtekst|Geen |Inhoud van het bestand wilt bijwerken in het vak|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


### <a name="when-a-file-is-modified"></a>Wanneer een bestand wordt gewijzigd
De gebeurtenis een stroom wanneer een bestand wordt gewijzigd in een map vak.  
```GET: /datasets/default/triggers/onupdatedfile```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|map-id|tekenreeks|Ja|query|Geen |Unieke id van de map in vak|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="object-definitions"></a>Objectdefinities

#### <a name="datasetsmetadata"></a>DataSetsMetadata

|Naam van eigenschap | Gegevenstype | Vereist|
|---|---|---|
|in tabelvorm|niet gedefinieerd|geen|
|BLOB|niet gedefinieerd|geen|

#### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|bron|tekenreeks|geen|
|Weergavenaam|tekenreeks|geen|
|urlEncoding|tekenreeks|geen|
|tableDisplayName|tekenreeks|geen|
|tablePluralName|tekenreeks|geen|

#### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|bron|tekenreeks|geen|
|Weergavenaam|tekenreeks|geen|
|urlEncoding|tekenreeks|geen|

#### <a name="blobmetadata"></a>BlobMetadata

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|Naam|tekenreeks|geen|
|Weergavenaam|tekenreeks|geen|
|Pad|tekenreeks|geen|
|LastModified|tekenreeks|geen|
|Grootte|geheel getal|geen|
|MediaType|tekenreeks|geen|
|IsFolder|Booleaanse waarde|geen|
|ETag|tekenreeks|geen|
|FileLocator|tekenreeks|geen|

## <a name="next-steps"></a>Volgende stappen

[Een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).
