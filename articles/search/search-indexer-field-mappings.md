<properties
pageTitle="Veldtoewijzingen in indexeerfuncties Azure zoeken"
description="Azure zoeken indexering veldtoewijzingen voor verschillen tussen de namen van velden en gegevensweergaven configureren"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="10/17/2016"
ms.author="eugenesh" />

# <a name="field-mappings-in-azure-search-indexers"></a>Veldtoewijzingen in indexeerfuncties Azure zoeken

Wanneer u een Azure zoeken indexeerfuncties gebruikt, kunt u zelf af en toe in situaties waarin de invoergegevens niet helemaal overeenkomt met het schema van de doel-index vinden. In dat geval kunt u **de veldtoewijzingen van het** voor het omzetten van uw gegevens naar de gewenste vorm. 

Enkele situaties waarin het veldtoewijzingen nuttig zijn:
 
- Uw gegevensbron heeft een veld `_id`, maar Azure zoeken veldnamen die beginnen met een onderstrepingsteken niet is toegestaan. Een veldtoewijzing kunt u een veld 'naam'. 
- U wilt vullen verschillende velden in de index met dezelfde gegevens brongegevens, bijvoorbeeld omdat u wilt toepassen op verschillende analyzers die velden. Veldtoewijzingen kunnen u een bron gegevensveld zich ' splitsen'.
- U moet naar Base64 coderen of decoderen van uw gegevens. Veldtoewijzingen ondersteuning voor diverse **functies van de toewijzing**-functies voor Base64 coderen en decoderen.   


> [AZURE.IMPORTANT] Veld toewijzingen functionaliteit is momenteel in de proefversie. Dit is alleen beschikbaar in de REST API gebruik versie **2015-02-28-Preview**. Neem onthouden, voorbeeld API's zijn bedoeld voor het testen en evaluatie en mag niet worden gebruikt in productieomgevingen.

## <a name="setting-up-field-mappings"></a>Bij het instellen van het veldtoewijzingen

U kunt veldtoewijzingen toevoegen bij het maken van een nieuwe indexering met de API voor [Indexering maken](search-api-indexers-2015-02-28-preview.md#create-indexer) . U kunt de veldtoewijzingen van op een indexing indexering met de [Update voor indexering](search-api-indexers-2015-02-28-preview.md#update-indexer) API beheren. 

Een veldtoewijzing bestaat uit 3 onderdelen: 

1. A `sourceFieldName`, die een veld in de gegevensbron vertegenwoordigt. Deze eigenschap is vereist. 

2. Een optionele `targetFieldName`, die een veld in de zoekindex vertegenwoordigt. Indien weggelaten wordt dezelfde naam zoals in de gegevensbron gebruikt. 

3. Een optionele `mappingFunction`, waarin uw gegevens met behulp van een van de kunt transformeren vooraf gedefinieerde functies. De volledige lijst met functies is [hieronder](#mappingFunctions).

Toewijzingen van de velden worden toegevoegd aan de `fieldMappings` matrix op de definitie van indexering. 

Hier is bijvoorbeeld hoe u de verschillen tussen de veldnamen kan bevatten: 

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 
} 
```

Een indexering kan meerdere veldtoewijzingen bevatten. Hier volgt een voorbeeld van hoe u kunt "zich splitsen" een veld:

```JSON

"fieldMappings" : [ 
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }, 
] 
```

> [AZURE.NOTE] Azure zoeken wordt niet hoofdlettergevoelig vergelijking gebruikt voor het oplossen van het veld en functie namen in veldtoewijzingen. Dit is handig (u hoeft alle het omhulsel direct), maar dit betekent dat uw gegevensbron of index kan geen velden die alleen op basis van hoofdletters/kleine letters verschillen.  

<a name="mappingFunctions"></a>
## <a name="field-mapping-functions"></a>Veld toewijzing functies

Deze functies worden momenteel ondersteund: 

- [base64Encode](#base64EncodeFunction)
- [base64Decode](#base64DecodeFunction)
- [extractTokenAtPosition](#extractTokenAtPositionFunction)
- [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>
### <a name="base64encode"></a>base64Encode 

Voert de *URL geschikt* Base64-codering van de invoer tekenreeks. Wordt aangenomen dat de invoer UTF-8-codering. 

#### <a name="sample-use-case"></a>Voorbeeld use-case 

Alleen URL geschikt tekens kunnen worden weergegeven in een document-sleutel Azure zoeken (omdat klanten kunnen het document via de API opzoeken, zoals adres moeten). Als uw gegevens URL-onveilige tekens bevat en u wilt gebruiken om te vullen een sleutelveld in de zoekindex, gebruikt u deze functie.   

#### <a name="example"></a>Voorbeeld 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Path", 
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" } 
  }] 
```

