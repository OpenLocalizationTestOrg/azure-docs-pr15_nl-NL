<properties
    pageTitle="Een upgrade naar versie 2 van de tekst Analytics API | Microsoft Azure"
    description="Azure Machine Learning-tekst Analytics - Upgrade naar versie 2"
    services="cognitive-services"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>

# <a name="upgrading-to-version-2-of-the-text-analytics-api"></a>Een upgrade naar versie 2 van de tekst Analytics-API #

Deze handleiding gaat u door het proces van het bijwerken van uw code uit de [eerste versie van de API](../machine-learning/machine-learning-apps-text-analytics.md) voor het gebruik van de tweede versie. 

Als u de API niet hebt gebruikt en voor meer informatie wilt, kunt u **[meer informatie over de API hier](//go.microsoft.com/fwlink/?LinkID=759711)** of **[volgen van de handleiding aan de slag](//go.microsoft.com/fwlink/?LinkID=760860)**. Raadpleeg de **[Definitie van de API](//go.microsoft.com/fwlink/?LinkID=759346)**voor technische verwijzing.

### <a name="part-1-get-a-new-key"></a>Deel 1. Een nieuwe sleutel ophalen ###

U moet eerst een nieuwe API-sleutel krijgen van de **Azure-Portal**:

1. Navigeer naar de tekst Analytics-service via de [Galerie met Cortana Intelligence](//gallery.cortanaintelligence.com/MachineLearningAPI/Text-Analytics-2). Hier vindt u koppelingen naar de documentatie en code voorbeelden ook.

1. Klik op **aanmelden**. Deze koppeling gaat u naar de beheerportal Azure, waar u zich kunt aanmelden voor de service.

1. Selecteer een abonnement. U kunt de **gratis laag voor 5000 transacties/maand**kunt selecteren. Als er is een gratis abonnement, moet u niet betalen voor het gebruik van de service. U moet aanmelden bij uw Azure-abonnement. 

1. Nadat u zich aanmeldt voor tekst analyses, krijgt u een **API-sleutel**. Kopieer deze toets, zoals u deze moet bij gebruik van de API-services.

### <a name="part-2-update-the-headers"></a>Deel 2. Bijwerken van de koppen ###

Werk de ingediende koptekst-waarden zoals hieronder wordt weergegeven. Houd er rekening mee dat de accountsleutel niet langer is gecodeerd.

**Versie 1**

    Authorization: Basic base64encode(<your Data Market account key>)
    Accept: application/json

**Versie 2**

    Content-Type: application/json
    Accept: application/json
    Ocp-Apim-Subscription-Key: <your Azure Portal account key>


### <a name="part-3-update-the-base-url"></a>Deel 3. Bijwerken van de basis-URL ###

**Versie 1**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/

**Versie 2**

    https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/

### <a name="part-4a-update-the-formats-for-sentiment-key-phrases-and-languages"></a>Deel 4a. Bijwerken van de indelingen voor sentiment, key woordgroepen en talen ###

#### <a name="endpoints"></a>Eindpunten ####

KRIJG eindpunten zijn nu afgekeurd, zodat alle invoer als een POST-aanvraag moet worden ingediend. Werk de eindpunten bij de kleuren die hieronder wordt weergegeven.

| |Één versie 1-eindpunt|Versie 1 batch eindpunt|Versie 2 eindpunt|
|---|---|---|---|
|Type bellen|Toevoegen|Verzenden|Verzenden|
|Sentiment|```GetSentiment```|```GetSentimentBatch```|```sentiment```|
|Belangrijke woordgroepen|```GetKeyPhrases```|```GetKeyPhrasesBatch```|```keyPhrases```|
|Talen|```GetLanguage```|```GetLanguageBatch```|```languages```|

#### <a name="input-formats"></a>Invoer-indelingen ####

Houd er rekening mee dat alleen bericht opmaken nu wordt aangenomen, zodat u moet opnieuw indelen elke invoer waar vroeger de eindpunten één document dienovereenkomstig gewijzigd. Ingangen zijn niet hoofdlettergevoelig.

**Versie 1 (batch)**

    {
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Versie 2**

    {
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="output-from-sentiment"></a>Uitvoer van sentiment ####

**Versie 1**

    {
      "SentimentBatch":[{
        "Id":"string",
        "Score":"double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Versie 2**

    {
      "documents":[{
        "id":"string",
        "score":"double"
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-key-phrases"></a>Uitvoer van belangrijke woordgroepen ####

**Versie 1**

    {
      "KeyPhrasesBatch":[{
        "Id":"string",
        "KeyPhrases":["string"]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Versie 2**

    {
      "documents":[{
        "id":"string",
        "keyPhrases":["string"]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-languages"></a>Uitvoer van talen ####


**Versie 1**

    {
      "LanguageBatch":[{
        "id":"string",
        "detectedLanguages": [{
          "Score":"double"
          "Name":"string",
          "Iso6391Name":"string"
        }]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Versie 2**

    {
      "documents":[{
        "id":"string",
        "detectedLanguages": [{
          "score":"double"
          "name":"string",
          "iso6391Name":"string"
        }]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }


### <a name="part-4b-update-the-formats-for-topics"></a>Deel 4b. De notaties voor de onderwerpen bijwerken ###

#### <a name="endpoints"></a>Eindpunten ####

| |Versie 1-eindpunt | Versie 2-eindpunt|
|---|---|---|
|Indienen voor onderwerp detectie (POST)|```StartTopicDetection```|```topics```|
|Onderwerp resultaten (GET) ophalen|```GetTopicDetectionResult?JobId=<jobId>```|```operations/<operationId>```|

#### <a name="input-formats"></a>Invoer-indelingen ####

**Versie 1**

    {
      "StopWords": [
        "string"
      ],
      "StopPhrases": [
        "string"
      ], 
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Versie 2**

    {
      "stopWords": [
        "string"
      ],
      "stopPhrases": [
        "string"
      ],
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="submission-results"></a>Resultaten van indienen ####

**Versie 1 (POST)**

Wanneer de taak is voltooid, ontvangt u eerder, de volgende JSON-uitvoer, waar de taak-id zou worden toegevoegd aan een URL voor het ophalen van de uitvoer.

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

**Versie 2 (POST)**

Het antwoord bevat een waarde koptekst nu als volgt bepaald, waarbij `operation-location` wordt gebruikt als het eindpunt worden gecontroleerd op de resultaten:

    'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

#### <a name="operation-results"></a>Resultaat van de bewerking ####

**Versie 1 (GET)**

    {
      "TopicInfo" : [{
        "TopicId" : "string"
        "Score" : "double"
        "KeyPhrase" : "string"
      }],
      "TopicAssignment" : [{
        "Id" : "string",
        "TopicId" : "string",
        "Distance" : "double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Versie 2 (GET)**

Als voorheen **regelmatig poll uitvoeren onder de uitvoer** (de voorgestelde periode is elke minuut) tot de uitvoer wordt geretourneerd. 

Wanneer de onderwerpen-API is voltooid, de status Lees `succeeded` wordt geretourneerd. Dit wordt het resultaat vervolgens opnemen in de notatie die hieronder wordt weergegeven:

    {
        "status": "succeeded",
        "createdDateTime": "string",
        "operationType": "topics",
        "processingResult": {
            "topics" : [{
            "id" : "string"
            "score" : "double"
            "keyPhrase" : "string"
          }],
          "topicAssignments" : [{
            "topicId" : "string",
            "documentId" : "string",
            "distance" : "double"
          }],
          "errors" : [{
              "id":"string",
              "message":"string"
          }]
        }
    }

### <a name="part-5-test-it"></a>Deel 5. Testen! ###

Nu moet u aan de slag! Test uw code met een kleine steekproef om ervoor te zorgen dat u uw gegevens kunt verwerken.
