<properties
    pageTitle="Hoe u gegevens van de gegevensbronnen-profiel"
    description="Hoe kan ik artikel markering het opnemen van de tabel en kolom niveau gegevens profielen tijdens het registreren van gegevensbronnen in de catalogus van Azure-gegevens en het gebruik van gegevens profielen voor meer informatie over gegevensbronnen."
    services="data-catalog"
    documentationCenter=""
    authors="spelluru"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/13/2016"
    ms.author="spelluru"/>

# <a name="data-profile-data-sources"></a>Gegevensbronnen van het profiel-gegevens

## <a name="introduction"></a>Inleiding

**Microsoft Azure-gegevenscatalogus** is een volledig beheerde cloudservice die als een systeem van registratie en systeem van discovery voor enterprise-gegevensbronnen fungeert. **Azure-gegevenscatalogus** is met andere woorden, helpen bij het personen ontdekken, inzichtelijk maken en gegevensbronnen, en helpt organisaties gebruikt om meer waarde van de bestaande gegevens. Als een gegevensbron met **De gegevenscatalogus Azure**is geregistreerd, de metagegevens is gekopieerd en geïndexeerd door de service, maar het verhaal er niet beëindigen.

De functie **Gegevens profielen** van **Azure-gegevenscatalogus** worden onderzocht, de gegevens van de ondersteunde gegevensbronnen in de catalogus en statistieken en informatie over deze gegevens worden verzameld. Het is eenvoudig om op te nemen van een profiel van de activa van uw gegevens. Wanneer u een activum gegevens hebt geregistreerd, kiest u **Gegevens-profiel opnemen** in het hulpprogramma voor de registratie gegevens.

## <a name="what-is-data-profiling"></a>Wat is een beoordeling gegevens

Gegevens profielen worden onderzocht, de gegevens in de gegevensbron wordt geregistreerd en statistieken en informatie over deze gegevens worden verzameld. Tijdens de bron van gegevensdetectie kunt deze statistieken u de geschiktheid van de gegevens voor het oplossen van hun probleem bedrijven bepalen.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

De volgende gegevensbronnen ondersteunen gegevens profielen:

- SQL Server (inclusief Azure SQL database- en Azure SQL Data Warehouse) tabellen en weergaven
- Oracle-tabellen en weergaven
- Teradata-tabellen en weergaven
- Component tabellen

Inclusief gegevens profielen wanneer gegevens activa registreren gebruikers helpt beantwoorden vragen over gegevensbronnen, waaronder:

-   Kan worden gebruikt voor het oplossen van een probleem met mijn business?
-   Voldoet de gegevens aan bepaalde standaarden of patronen?
-   Wat zijn enkele van de afwijkingen van de gegevensbron?
-   Wat zijn de mogelijke uitdagingen van deze gegevens integreren in mijn toepassing?

> [AZURE.NOTE] U kunt ook documentatie toevoegen aan een actief om te beschrijven van de manier waarop gegevens in een toepassing kan worden geïntegreerd. Lees [hoe u het document gegevensbronnen](data-catalog-how-to-documentation.md).


<a name="howto"/>
## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Het opnemen van een profiel gegevens bij het registreren van een gegevensbron

Het is eenvoudig om op te nemen van een profiel van de gegevensbron. Wanneer u een gegevensbron in het deelvenster **om te worden geregistreerd objecten** van het hulpprogramma voor de registratie gegevens hebt geregistreerd, kiest u **Gegevens-profiel opnemen**.

![](media\data-catalog-data-profile\data-catalog-register-profile.png)

Meer informatie over het registreren van gegevensbronnen, raadpleegt u [hoe u kunt gegevensbronnen te registreren](data-catalog-how-to-register.md) en [aan de slag met Azure-gegevenscatalogus](data-catalog-get-started.md).


## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Filteren op gegevens activa die profielen gegevens bevatten
Als u wilt ontdekken gegevens activa die een profiel gegevens bevatten, kunt u ook `has:tableDataProfiles` of `has:columnsDataProfiles` als een van uw zoektermen.

> [AZURE.NOTE] **Gegevens-profiel opnemen** selecteren in het hulpprogramma voor de registratie gegevens bevat zowel de tabel en de profielgegevens van kolom niveau. De catalogus-API gegevens kunnen echter activa van de gegevens worden geregistreerd met slechts één set profielgegevens opgenomen.

## <a name="viewing-data-profile-information"></a>Profielgegevens gegevens weergeven

Als u een geschikte gegevensbron met een profiel hebt gevonden, kunt u de profielgegevens van de gegevens kunt weergeven. De gegevens als profiel wilt weergeven, selecteert u een activum gegevens en **Gegevens profiel** kiezen in het venster van gegevenscatalogus-portal.

![](media\data-catalog-data-profile\data-catalog-view.png)

Een profiel gegevens in **De gegevenscatalogus Azure** toont tabel en de kolom profiel informatie inclusief:

### <a name="object-data-profile"></a>Object gegevens profiel

-   Aantal rijen
-   Tabelgrootte
-   Wanneer het object voor het laatst werd bijgewerkt

### <a name="column-data-profile"></a>Kolom gegevens profiel

- Kolomgegevenstype
- Aantal unieke waarden
- Aantal rijen met NULL-waarden
- Minimum, maximum, gemiddelde en standaarddeviatie voor kolomwaarden

## <a name="summary"></a>Overzicht
Gegevens profiel bevat statistieken en informatie over geregistreerde gegevens activa om te bepalen de geschiktheid van de gegevens voor het oplossen van problemen. Samen met aantekeningen maken en gegevensbronnen documenteert, kunnen profielen gegevens geven gebruikers een beter begrip van uw gegevens.


## <a name="see-also"></a>Zie ook
-   [Het registreren van gegevensbronnen](data-catalog-how-to-register.md)
-   [Aan de slag met Azure-gegevenscatalogus](data-catalog-get-started.md)
