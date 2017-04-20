<properties
   pageTitle="Azure gegevenscatalogus terminologie | Microsoft Azure"
   description="In dit artikel wordt een inleiding gegeven op concepten en termen die worden gebruikt in de gegevenscatalogus Azure documentatie."
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
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-terminology"></a>Azure gegevenscatalogus terminologie

## <a name="catalog"></a>Catalogus

De catalogus met Azure-gegevens is een cloudgebaseerde metagegevens-opslagplaats waarin activa bronnen en gegevens kunnen worden geregistreerd. De catalogus fungeert als een centrale opslaglocatie voor structurele metagegevens opgehaald uit de gegevensbronnen en voor beschrijvende metagegevens die worden toegevoegd door gebruikers.

## <a name="data-source"></a>Gegevensbron

Een gegevensbron is een systeem of de container die gegevens activa beheert. Voorbeelden hiervan zijn SQL Server-databases, Oracle-databases, SQL Server Analysis Services-databases (in tabelvorm of multidimensionale) en SQL Server Reporting Services-servers.

## <a name="data-asset"></a>Gegevens van activa

Gegevens activa zijn opgenomen in de gegevensbronnen die kunnen worden geregistreerd met de catalogus met objecten. Voorbeelden hiervan zijn SQL Server-tabellen en weergaven, Oracle-tabellen en weergaven, SQL Server Analysis Services afmetingen, dimensies en KPI's en SQL Server Reporting Services-rapporten.

## <a name="data-asset-location"></a>Locatie van de activa gegevens

De catalogus slaat de locatie van een gegevensbron of gegevens activa die kan worden gebruikt om verbinding maken met de bron een clienttoepassing gebruiken. De indeling en de details van de locatie, hangen af van het gegevensbrontype. Een SQL Server-tabel kan bijvoorbeeld worden geïdentificeerd door de naam van de vier deel – servernaam, de databasenaam van de, de schemanaam van het, objectnaam – terwijl een SQL Server Reporting Services-rapport kan worden geïdentificeerd door de URL.

## <a name="structural-metadata"></a>Structurele metagegevens

Structurele metagegevens is de metagegevens opgehaald uit een gegevensbron die de structuur van een activum gegevens wordt beschreven. Dit geldt ook voor de locatie van de activa, de objectnaam en type en aanvullende type / regiospecifieke kenmerken. De structurele metagegevens voor tabellen en weergaven bevat bijvoorbeeld de namen en gegevenstypen voor de kolommen van het object.

## <a name="descriptive-metadata"></a>Beschrijvende metagegevens

