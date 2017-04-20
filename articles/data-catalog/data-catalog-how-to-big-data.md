<properties
   pageTitle="Het werken met gegevensbronnen 'big data' | Microsoft Azure"
   description="Hoe kan ik artikel patronen voor het gebruik van de gegevenscatalogus Azure met 'big data'-gegevensbronnen, waaronder Azure-blobopslag, Lake van Azure-gegevens en Hadoop HDFS markeren."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Het werken met grote gegevensbronnen in Azure-gegevenscatalogus

## <a name="introduction"></a>Inleiding
**Microsoft Azure-gegevenscatalogus** is een volledig beheerde cloudservice die als een systeem van registratie en systeem van discovery voor enterprise-gegevensbronnen fungeert. **Azure-gegevenscatalogus** is met andere woorden, alles over helpen mensen ontdekken, te begrijpen en gegevensbronnen, en helpt organisaties gebruikt om meer waarde uit de bestaande gegevensbronnen, waaronder groot gegevens.

**Azure-gegevenscatalogus** ondersteunt de registratie van Azure Blog Storage BLOB's en mappen, evenals Hadoop HDFS bestanden en mappen. De gedeeltelijk gestructureerd aard van deze gegevensbronnen biedt uitstekende flexibiliteit, maar dat ook die gebruikers hoe de gegevensbronnen om te krijgen van de meeste waarde overwegen moeten uit geregistreerd met **De gegevenscatalogus Azure**is ingedeeld.

## <a name="directories-as-logical-data-sets"></a>Mappen als logische gegevenssets

Een algemene patroon voor het ordenen van gegevensbronnen groot is als logische gegevenssets te mappen. Op het hoogste niveau mappen worden gebruikt voor het definiëren van een gegevensverzameling, terwijl submappen partities definiëren, en bestanden opslag van de gegevens zelf.

Een voorbeeld van dit patroon mogelijk:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

In dit voorbeeld vertegenwoordigen vehicle_maintenance_events en location_tracking_events logische gegevenssets. Elk van deze mappen bevat gegevensbestanden zijn gerangschikt op jaar en maand in submappen. Elk van deze mappen zich mogelijk honderden of duizenden bestanden.

In dit patroon logisch registreren van afzonderlijke bestanden met **De gegevenscatalogus Azure** waarschijnlijk niet. Registreren in plaats daarvan de mappen met daarin de gegevensgroepen die herkenbaar zijn voor de gebruikers die werken met de gegevens.

## <a name="reference-data-files"></a>Gegevensbestanden verwijzing

Een bijbehorende patroon is voor de opslag van gegevenssets verwijzing als afzonderlijke bestanden. Deze gegevenssets mogelijk worden beschouwd als de 'kleine'-kant van grote gegevens en zijn vaak vergelijkbaar met dimensies in een model analytische gegevens. Verwijzing-gegevensbestanden bevatten de records die context bieden voor het grootste deel van de bestanden die elders is opgeslagen in de winkel groot gegevens worden gebruikt.

Een voorbeeld van dit patroon mogelijk:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Wanneer een analisten of gegevens wetenschappelijk met de gegevens in de grotere mapstructuren samen werkt, kan de gegevens in deze verwijzing-bestanden op te geven meer gedetailleerde informatie over entiteiten waarnaar wordt verwezen naar alleen op naam of -ID in de grotere gegevensset worden gebruikt.

In dit patroon, wordt het zinvol zijn de afzonderlijke verwijzing bestanden met **De gegevenscatalogus Azure**registreren. Elk bestand vertegenwoordigt een gegevensgroep en elkaar kan worden van aantekeningen voorzien en afzonderlijk ontdekt.

## <a name="alternate-patterns"></a>Alternatieve patronen

De hierboven beschreven patronen zijn slechts twee mogelijke manieren die een groot gegevensopslag kan worden georganiseerd, maar elke implementatie afwijken. Ongeacht hoe uw gegevensbronnen zijn gestructureerd, tijdens de registratie van grote gegevensbronnen met **Azure-gegevenscatalogus**, richt u voor het registreren van de bestanden en mappen met daarin de gegevensgroepen die met een bepaalde tekenreeks aan anderen in uw organisatie. Registratie van alle bestanden en mappen kunt onbelangrijke e-mail de catalogus, waardoor het moeilijker voor gebruikers om te zoeken die ze nodig hebben.

## <a name="summary"></a>Overzicht
Gegevensbronnen met **Azure-gegevenscatalogus** registreren, maakt deze gemakkelijker te vinden en te begrijpen. Via het registreren en aantekeningen de grote bestanden en mappen die logische gegevenssets voorstellen, kunt u gebruikers zoeken en gebruiken van de grote gegevensbronnen die zij nodig hebben.
