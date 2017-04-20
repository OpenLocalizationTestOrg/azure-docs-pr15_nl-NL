<properties
   pageTitle="Hoe u gegevensbronnen van aantekeningen voorzien | Microsoft Azure"
   description="Hoe kan ik artikel markeren hoe u gegevens activa in de gegevenscatalogus Azure, inclusief beschrijvende namen, labels, beschrijvingen en experts van aantekeningen voorzien."
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


# <a name="how-to-annotate-data-sources"></a>Hoe u gegevensbronnen van aantekeningen voorzien

## <a name="introduction"></a>Inleiding
**Microsoft Azure-gegevenscatalogus** is een volledig beheerde cloudservice die als een systeem van registratie en systeem van discovery voor enterprise-gegevensbronnen fungeert. Met andere woorden, is de gegevenscatalogus helpen bij het personen ontdekken, inzichtelijk maken en gegevensbronnen, en helpt organisaties gebruikt om meer waarde van de bestaande gegevens. Als een gegevensbron is geregistreerd bij gegevenscatalogus, de metagegevens is gekopieerd en geïndexeerd door de service, maar het verhaal er niet beëindigen. Gegevenscatalogus kan gebruikers hun eigen beschrijvende metagegevens – zoals beschrijvingen en labels – als aanvulling op de metagegevens opgehaald uit de gegevensbron en om de gegevensbron begrijpelijker aan meer personen op te geven.

## <a name="annotation-and-crowdsourcing"></a>Aantekeningen en crowdsourcing
Iedereen beschikt over een advies. En dit is een goede ding.
Gegevenscatalogus herkent dat verschillende gebruikers verschillende perspectieven hebt in de enterprise-gegevensbronnen en dat elk van deze perspectieven waardevol kan zijn. Bekijk de volgende scenario:

* De systeembeheerder weet de serviceovereenkomst voor de servers of services die host de gegevensbron.
* De beheerder van de database kent de back-planning voor elke database en de toegestane ETL verwerking windows.
* De eigenaar van het systeem bekend is het proces voor gebruikers toegang tot de gegevensbron aanvragen.
* De data-steward weet hoe de activa en kenmerken in de gegevensbron aan het gegevensmodel enterprise toewijzen.
* De analisten weet hoe de gegevens worden gebruikt in de context van de bedrijfsprocessen die hij ondersteunt.

Elk van deze perspectieven is waardevol en gegevenscatalogus wordt gebruikgemaakt van een benadering crowdsourcing metagegevens waarmee een optie om te worden vastgelegd en worden gebruikt om een beeld krijgen van geregistreerde gegevensbronnen. Met de gegevenscatalogus-portal, kan elke gebruiker toevoegen en bewerken van eigen aantekeningen, terwijl deze wordt worden kunnen aantekeningen die door andere gebruikers zien.

## <a name="different-types-of-annotations"></a>Verschillende soorten aantekeningen
Gegevenscatalogus ondersteunt de volgende soorten aantekeningen:

