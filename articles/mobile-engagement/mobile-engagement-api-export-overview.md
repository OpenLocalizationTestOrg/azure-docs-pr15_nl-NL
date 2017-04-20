<properties
    pageTitle="Overzicht van mobiele betrokkenheid exporteren API"
    description="Informatie over de basisbeginselen over het exporteren van uw onbewerkte gegevens die zijn gegenereerd door de apparaten van de gebruiker om te profiteren van deze in uw eigen hulpmiddelen"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="kpiteira"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile"
    ms.date="04/26/2016"
    ms.author="kpiteira"/>

# <a name="mobile-engagement-export-api-overview"></a>Overzicht van mobiele betrokkenheid exporteren API

## <a name="introduction"></a>Inleiding

In dit document leert u de basisbeginselen over het exporteren van uw onbewerkte gegevens die zijn gegenereerd door de apparaten van de gebruiker om te profiteren van deze in uw eigen hulpmiddelen.

## <a name="pre-requisites"></a>Minimumvereisten

De onbewerkte gegevens exporteren uit Mobile betrokkenheid vereist:

- API verificatie instellen om te kunnen gebruiken de API's (Zie [verificatie handmatige instelling](mobile-engagement-api-authentication-manual.md)),
- Gebruikt u de REST API's of de [.net SDK](mobile-engagement-dotnet-sdk-service-api.md),
- Een opslag van Azure-account.

>[AZURE.NOTE] Ook wordt geadviseerd de bronnen met uitstekende [Microsoft Azure opslag Explorer](http://storageexplorer.com/)ten minste tijdens de ontwikkelingsfase aangezien deze een eenvoudig te gebruiken gebruikersinterface voor de communicatie met Azure opslagruimte biedt.

## <a name="what-can-be-exported"></a>Wat kan worden geëxporteerd?

Mobile betrokkenheid kunnen de gebruikers voor het verzamelen van groot aantal typen gegevens en daarom heeft verschillende soorten exporteren die geschikt zijn voor deze verschillende gegevenstypen.
Er zijn 2 essentiële soorten exporteren:

- Momentopname: gebruikt om gegevens te exporteren vertegenwoordigt die meestal een status en waarvoor Mobile betrokkenheid heeft geen een geschiedenis. Dit geldt ook voor labels (app-info), tokens of push campaign feedback bijvoorbeeld. Dit soort exporteren als gevolg hiervan niet zijn gerelateerd aan een datum.
- historische: dit type exporteren wordt gebruikt voor gegevens die zich na verloop van tijd zoals gebeurtenissen of activiteiten bijvoorbeeld.

De onderstaande tabel worden beschreven uitgebreid alle de mogelijke uitvoer:

| Type exporteren | Gegevenstype | Beschrijving                                                                                                                                 |
|-------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Momentopname    | Pushmeldingen      | Genereert een exporteren van Push campagnes feedback op basis van de per deviceid/gebruikers-id                                                              |
| Momentopname    | Tag       | Genereert een uitvoer van de labels (app-info), die is gekoppeld aan elke apparaten                                                                       |
| Momentopname    | Apparaat    | Genereert een uitvoer van het grootste deel van de gegevens over apparaten, zoals de technicals (model, landinstelling, tijdzoneverschil,...), de labels, eerst gezien... |
| Momentopname    | Token     | Genereert een uitvoer van de geldige tokens                                                                                                 |
| Historische  | Activiteit  | Genereert een exporteren van alle activiteiten voor elke apparaten op een bepaalde periode                                                           |
| Historische  | Gebeurtenis     | Genereert een exporteren van alle activiteiten voor elke apparaten op een bepaalde periode                                                           |
| Historische  | Taak       | Genereert een uitvoer van alle taken voor elke apparaten op een bepaalde periode                                                                 |
| Historische  | Fout     | Genereert een uitvoer van alle fouten voor elke apparaten op een bepaalde periode                                                               |

## <a name="how-does-it-work"></a>Hoe werkt dit?

Exports zijn te lang actieve taken die kan leiden tot grote gegevensbestanden. Om die reden niet ze aanroepen om terug te keren direct een bestand wilt downloaden.
Om te kunnen gegevens exporteren vanuit mobiele betrokkenheid, hebt u een **Taak exporteren** via API wilt maken waarin u doorgaans opgeven:

- Het type exporteren (momentopname of historische)
- Het gegevenstype
- De **Container van Azure-opslag** (met inbegrip van een geldige SA's met schrijftoegang) waar het resultaat van de export wordt geschreven.

Houd er rekening mee dat het kan enkele minuten duren voordat uw taak om te worden gestart en vervolgens deze van een paar seconden voor kleine apps enkele uren voor apps gebruiken met een groot aantal gebruikers of activiteit uitvoeren kan.

Nadat de taak is gemaakt, is het mogelijk om te controleren van de status ervan om te zien als deze nog actief is of als deze is voltooid.

Nadat de taak is voltooid, is het resulterende gegevensbestand is beschikbaar op de container meegeleverde opslag.