<a name="base64DecodeFunction"></a>
### <a name="base64decode"></a>base64Decode

Voert Base64 decoderen van de invoer tekenreeks. De invoer wordt uitgegaan van een *URL-safe* tekenreeks Base64-codering. 

#### <a name="sample-use-case"></a>Voorbeeld use-case 

Aangepaste metagegevenswaarden die zijn BLOB moet ASCII-codering. U kunt Base64-codering gebruiken om aan te geven willekeurige Unicode-tekenreeksen in de aangepaste metagegevens blob. Als u wilt zoeken betekenisvolle, kunt u deze functie echter gebruiken de gecodeerde gegevens terug omzetten in "normale" tekenreeksen tijdens het vullen van de zoekindex.  

#### <a name="example"></a>Voorbeeld 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Base64EncodedMetadata", 
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" } 
  }] 
```

<a name="extractTokenAtPositionFunction"></a>
### <a name="extracttokenatposition"></a>extractTokenAtPosition

Een tekenreeksveld met het opgegeven scheidingsteken splitst, en het token uitgelicht op de opgegeven positie in de resulterende splitsen.

Als de invoer is bijvoorbeeld `Jane Doe`, de `delimiter` is `" "`(spatie) en de `position` 0 is, wordt het resultaat `Jane`; Als de `position` 1, is het resultaat is `Doe`. Als de positie naar een token die niet bestaat verwijst, wordt er een fout geretourneerd.

#### <a name="sample-use-case"></a>Voorbeeld use-case 

De gegevensbron bevat een `PersonName` veld en u wilt indexeren deze als twee aparte `FirstName` en `LastName` velden. Gebruik deze functie kunt u de invoer met spaties als scheidingsteken splitsen.

#### <a name="parameters"></a>Parameters

- `delimiter`: een tekenreeks die u wilt gebruiken als het scheidingsteken bij de ingevoerde tekenreeks splitsen.
- `position`: een geheel getal nul positie van het token als volgt te werk nadat de ingevoerde tekenreeks wordt gesplitst.    

#### <a name="example"></a>Voorbeeld

```JSON 

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } } 
  }, 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } } 
  }] 
```

<a name="jsonArrayToStringCollectionFunction"></a>
### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection

Een tekenreeks die is opgemaakt als een matrix JSON van tekenreeksen in de tekenreeksmatrix van een die kan worden gebruikt om te vullen transformaties een `Collection(Edm.String)` veld in de index. 

Als de ingevoerde tekenreeks wordt bijvoorbeeld `["red", "white", "blue"]`, klikt u vervolgens het veld als doel van het type `Collection(Edm.String)` wordt gevuld met de drie waarden `red`, `white` en `blue`. Voor de invoerwaarden die niet kunnen worden geparseerd als JSON tekenreeksmatrices, een fout geretourneerd. 

#### <a name="sample-use-case"></a>Voorbeeld use-case

Azure SQL-database geen ingebouwde gegevenstype natuurlijk wordt toegewezen aan `Collection(Edm.String)` velden in Azure zoeken. Maak uw brongegevens op als een matrix van de tekenreeks JSON in te vullen tekenreeks siteverzameling velden, en gebruik deze functie. 

#### <a name="example"></a>Voorbeeld 

```JSON

"fieldMappings" : [ 
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
] 
```

## <a name="help-us-make-azure-search-better"></a>Help ons te verbeteren Azure zoeken

Als u functieverzoeken verzenden of ideeën voor de verbeteringen hebt, neem contact maken met ons op onze [UserVoice-site](https://feedback.azure.com/forums/263029-azure-search/).