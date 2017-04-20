<properties
    pageTitle="Machine Learning-API's: Tekst Analytics | Microsoft Azure"
    description="Microsoft Machine Learning tekst Analytics API's kan worden gebruikt om te analyseren ongestructureerde tekst voor sentiment analyse, de extractie van de belangrijkste woordgroep, taaldetectie en onderwerp detectie."
    services="machine-learning"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/> 

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>


# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>Machine Learning-API's: Tekst Analytics voor Sentiment, belangrijke woordgroep extractie, taaldetectie en onderwerp detectie

>[AZURE.NOTE] Deze handleiding is bedoeld voor versie 1 van de API. Versie 2, [**raadpleegt u dit document**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). Versie 2 is nu de voorkeur versie van deze API.

## <a name="overview"></a>Overzicht

De tekst Analytics-API is een reeks tekst analytics [-webservices](https://datamarket.azure.com/dataset/amla/text-analytics) ingebouwd met Azure Machine Learning. De API kan worden gebruikt om te analyseren ongestructureerde tekst voor taken zoals sentiment analyse, de extractie van de belangrijkste woordgroep, taaldetectie en onderwerp detectie. U hebt geen trainingsgegevens nodig om deze API: alleen brengen uw tekstgegevens. Deze API gebruikt geavanceerde natuurlijke taal processing technieken voor het leveren van aanbevolen in class voorspellingen.

U kunt tekst analytics in actie zien op onze [demo site](https://text-analytics-demo.azurewebsites.net/), waar u ook vindt [voorbeelden](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) over het implementeren van tekst analytics in C# en Python.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 

---

## <a name="sentiment-analysis"></a>Sentiment analyse

De API retourneert numerieke scores tussen 0 en 1. Scores dicht bij 1 aangeven positieve sentiment, terwijl scores dicht bij 0 negatieve sentiment geven. Sentiment score wordt gegenereerd op basis van de classificatie-technieken. De invoer functies aan de classificatie opnemen n-g, functies die zijn gegenereerd op basis van het onderdeel van spraak labels en word insluitingen. Engels is momenteel de enige ondersteunde taal.
 
## <a name="key-phrase-extraction"></a>De extractie van de belangrijkste woordgroep

De API geeft als resultaat een lijst met tekenreeksen ter aanduiding van de hoofdpunten bespreken in de ingevoerde tekst. Gebruiken we technieken van Microsoft Office van geavanceerde natuurlijke taal Processing toolkit. Engels is momenteel de enige ondersteunde taal.

## <a name="language-detection"></a>Taaldetectie

De API geeft als resultaat de gevonden taal en numerieke scores tussen 0 en 1. Scores dicht bij 1 geven aan dat de opgegeven taal waar is u kunt 100%. Een totaal van 120 talen worden ondersteund.

## <a name="topic-detection"></a>Onderwerp detectie

Dit is een nieuw uitgebrachte API welke geeft als resultaat de bovenkant gedetecteerd onderwerpen voor een lijst met tekstrecords verzonden. Een onderwerp wordt aangeduid met een belangrijke tekst, die een of meer gerelateerde woorden. Deze API moet ten minste 100 tekstrecords moeten worden verzonden, maar is ontworpen voor het detecteren van onderwerpen over honderden tot duizenden records. Houd er rekening mee dat deze API 1 transactie per tekst record wordt toegezonden kosten. De API is ontworpen voor gebruik voor korte, menselijke geschreven tekst zoals beoordelingen en feedback van gebruikers.

---

## <a name="api-definition"></a>API definitie

### <a name="headers"></a>Kopteksten

Zorgen ervoor dat u de juiste koppen in uw aanvraag, ziet er als volgt:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

U kunt uw accountsleutel vinden van uw account in de [Azure Datamarket](https://datamarket.azure.com/account/keys). Houd er rekening mee dat de JSON momenteel alleen voor invoer- en uitvoerbereik indelingen wordt geaccepteerd. XML wordt niet ondersteund.

---

## <a name="single-response-apis"></a>Eén antwoord API 's

### <a name="getsentiment"></a>GetSentiment

**URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Voorbeeld van de aanvraag**

Klik tijdens het gesprek onder aanvraagt we sentiment analyse voor de plaats van de tekst 'Hallo wereld':

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

Hiermee herstelt u een antwoord als volgt:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

---

### <a name="getkeyphrases"></a>GetKeyPhrases

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**Voorbeeld van de aanvraag**

In de onderstaande oproep aanvraagt we dat de belangrijkste zinnen zijn gevonden in de tekst 'Dit is een geweldig hotel blijft staan in, met decor unieke en beschrijvende personeel':

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

Hiermee herstelt u een antwoord als volgt:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }
 
---

### <a name="getlanguage"></a>GetLanguage

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

**Voorbeeld van de aanvraag**

Klik tijdens het gesprek GET onderstaande aanvraagt we voor de sentiment voor de belangrijkste zinnen in de tekst *Hallo allemaal*

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

Hiermee herstelt u een antwoord als volgt:

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

**Optionele parameters**

`NumberOfLanguagesToDetect`is een optionele parameter. De standaardinstelling is 1.

---

## <a name="batch-apis"></a>Batch API 's

De tekst Analytics-service kunt u doen sentiment en sleutel-zin extracties in de batchmodus. Houd er rekening mee dat elk van de records van een score telt als één transactie voorzien. Als u bijvoorbeeld als u ook zelf sentiment voor 1000 records in één aanroep aanvragen, wordt 1000 transacties afgetrokken.

Houd er rekening mee dat de id's in het systeem ingevoerd de id's die het resultaat van het systeem zijn. De webservice wordt niet gecontroleerd dat deze id uniek zijn. Dit is de verantwoordelijkheid van de beller om te controleren of uniekheid. 


### <a name="getsentimentbatch"></a>GetSentimentBatch

**URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Voorbeeld van de aanvraag**

Klik tijdens het gesprek bericht onder aanvraagt we voor de patronen van de zinnen "Hallo wereld", 'Hallo Foo wereld' en 'Hallo Mijn wereld' in de hoofdtekst van het verzoek:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

Het hoofdgedeelte van de aanvraag:

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

In de onderstaande reactie krijgt u de lijst met scores dat is gekoppeld aan uw tekst id's:

    {
      "odata.metadata":"<url>", 
      "SentimentBatch":
      [
        {"Score":0.9549767,"Id":"1"},
        {"Score":0.7767222,"Id":"2"},
        {"Score":0.8988889,"Id":"3"}
      ],  
      "Errors":[]
    }


---

### <a name="getkeyphrasesbatch"></a>GetKeyPhrasesBatch

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**Voorbeeld van de aanvraag**

In dit voorbeeld aanvraagt we voor de lijst met patronen voor de belangrijkste zinnen in de volgende teksten: 

* "Dit is een geweldig hotel blijft staan in, met decor unieke en beschrijvende personeel"
* "Dit is een indrukwekkende opbouwen conferentie, met heel interessant spreekt"
* "Het verkeer is puberteit heeft weten, ik drie uur, gaat u naar de luchthaven besteed"

Wordt deze aangevraagd als een bericht oproep naar het eindpunt:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

Het hoofdgedeelte van de aanvraag:

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

In de onderstaande reactie krijgt u de lijst met belangrijke termen die is gekoppeld aan uw tekst id's:

    { "odata.metadata":"<url>",
        "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[]
    }

---

### GetLanguageBatch

In the POST call below, we are requesting language detection for two text inputs:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

Request body:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

This returns the following response, where English is detected in the first input and French in the second input:

    {
       "LanguageBatch": [{
         "Id": "1",
         "DetectedLanguages": [{
            "Name": "Engels",
            "Iso6391Name": "en",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       },
       {
         "Id": "2",
         "DetectedLanguages": [{
            "Name": "Frans",
            "Iso6391Name": "fr",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       }],
       "Errors": []
    }

---

## <a name="topic-detection-apis"></a>Onderwerp detectie-API 's

Dit is een nieuw uitgebrachte API welke geeft als resultaat de bovenkant gedetecteerd onderwerpen voor een lijst met tekstrecords verzonden. Een onderwerp wordt aangeduid met een belangrijke tekst, die een of meer gerelateerde woorden. Houd er rekening mee dat deze API 1 transactie per tekst record wordt toegezonden kosten.

Deze API moet ten minste 100 tekstrecords moeten worden verzonden, maar is ontworpen voor het detecteren van onderwerpen over honderden tot duizenden records.


### <a name="topics--submit-job"></a>Onderwerpen – verzenden taak

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Voorbeeld van de aanvraag**


In het bericht gesprek onderstaande vragen we onderwerpen voor een reeks 100 artikelen, waarbij de eerste en laatste invoer artikelen worden weergegeven en twee StopPhrases zijn opgenomen.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

Het hoofdgedeelte van de aanvraag:

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

In de onderstaande reactie krijgt u de taak-id voor de ingediende taak:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

Een lijst met één woord of meerdere word zinnen die niet als onderwerpen moeten worden geretourneerd. Kan worden gebruikt voor zeer algemene onderwerpen wegfilteren. Bijvoorbeeld in een gegevensset over hotel beoordelingen, "hotel" en "hostel" mogelijk stoppen logisch woordgroepen.  

### <a name="topics--poll-for-job-results"></a>Onderwerpen – peiling voor de resultaten van de taak

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Voorbeeld van de aanvraag**

Hiermee geeft u de taak-id als resultaat gegeven van de 'Verzenden taak' stap als u wilt ophalen van de resultaten. Het is raadzaam dat u dit eindpunt elke minuut tot Status belt = 'Voltooid' in de reactie. Het duurt ongeveer 10 minuten voor een taak voltooid of langer voor taken met duizenden records.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Tijdens het verwerken van, worden het antwoord als volgt:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
        "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


De API retourneert uitvoer in JSON-notatie in de volgende indeling:

    {
        "odata.metadata":"<url>",
        "Status":"Finished",
        "TopicInfo":[
        {
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Score":8.0,
            "KeyPhrase":"food"
        },
        ...
        {
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Score":6.0,
            "KeyPhrase":"decor"
            }
        ],
        "TopicAssignment":[
        {
            "Id":"1",
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Distance":0.7809
        },
        ...
        {
            "Id":"100",
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Distance":0.8034
        }
        ],
        "Errors":[]


De eigenschappen voor elk onderdeel van het antwoord zijn als volgt:

**TopicInfo eigenschappen**

| Toets | Beschrijving |
|:-----|:----|
| Onderwerpklasse | Een unieke id voor elk onderwerp. |
| Score | Het aantal records die zijn toegewezen aan een onderwerp. |
| KeyPhrase | Een samengevat woord of woordgroep voor het onderwerp. 1 of meerdere woorden zijn. |

**TopicAssignment eigenschappen**

| Toets | Beschrijving |
|:-----|:----|
| ID | ID voor de record. Is gelijk aan de ID die is opgenomen in de invoer. |
| Onderwerpklasse | De onderwerp-ID die de record is toegewezen aan. |
| Afstand | Betrouwbaarheid dat de record deel uitmaakt van het onderwerp. Afstand dichter naar nul wordt aangegeven hogere betrouwbaarheid. |
