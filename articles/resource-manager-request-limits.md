<properties
   pageTitle="Azure resourcemanager verzoeklimieten | Microsoft Azure"
   description="Beschrijving van het gebruik beperken met Azure resourcemanager aanvragen wanneer abonnementen hebt bereikt."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="tomfitz"/>

# <a name="throttling-resource-manager-requests"></a>Resourcemanager aanvragen beperken

Voor elk abonnement en tenant, resourcemanager limieten aanvragen voor 15.000 per uur lezen en schrijven aanvragen 1.200 per uur. Als uw toepassing of script deze limieten bereikt, moet u uw aanvragen beperken. In dit onderwerp ziet u hoe bepaalt u de resterende aanvragen die u hebt voordat de limiet bereikt, en hoe u moet reageren wanneer u de limiet hebt bereikt.

Wanneer u de limiet hebt bereikt, ontvangt u de HTTP-statuscode **429 te veel aanvragen**.

Het aantal aanvragen is beperkt tot uw abonnement of uw tenant. Als er meerdere, gelijktijdige toepassingen het aanvragen van uw abonnement, het aanvragen van deze toepassingen worden toegevoegd samen om te bepalen van het aantal resterende aanvragen.

Abonnement beperkt aanvragen zijn die de betrokken bij het doorgeven van uw abonnements-id, zoals het ophalen van de resource van in uw abonnement groepen. Tenant beperkt aanvragen Neem niet uw abonnements-id, zoals ongeldige Azure locaties ophalen.

## <a name="remaining-requests"></a>Overige opdrachten

U kunt het aantal resterende aanvragen vaststellen door het antwoord kopteksten onderzoeken. Elk verzoek om een bevat waarden voor het aantal resterende lezen en schrijven aanvragen. De volgende tabel beschrijft de reactie koppen die u voor deze waarden controleren kunt:

| Tekenarcering | Beschrijving |
| --------------- | ----------- |
| x-MS-ratelimit-Remaining-Subscription-reads | Abonnement beperkt leest resterende |
| x-MS-ratelimit-Remaining-Subscription-Writes | Abonnement beperkt schrijft resterende |
| x-MS-ratelimit-Remaining-tenant-reads | Tenant beperkt leest resterende |
| x-MS-ratelimit-Remaining-tenant-Writes | Tenant beperkt schrijft resterende |
| x-MS-ratelimit-Remaining-Subscription-resource-Requests | Abonnement beperkt resource type aanvragen resterende.<br /><br />Deze koptekst waarde wordt alleen geretourneerd als een service is de standaardbeperking overschreven. Resourcemanager wordt deze waarde in plaats van het abonnement lezen of schrijven. |
| x-MS-ratelimit-Remaining-Subscription-resource-ENTITIES-Read | Abonnement beperkt aanvragen voor siteverzameling van resource type resterende.<br /><br />Deze koptekst waarde wordt alleen geretourneerd als een service is de standaardbeperking overschreven. Deze waarde bevat het aantal resterende siteverzameling aanvragen (lijst resources). |
| x-MS-ratelimit-Remaining-tenant-resource-Requests | Tenant beperkt resource type aanvragen resterende.<br /><br />Deze kop is alleen toegevoegd voor aanvragen op het niveau van de tenant en alleen als een service is de standaardbeperking overschreven. Resourcemanager wordt deze waarde in plaats van de tenant lezen of schrijven. |
| x-MS-ratelimit-Remaining-tenant-resource-ENTITIES-Read | Tenant beperkt aanvragen voor siteverzameling van resource type resterende.<br /><br />Deze kop is alleen toegevoegd voor aanvragen op het niveau van de tenant en alleen als een service is de standaardbeperking overschreven. |

## <a name="retrieving-the-header-values"></a>De kop-waarden worden opgehaald

Deze koptekst-waarden in uw code of script ophalen verschilt niet ophalen van de waarde van een koptekst. 

Bijvoorbeeld in **C#**ophalen u de koptekst waarde uit een **HttpWebResponse** -object met de naam **antwoord** met de volgende code:

    response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)

In **PowerShell**, kunt u de kopwaarde ophalen uit een bewerking Roep-WebRequest.

    $r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
    $r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
    
Of, als wilt zien van de resterende aanvragen voor foutopsporing, kunt u bieden de **-fouten opsporen in** parameter op uw **PowerShell** -cmdlet.

    Get-AzureRmResourceGroup -Debug
    
Die resulteert veel informatie, waaronder de volgende antwoord-waarde:

    ...
    DEBUG: ============================ HTTP RESPONSE ============================

    Status Code:
    OK

    Headers:
    Pragma                        : no-cache
    x-ms-ratelimit-remaining-subscription-reads: 14999
    ...

In **Azure CLI**, kunt u de kopwaarde ophalen met behulp van de uitgebreidere optie.

    azure group list -vv --json

Welke geeft als resultaat een groot aantal informatie, inclusief het volgende object:

    ...
    silly: returnObject
    {
      "statusCode": 200,
      "header": {
        "cache-control": "no-cache",
        "pragma": "no-cache",
        "content-type": "application/json; charset=utf-8",
        "expires": "-1",
        "x-ms-ratelimit-remaining-subscription-reads": "14998",
        ...

## <a name="waiting-before-sending-next-request"></a>Wachten voordat u volgende aanvraag verzendt

Wanneer u de aanvraaglimiet van de hebt bereikt, resourcemanager geeft als resultaat de statuscode **429** HTTP en een **Nieuwe poging na** waarde in de koptekst. Het **Opnieuw na** waarde geeft het aantal seconden dat uw toepassing moet wachten (en naar bed gaan) voordat u de volgende aanvraag verzendt. Als u een aanvraag verzendt voordat de waarde voor de nieuwe pogingen is verstreken, wordt uw aanvraag wordt niet verwerkt en wordt een nieuwe opnieuw waarde wordt geretourneerd.
