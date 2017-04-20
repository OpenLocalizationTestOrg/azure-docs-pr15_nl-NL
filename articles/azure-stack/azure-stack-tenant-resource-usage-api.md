<properties
    pageTitle="Resourcegebruik API tenant | Microsoft Azure"
    description="Overzicht van Resourcegebruik API, waarin informatie over het gebruik van de Azure stapel ophalen."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="tenant-resource-usage-api"></a>Resourcegebruik API tenant

Een tenant kunt de Tenant-API gebruiken om het hulpprogramma voor het gebruik van de tenant eigen-resourcegegevens weergeven. Deze API komt overeen met de API voor het gebruik van Azure (die momenteel in privé preview).

U kunt de Windows PowerShell-cmdlet **Get-UsageAggregates** gebruiken om gegevens over zoekgebruik zoals in Azure wordt aangegeven.

## <a name="api-call"></a>API-oproep

### <a name="request"></a>Aanvragen

Het verzoek krijgt verbruik details voor de gevraagde abonnementen en voor de gevraagde tijdsbestek. Er is geen hoofdgedeelte van de aanvraag.

| **Methode**  | **URI aanvragen** |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Toevoegen         | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumenten

| **De argumenten**             | **Beschrijving** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Armendpoint*             | Azure resourcemanager eindpunt van uw omgeving Azure stapel. |
| *subId*                   | Abonnements-ID van de gebruiker van wie u de oproep is plaatst. U kunt deze API alleen naar query gebruiken voor het gebruik van een één abonnement. Providers kunnen u de Provider Resource gebruik API aan het gebruik van de query gebruiken voor alle tenants. |
| *reportedStartTime*       | Begintijd van de query. De waarde voor *DateTime* moet in UTC en aan het begin van het uur, bijvoorbeeld 13:00. Stel deze waarde in UTC middernacht voor dagelijkse aggregatie. De indeling is *voorafgegaan* ISO-8601, bijvoorbeeld 2015-06-16T18% 3a53% 3a11% 2b00% 3a00Z, waar dubbele punt is voorafgegaan naar % 3a en plus is voorafgegaan aan % 2b zodat deze URI beschrijvende is. |
| *reportedEndTime*         | Eindtijd van de query. De beperkingen voor *reportedStartTime* gelden ook voor dit argument. De waarde voor *reportedEndTime* kan niet in de toekomst zijn. |
| *aggregationGranularity*  | Optionele parameter met twee aparte mogelijke waarden: dagelijks en per uur. Als de waarden voorstellen, een geeft als resultaat de gegevens in de dagelijkse granulatie en de andere is een per uur op te lossen. De optie dagelijks is de standaardwaarde. |
| *subscriberId*            | Abonnement-ID. Als u gefilterde gegevens, is de abonnements-ID van een directe tenant van de provider vereist. Als er geen abonnement-ID-parameter is opgegeven, geeft de oproep gebruiksgegevens voor directe tenants van de provider. |
| *API-versie*             | Versie van het protocol dat is gebruikt om deze aanvraag. U moet 2015-06-01-preview gebruiken. |
| *continuationToken*       | Token opgehaald uit de laatste oproep door naar de gebruik API-provider. Dit is nodig wanneer een antwoord groter dan 1000 lijnen is. Dit is de bladwijzer voor taakvoortgang. Als niet aanwezig zijn, de gegevens zijn opgehaald vanaf het begin van de dag of uur, op basis van de granulatie doorgegeven. |

### <a name="response"></a>Antwoord

/Subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 & reportedEndTime ophalen = 2015-06-01T00% 3a00% 3a00% 2b00% 3a00 & aggregationGranularity = dagelijks & api-versie 1.0 =

{

'waarde':\[

{

"-id": "/ subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",

"naam": "Abb1-meterID1"

"type": "Microsoft.Commerce/UsageAggregate",

"eigenschappen": {

"subscriptionId": "Abb1",

"usageStartTime": "2015-03-03T00:00:00 + 00:00",

"usageEndTime": "2015-03-04T00:00:00 + 00:00",

"instanceData": "{\\" Microsoft.Resources\\": {\\" resourceUri\\":\\" resourceUri1\\',\\"locatie\\":\\"Alaska\\',\\" tags\\": null,\\" additionalInfo\\": null}}",

"hoeveelheid":2.4000000000,

"meterId": "meterID1"

}

},

…

### <a name="response-details"></a>Antwoord details

| **De argumenten**      | **Beschrijving** |
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *ID*              | Unieke ID van de aggregatie gebruik |
| *naam*            | Naam van de aggregatie gebruik |
| *type*            | De definitie van de resource |
| *subscriptionId*  | Abonnement-id van de Azure gebruiker |
| *usageStartTime*  | UTC begintijd van het gebruik Emmertje die deze samenvoeging gebruik hoort |
| *usageEndTime*    | UTC eindtijd van de gebruik Emmertje die deze samenvoeging gebruik hoort |
| *instanceData*    | Sleutel-waardeparen van gegevens over exemplaar (in een nieuwe indeling):<br>  *resourceUri*: volledig gekwalificeerd resource-ID, inclusief resourcegroepen en de exemplaarnaam <br>  *locatie*: regio waarin deze service is uitgevoerd <br>  *tags*: Resource tags die de gebruiker aangeeft <br>  *additionalInfo*: meer details over de resource wordt gebruikt, bijvoorbeeld OS versie of de afbeelding van type |
| *aantal*        | Hoeveelheid verbruik van resources die zijn aangebracht in deze periode |
| *meterId*         | Unieke ID voor de resource die is verbruikt (ook wel *ResourceID*) |

## <a name="next-steps"></a>Volgende stappen

[Veelgestelde vragen over het gebruik betrekking](azure-stack-usage-related-faq.md)
