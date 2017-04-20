<properties
    pageTitle="De Microsoft Translator in logica apps toevoegen | Microsoft Azure"
    description="Overzicht van de Microsoft Translator verbindingslijn met REST API parameters"
    services=""
    suite=""
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

# <a name="get-started-with-the-microsoft-translator-connector"></a>Aan de slag met de verbindingslijn Microsoft Translator
Verbinding maken met Microsoft Translator tekst vertalen, een taal en meer detecteren. Met Microsoft Translator, kunt u het volgende doen: 

- De bedrijfswerkstroom van uw op basis van de gegevens die u ophalen uit Microsoft Translator maken. 
- Acties gebruiken voor het tekst vertalen, een taal en meer detecteren. Deze acties bent u een reactie en klikt u vervolgens de uitvoer beschikbaar maken voor andere acties. Wanneer een nieuw bestand is gemaakt in Dropbox, kunt u bijvoorbeeld de tekst in het bestand naar een andere taal met Microsoft Translator vertalen.

Als u wilt een bewerking in logica apps toevoegen, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties
Microsoft Translator bevat de volgende bewerkingen uit. Er zijn geen triggers.

Triggers | Acties
--- | ---
Geen | <ul><li>Taal bepalen</li><li>Tekst naar spraak</li><li>Tekst vertalen</li><li>Talen downloaden</li><li>Spraak talen downloaden</li></ul>

Alle verbindingslijnen ondersteuning voor gegevens in JSON en XML-indelingen.


## <a name="create-a-connection-to-microsoft-translator"></a>Een verbinding maken met Microsoft Translator

>[AZURE.INCLUDE [Steps to create a connection to Microsoft Translator](../../includes/connectors-create-api-microsofttranslator.md)]


## <a name="swagger-rest-api-reference"></a>Swagger REST API verwijzing
Dit geldt voor versie: 1.0.

### <a name="detect-language"></a>Taal bepalen    
Vastgesteld bron taal van tekst gegeven.  
```GET: /Detect```

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|query|tekenreeks|Ja|query|geen |Tekst waarvan de taal worden geïdentificeerd|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


### <a name="text-to-speech"></a>Tekst naar spraak    
Converteert een bepaalde tekst naar spraak als een audiostroom in Golf-indeling.  
```GET: /Speak```

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|query|tekenreeks|Ja|query|geen |Tekst converteren|
|taal|tekenreeks|Ja|query|geen |Taalcode om spraak te genereren (voorbeeld: ' en-us')|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


### <a name="translate-text"></a>Tekst vertalen    
Converteert tekst naar een bepaalde taal met Microsoft Translator.  
```GET: /Translate```

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|query|tekenreeks|Ja|query|geen |Tekst vertalen|
|languageTo|tekenreeks|Ja|query| geen|TARGET taal (voorbeeld: "Frans")|
|languageFrom|tekenreeks|geen|query|geen |Bron taal worden weergegeven. Als u niet wordt opgegeven, probeert Microsoft Translator te automatisch detecteren. (voorbeeld: nl)|
|categorie|tekenreeks|geen|query|algemene |Vertaling categorie (standaard: 'algemene')|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


### <a name="get-languages"></a>Talen downloaden    
Hiermee haalt u alle talen die ondersteuning biedt voor Microsoft Translator.  
```GET: /TranslatableLanguages```

Er zijn geen parameters voor dit gesprek. 

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


### <a name="get-speech-languages"></a>Spraak talen downloaden    
Hiermee haalt u de talen die beschikbaar zijn voor spraak synthese.  
```GET: /SpeakLanguages``` 

Er zijn geen parameters voor dit gesprek.

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|

## <a name="object-definitions"></a>Objectdefinities

#### <a name="language-language-model-for-microsoft-translator-translatable-languages"></a>Taal: taalmodel voor Microsoft Translator omgezet talen

|Naam van eigenschap | Gegevenstype | Vereist|
|---|---|---|
|Code|tekenreeks|geen|
|Naam|tekenreeks|geen|


## <a name="next-steps"></a>Volgende stappen

[Een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

Ga terug naar de [lijst API's](apis-list.md).


<!--References-->
[5]: https://datamarket.azure.com/developer/applications/
[6]: ./media/connectors-create-api-microsofttranslator/register-your-application.png
