<properties
    pageTitle="Aan de slag met: Machine Learning tekst Analytics-API's | Microsoft Azure"
    description="Azure Machine Learning-tekst Analytics - aan de slag met"
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

# <a name="getting-started-with-the-text-analytics-apis-to-detect-sentiment-key-phrases-topics-and-language"></a>Aan de slag met de tekst Analytics-API's om te bepalen sentiment, key woordgroepen, onderwerpen en taal

<a name="HOLTop"></a>

In dit document wordt beschreven hoe naar ingebouwde uw service of de toepassing voor het gebruik van de [Tekst Analytics-API's](//go.microsoft.com/fwlink/?LinkID=759711).
U kunt deze API's gebruiken om te bepalen sentiment, key woordgroepen, onderwerpen en taal op uw tekst. [Klik hier voor een interactieve demo van de ervaring.](//go.microsoft.com/fwlink/?LinkID=759712)

Raadpleeg de [API definities](//go.microsoft.com/fwlink/?LinkID=759346) voor technische documentatie voor de API's.

Deze handleiding is bedoeld voor versie 2 van de API's. Voor informatie over versie 1 van de API's, [raadpleegt u dit document](../machine-learning/machine-learning-apps-text-analytics.md).

Aan het einde van deze zelfstudie, is mogelijk via programmacode vinden:

- **Sentiment** - tekst Is positief of negatief?

- **Toets woordgroepen** - wat zijn de personen in een enkel artikel bespreekt?

- **Onderwerpen** - wat personen zijn bespreekt over veel artikelen?

- **Talen** - tekst voor welke taal is geschreven in?

Houd er rekening mee dat deze API 1 transactie per document na lezing eventueel kosten. Als u bijvoorbeeld als u ook zelf sentiment voor documenten van 1000 in één aanroep aanvragen, wordt 1000 transacties afgetrokken.



<a name="Overview"></a>
## <a name="general-overview"></a>Algemeen overzicht ##

Dit document is een stapsgewijze handleiding. Ons doel is Doorloop de stappen die nodig zijn voor een model trainen en wijst u resources waarmee u plaatsen in productie. Deze oefening duurt ongeveer 30 minuten.

Voor deze taken, u moet een editor en de RESTful eindpunten bellen in de taal van keuze.

Laten we aan de slag!

## <a name="task-1---signing-up-for-the-text-analytics-apis"></a>Taak 1 - ondertekend voor de tekst Analytics-API's ####

In deze taak wordt u zich aanmeldt voor de tekst analytics-service.

1. Ga naar **Cognitieve Services** in de [Portal van Azure](//go.microsoft.com/fwlink/?LinkId=761108) en zorg ervoor **Dat Analytics tekst** is geselecteerd als het type van het API.

1. Selecteer een abonnement. U kunt de **gratis laag voor 5000 transacties/maand**kunt selecteren. Als er is een gratis abonnement, moet u niet betalen voor het gebruik van de service. U moet aanmelden bij uw Azure-abonnement. 

1. Vul de overige velden en maak uw account.

1. Nadat u zich aanmeldt voor tekst analyses, uw **API-sleutel**niet vinden. Kopieer de primaire sleutel, zoals u deze moet bij gebruik van de API-services.


## <a name="task-2---detect-sentiment-key-phrases-and-languages"></a>Taak 2: sentiment, key woordgroepen en talen detecteren ####

Het is eenvoudig sentiment, key woordgroepen en talen detecteren in uw tekst. Via programmacode krijgt u hetzelfde resultaat als de [demo ervaring](//go.microsoft.com/fwlink/?LinkID=759712) geeft als resultaat.

>[AZURE.TIP] Voor sentiment analyse, is het raadzaam dat u tekst in zinnen splitsen. Dit is gewoonlijk het leidt tot een hogere precisie in sentiment voorspellingen.

Houd er rekening mee dat de talen worden ondersteund als volgt:

| Functie | Ondersteunde talen |
|:-----|:----|
| Sentiment | `en`(Engelstalig), `es` (Spaans), `fr` (Frans), `pt` (Portugees) |
| Belangrijke woordgroepen | `en`(Engelstalig), `es` (Spaans), `de` (Duits), `ja` (Japans) |


1. U moet de koppen instellen als volgt uit. Houd er rekening mee dat JSON momenteel de enige geaccepteerde invoer indeling voor de API's is. XML wordt niet ondersteund.

        Ocp-Apim-Subscription-Key: <your API key>
        Content-Type: application/json
        Accept: application/json

1. Maak vervolgens uw invoer rijen in JSON. Voor sentiment, key woordgroepen en taal is de notatie hetzelfde. Houd er rekening mee dat elke-ID uniek zijn moet en worden de ID geretourneerd door het systeem. De maximale grootte van een document dat kan worden ingediend is 10KB en de totale maximale grootte van ingediende invoer is 1MB. Mogelijk worden niet meer dan 1000 documenten in een oproep ingediend. Tarief beperken bestaat met een snelheid van 100 oproepen per minuut - daarom aangeraden dat u grote hoeveelheden documenten in één aanroep verzenden. Taal is een optionele parameter die moet worden opgegeven als het analyseren van niet-Engelstalige tekst. Een voorbeeld van de invoer is hieronder wordt weergegeven, waarbij de optionele parameter `language` voor sentiment extractie van woordgroep analyse of sleutel is opgenomen:

        {
            "documents": [
                {
                    "language": "en",
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "language": "en",
                    "id": "100",
                    "text": "Final document"
                }
            ]
        }

1. Bellen **bericht** voor de invoer voor sentiment, key woordgroepen en taal. De URL's er als volgt uit:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages

1. In dit gesprek retourneert een JSON opgemaakt als reactie met de id's en eigenschappen gedetecteerd. Een voorbeeld van de uitvoer voor sentiment hieronder (met details van fouten die zijn uitgesloten). In het geval van sentiment, wordt voor elk document een score tussen 0 en 1 geretourneerd:

        // Sentiment response
        {
            "documents": [
                {
                    "id": "1",
                    "score": "0.934"
                },
                ...
                {
                    "id": "100",
                    "score": "0.002"
                },
            ]
        }

        // Key phrases response
        {
            "documents": [
                {
                    "id": "1",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
                ...
                {
                    "id": "100",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
            ]
        }

        // Languages response
        {
            "documents": [
                {
                    "id": "1",
                    "detectedLanguages": [
                        {
                            "name": "English",
                            "iso6391Name": "en",
                            "score": "1"
                        }
                    ]
                },
                ...
                {
                    "id": "100",
                    "detectedLanguages": [
                        {
                            "name": "French",
                            "iso6391Name": "fr",
                            "score": "0.985"
                        }
                    ]
                }
            ]
        }


## <a name="task-3---detect-topics-in-a-corpus-of-text"></a>Taak 3: detecteren onderwerpen in een corpus van tekst ####

Dit is een nieuw uitgebrachte API welke geeft als resultaat de bovenkant gedetecteerd onderwerpen voor een lijst met tekstrecords verzonden. Een onderwerp wordt aangeduid met een belangrijke tekst, die een of meer gerelateerde woorden. De API is ontworpen voor gebruik voor korte, menselijke geschreven tekst zoals beoordelingen en feedback van gebruikers.

Deze API moet **ten minste 100 tekstrecords** in te dienen, maar is ontworpen voor het detecteren van onderwerpen over honderden tot duizenden records. Een niet-Engelstalige of records met minder dan 3 woorden worden genegeerd en kunnen daarom niet worden toegewezen aan onderwerpen. Voor de detectie, onderwerp, de maximale grootte van een document dat kan worden ingediend is 30KB en de totale maximale grootte van ingediende invoer is 30MB. Onderwerp detectie is snelheid beperkt tot 5 ingediende items elke 5 minuten.

Er zijn twee extra **optioneel** invoerparameters die helpen kunnen om de kwaliteit van de resultaten te verbeteren:

- **Woorden stoppen.**  Deze woorden en hun sluiten formulieren (bijvoorbeeld meervouden) worden uitgesloten uit de hele onderwerp detectie pijplijn. Gebruik deze opdracht voor veelgebruikte woorden (voor bijvoorbeeld "probleem", "fout" en "gebruiker" mogelijk keuzen die klachten over software). Elke tekenreeks moet één woord.
- **Stoppen met woordgroepen** - deze woordgroepen wordt worden uitgesloten uit de lijst met geretourneerde onderwerpen. Gebruik deze algemene onderwerpen die u niet wilt zien in de resultaten uitsluiten. Bijvoorbeeld: 'Microsoft' en 'Azure' zou keuzen die onderwerpen uitsluiten. Tekenreeksen kunnen meerdere woorden bevatten.

Volg deze stappen om op te sporen onderwerpen in uw tekst.

1. De invoer in JSON opmaken. Schakel ditmaal die u kunt definiëren stoppen woorden en zinnen te stoppen.

        {
            "documents": [
                {
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "id": "100",
                    "text": "Final document"
                }
            ],
            "stopWords": [
                "issue", "error", "user"
            ],
            "stopPhrases": [
                "Microsoft", "Azure"
            ]
        }

1. Met dezelfde kopteksten zoals gedefinieerd op taak 2 maken, een **bericht** bellen naar het eindpunt onderwerpen:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/topics

1. Hiermee herstelt u een `operation-location` als de kop van het antwoord, waar de waarde de URL voor de query voor de resulterende onderwerpen is:

        'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

1. De resulterende query `operation-location` regelmatig met een **GET** -aanvraag. Eenmaal per minuut wordt aanbevolen.

        GET https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>

1. Het eindpunt zullen retourneren een antwoord inclusief `{"status": "notstarted"}` vóór de verwerking `{"status": "running"}` tijdens het verwerken en `{"status": "succeeded"}` met de uitvoer na voltooiing. U kunt vervolgens de uitvoer die weergegeven in de volgende indeling (notitie details worden zoals fout indeling en datums niet in dit voorbeeld opgenomen zijn) gebruiken:

        {
            "status": "succeeded",
            "operationProcessingResult": {
                "topics": [
                    {
                        "id": "8b89dd7e-de2b-4a48-94c0-8e7844265196"
                        "score": "5"
                        "keyPhrase": "first topic name"
                    },
                    ...
                    {
                        "id": "359ed9cb-f793-4168-9cde-cd63d24e0d6d"
                        "score": "3"
                        "keyPhrase": "final topic name"
                    }
                ],
                "topicAssignments": [
                    {
                        "topicId": "8b89dd7e-de2b-4a48-94c0-8e7844265196",
                        "documentId": "1",
                        "distance": "0.354"
                    },
                    ...
                    {
                        "topicId": "359ed9cb-f793-4168-9cde-cd63d24e0d6d",
                        "documentId": "55",
                        "distance": "0.758"
                    },            
                ]
            }
        }

Houd er rekening mee dat het succesvolle antwoord voor onderwerpen uit de `operations` eindpunt heeft de volgende schema:

    {
            "topics" : [{
                "id" : "string",
                "score" : "number",
                "keyPhrase" : "string"
            }],
            "topicAssignments" : [{
                "documentId" : "string",
                "topicId" : "string",
                "distance" : "number"
            }],
            "errors" : [{
                "id" : "string",
                "message" : "string"
            }]
        }

Toelichtingen voor elk onderdeel van dit antwoord zijn als volgt:

**onderwerpen**

| Toets | Beschrijving |
|:-----|:----|
| ID | Een unieke id voor elk onderwerp. |
| score | Het aantal documenten die zijn toegewezen aan een onderwerp. |
| keyPhrase | Een samengevat woord of woordgroep voor het onderwerp. |

**topicAssignments**

| Toets | Beschrijving |
|:-----|:----|
| documentId | ID voor het document. Is gelijk aan de ID die is opgenomen in de invoer. |
| onderwerpklasse | De onderwerp-ID die het document is toegewezen aan. |
| afstand | Document-naar-onderwerp lidmaatschap score tussen 0 en 1. De laagste een afstand score hoe sterker het onderwerp lidmaatschap is. |

**fouten**

| Toets | Beschrijving |
|:-----|:----|
| ID | Unieke document-id die de fout naar verwijst worden ingevoerd. |
| Bericht | Foutbericht wordt weergegeven. |

## <a name="next-steps"></a>Volgende stappen ##

Gefeliciteerd! U hebt nu voltooid door middel van tekst-analyses op uw gegevens. U kunt nu om te zoeken in een hulpmiddel zoals [Power BI](//powerbi.microsoft.com) gebruiken uw gegevens kunt visualiseren, evenals uw inzichten zodat u een realtime weergave van de tekstgegevens van uw te automatiseren.

Als u wilt zien hoe tekst Analytics-functies, zoals sentiment, kunnen worden gebruikt als onderdeel van een bots, raadpleegt u het voorbeeld [Emotionele bots](http://docs.botframework.com/en-us/bot-intelligence/language/#example-emotional-bot) op de site bots Framework.