Beschrijvende metagegevens zijn metagegevens waarmee het doel of de bedoeling aangeeft van een activum gegevens wordt beschreven. Meestal beschrijvende metagegevens is toegevoegd door gebruikers van de catalogus met behulp van de gegevenscatalogus van Azure-portal, maar deze kan ook worden geëxtraheerd uit de gegevensbron tijdens de registratie. Bijvoorbeeld wordt de registratie-hulpprogramma Azure-gegevenscatalogus opgehaald door de beschrijvingen van de eigenschap beschrijving in SQL Server Analysis Services en SQL Server Reporting Services en van de [uitgebreide eigenschap ms_description](https://technet.microsoft.com/library/ms190243.aspx) in SQL Server-databases, als deze eigenschappen zijn gevuld met waarden.

## <a name="request-access"></a>Toegang aanvragen

Beschrijvende metagegevens van een activum gegevens kunt informatie over hoe u toegang tot de gegevens van activa of een gegevensbron aanvragen opnemen. Deze informatie wordt weergegeven met de locatie van de activa gegevens en een of meer van de volgende opties kunt opnemen:

- Het e-mailadres van de gebruiker of een team die verantwoordelijk is voor het verlenen van toegang tot de gegevensbron.
- De URL van de beschreven proces dat gebruikers toegang krijgen tot de gegevensbron moeten volgen.
- De URL van een identiteit en toegang Beheerhulpmiddel (zoals Microsoft identiteit Manager) die kan worden gebruikt voor toegang tot de gegevensbron.
- De invoer in een vrije tekst waarmee wordt beschreven hoe gebruikers toegang hebben tot de gegevensbron.

## <a name="preview"></a>Voorbeeld

Een voorbeeld weergeven in de gegevenscatalogus Azure is een momentopname van maximaal 20 records die kunnen worden opgehaald uit de gegevensbron tijdens de registratie en opgeslagen in de catalogus met de metagegevens van de activa gegevens. De voorbeeldversie kan helpen gebruikers die kennismaken met een activum gegevens beter begrip van de functie en doel. Met andere woorden, zien voorbeeldgegevens kunt meer waardevol zijn dan ziet alleen de kolomnamen en gegevenstypen.
Voorbeelden worden alleen ondersteund voor tabellen en weergaven en u moet expliciet selecteren door de gebruiker tijdens de registratie.

## <a name="data-profile"></a>Gegevens-profiel

Een profiel gegevens in de gegevenscatalogus Azure is een momentopname van de tabel- en kolom niveau metagegevens over een activum geregistreerde gegevens die kan worden opgehaald uit de gegevensbron tijdens de registratie en opgeslagen in de catalogus met de metagegevens van de activa gegevens. De gegevens-profiel kan helpen, zodat gebruikers die kennismaken met een activum gegevens beter begrip van de functie en doel. Vergelijkbaar met voorbeelden, profielen gegevens u moet expliciet selecteren door de gebruiker tijdens de registratie.

> [AZURE.NOTE] Ophalen van een profiel gegevens kan een dure bewerking voor grote tabellen en weergaven, en de benodigde tijd voor het registreren van een gegevensbron aanzienlijk kan toenemen.

## <a name="user-perspective"></a>Gebruikersperspectief

Op Azure-gegevenscatalogus bieden iedere gebruiker beschrijvende metagegevens voor een activum geregistreerde gegevens. Elke gebruiker heeft een unieke perspectief op de gegevens en het gebruik ervan. De beheerder die verantwoordelijk is voor een server voorschrijven bijvoorbeeld de details van de serviceovereenkomst (SLA) of een back-windows; een data-steward mogelijk ontvangt u koppelingen naar de documentatie van het bedrijf verwerkt de gegevens worden ondersteund; en een analisten mogelijk Geef een beschrijving in de voorwaarden die meest relevant zijn voor andere analisten, en die het meest waardevolle voor gebruikers die u nodig hebt om te detecteren en de gegevens begrijpen.

Elk van deze perspectieven inherent waardevol zijn en met de gegevenscatalogus Azure elke gebruiker kan de gegevens bevatten die herkenbaar zijn voor deze terwijl alle gebruikers kunnen deze informatie gebruiken voor meer informatie over de gegevens en het doel.

## <a name="expert"></a>Expert

Een expert is een gebruiker die is vastgesteld dat ze een op de hoogte "expert" perspectief voor een activum gegevens. Elke gebruiker kunt zichzelf of een andere gebruiker toevoegen als een expert voor activa. Wordt vermeld als een expert brengt geen extra bevoegdheden in de catalogus van Azure-gegevens; Deze kan gebruikers deze perspectieven die meestal nuttig zijn zijn bij het controleren van de activa beschrijvende metagegevens gemakkelijk te vinden.

## <a name="owner"></a>Eigenaar

Een eigenaar is een gebruiker met extra bevoegdheden voor het beheren van een activum gegevens in Azure-gegevenscatalogus. Gebruikers kunnen eigenaar worden van geregistreerde gegevens activa en eigenaren van andere gebruikers kunnen toevoegen als collega eigenaren. Zie voor meer informatie [het beheren van gegevens activa](data-catalog-how-to-manage.md)  
> [AZURE.NOTE] Eigendom en beheer zijn alleen beschikbaar in de standaard-editie van Azure gegevenscatalogus.

## <a name="registration"></a>Registratie

Registratie is de handeling van gegevens activa metagegevens ophalen uit een gegevensbron en deze kopiëren naar de gegevenscatalogus van Azure-service. Gegevens activa dat is geregistreerd kunnen vervolgens van aantekeningen voorzien en ontdekt.

## <a name="see-also"></a>Zie ook

- [Wat is de gegevenscatalogus Azure?](data-catalog-what-is-data-catalog.md) -In dit artikel bevat een overzicht van de gegevenscatalogus van Azure-service en de waarde die deze bevat de scenario's die wordt ondersteund.

- [Aan de slag met de gegevenscatalogus Azure](data-catalog-get-started.md) - in dit artikel bevat een end-to-end-zelfstudie om te zien hoe Azure-gegevenscatalogus gebruiken voor gegevensdetectie bron.  