| Aantekeningen     | Notities                                                                                                                                                                                                                                                                                                                                                           |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Beschrijvende naam  | Beschrijvende namen kunnen worden opgegeven tijdens het gegevensniveau activa, zodat de activa van de gegevens gemakkelijker te begrijpen. Beschrijvende namen zijn handig als de naam van het onderliggende object cryptische, verkorte of anderszins niet herkenbaar zijn voor gebruikers is.                                                                                                                            |
| Beschrijving    | Beschrijvingen kunnen worden opgegeven tijdens de gegevens van activa en het kenmerk / kolomniveaus. Beschrijvingen zijn korte tekst vrije vorm-aantekeningen die van de gebruiker beschrijven perspectief op de activa van de gegevens of het gebruik ervan.                                                                                                                                                              |
| Tags (gebruiker tags)          | Labels kunnen worden opgegeven tijdens de gegevens van activa en het kenmerk / kolomniveaus. Labels van de gebruiker zijn de gebruiker gedefinieerde labels die kunnen worden gebruikt voor het categoriseren van activa van de gegevens of kenmerken.                                                                                                                                                                                                    |
| Tags (woordenlijst tags)          | Labels kunnen worden opgegeven tijdens de gegevens van activa en het kenmerk / kolomniveaus. Woordenlijst voor labels worden centraal gedefinieerd termen die kunnen worden gebruikt voor het categoriseren van activa van de gegevens of kenmerken die met een algemene bedrijven taxonomie. Zie voor meer informatie [het instellen van de woordenlijst voor bedrijven voor geregeld labelen](data-catalog-how-to-business-glossary.md)                                                                                                                                                                                                    |
| Experts        | Experts kunnen worden opgegeven tijdens het niveau van de activa. Experts kunnen identificeren van gebruikers of groepen met expert perspectieven op de gegevens en dienen als punten van contactpersoon voor gebruikers die de geregistreerde gegevensbronnen ontdekken en vragen hebt die niet door de bestaande aantekeningen worden beantwoord.  |
| Toegang aanvragen | Verzoek om toegang krijgen tot gegevens kan worden opgegeven tijdens het niveau van de activa. Deze gegevens zijn voor gebruikers die een gegevensbron die ze nog niet machtigingen hebt voor toegang tot ontdekken. Gebruikers van het e-mailadres van de gebruiker of groep die toegang, de URL van het proces of elk hulpmiddel die gebruikers moeten toegang verleent kunnen invoeren of het proces zelf als tekst kunnen invoeren. |
| Documentatie | Documentatie kan worden opgegeven tijdens het niveau van de activa. Activa documentatie vindt u informatie over tekst met opmaak dat koppelingen en afbeeldingen kunt opnemen, en waarin alle informatie die niet worden overgebracht tot en met de beschrijvingen en labels kan leveren. |


## <a name="annotating-multiple-assets"></a>Aantekeningen maken van meerdere activa
Als u meerdere gegevens activa selecteert in de gegevenscatalogus-portal, kunnen gebruikers aantekeningen maken bij alle geselecteerde activa in één bewerking. Aantekeningen wordt toegepast op alle geselecteerde activa, zodat u gemakkelijk kunt selecteren en geef een consistente beschrijving en sets met labels en experts bij gerelateerde gegevens activa.

> [AZURE.NOTE] Labels en experts kunnen ook worden geleverd wanneer registreren gegevens activa gebruik van de gegevens van de gegevenscatalogus registratie van gegevensbron.

Wanneer het selecteren van meerdere tabellen en weergaven, alleen kolommen dat alle gegevens activa elkaar gemeen hebben ingeschakeld wordt weergegeven in de portal gegevenscatalogus. Hiermee kunnen gebruikers op te geven van de knoppen labels en beschrijvingen van alle kolommen met dezelfde naam voor alle geselecteerde activa.

## <a name="annotations-and-discovery"></a>Aantekeningen en discovery
Net zoals de metagegevens opgehaald uit de gegevensbron tijdens de registratie wordt toegevoegd aan de zoekindex gegevenscatalogus, wordt ook de gebruiker opgegeven metagegevens geïndexeerd. Dit betekent dat niet alleen aantekeningen kunnen u eenvoudiger voor gebruikers voor meer informatie over de gegevens die ze detecteren, aantekeningen ook gemakkelijker voor gebruikers te vinden van de activa geannoteerde gegevens door te zoeken met de voorwaarden zinvolle hierop toe.

## <a name="summary"></a>Overzicht
Een gegevensbron met de gegevenscatalogus registreren, maakt die gegevens makkelijker kan worden gevonden door structurele en beschrijvende metagegevens van de gegevensbron te kopiëren naar de catalogus-service. Als een gegevensbron is geregistreerd, kunnen gebruikers aantekeningen om gemakkelijker te detecteren en begrijpen van binnen de portal gegevenscatalogus opgeven.

## <a name="see-also"></a>Zie ook
- [Aan de slag met Azure-gegevenscatalogus](data-catalog-get-started.md) zelfstudie voor meer informatie over hoe u gegevensbronnen van aantekeningen voorzien.
