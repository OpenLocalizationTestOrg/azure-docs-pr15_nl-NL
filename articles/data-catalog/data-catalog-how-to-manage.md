<properties
   pageTitle="Het beheren van gegevens activa | Microsoft Azure"
   description="Op How-to artikel instellen met zichtbaarheid en eigendom van gegevens activa markering geregistreerd in Azure-gegevenscatalogus."
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


# <a name="how-to-manage-data-assets"></a>Het beheren van gegevens activa

## <a name="introduction"></a>Inleiding

**Azure-gegevenscatalogus** biedt mogelijkheden voor gegevensdetectie bron, zodat gebruikers kunnen eenvoudig ontdekken en begrijpt de gegevensbronnen die zijn vereist voor het uitvoeren van analyse en besluiten te nemen. Deze mogelijkheden discovery maken van de grootste impact wanneer alle gebruikers kunnen vinden en meer informatie over een breed scala aan beschikbare gegevensbronnen. Met dit in gedachten, het standaardgedrag van de gegevenscatalogus is bedoeld voor alle geregistreerde gegevensbronnen om te zijn zichtbaar voor- en makkelijker kan worden gevonden door – alle gebruikers catalogus.

Gegevenscatalogus wordt niet gebruikers toegang geven tot de gegevens zelf. Toegang tot gegevens wordt bepaald door de eigenaar van de gegevensbron. Gegevenscatalogus kan gebruikers naar gegevensbronnen ontdekken en weer te geven van de metagegevens die betrekking hebben op de bronnen die zijn geregistreerd in de catalogus.

Er zijn situaties, echter waar gegevensbronnen alleen weergegeven aan specifieke gebruikers en groepen worden moeten. Voor deze scenario's gegevenscatalogus kunnen gebruikers eigenaar van de activa geregistreerde gegevens in de catalogus en aan een besturingselement voor vervolgens de zichtbaarheid van de activa ze de eigenaar bent.

> [AZURE.NOTE] De functionaliteit voor beschreven in dit artikel zijn alleen beschikbaar in de standaard-editie van Azure gegevenscatalogus. De gratis versie biedt geen mogelijkheden voor eigendom en beperken gegevens activa zichtbaarheid.

## <a name="managing-ownership-of-data-assets"></a>Eigendom van gegevens activa beheren
Gegevens activa geregistreerd in gegevenscatalogus zijn standaard zonder eigenaar; elke gebruiker die gemachtigd voor toegang tot de catalogus kunt ontdekken en deze activa van aantekeningen voorzien. Gebruikers kunnen eigenaar van de activa zonder eigenaar gegevens en vervolgens de zichtbaarheid van de activa die ze eigenaar kunnen beperken.

Wanneer een activum gegevens in de gegevenscatalogus eigendom is, wordt alleen gebruikers die zijn geautoriseerd door de eigenaren kunnen ontdekken van de activa en weergeven van de metagegevens en wordt alleen de eigenaren van de activa kunnen verwijderen uit de catalogus.

> [AZURE.NOTE] Eigendom in gegevenscatalogus geldt alleen voor de metagegevens die zijn opgeslagen in de catalogus. Deze verleent machtigingen op de onderliggende gegevensbron.

### <a name="taking-ownership"></a>Eigenaar
Gebruikers kunnen eigenaar van de activa gegevens worden door de optie 'Eigenaar' in de portal gegevenscatalogus. Geen speciale machtigingen zijn vereist voor eigenaar worden van een actief zonder eigenaar gegevens; elke gebruiker kan de eigenaar van de activa zonder eigenaar gegevens.

### <a name="adding-owners-and-co-owners"></a>Eigenaren en eigenaren van collega toevoegen
Als het eigendom van een activum gegevens is al, kunnen geen gebruikers gewoon eigenaar: ze moeten worden toegevoegd als collega eigenaren door de eigenaar van een bestaande. Een eigenaar kunt andere gebruikers of beveiligingsgroepen toevoegen als collega eigenaren.

> [AZURE.NOTE] Dit is een goede gewoonte om ten minste twee personen als eigenaren voor activa eigendom gegevens hebt.

### <a name="removing-owners"></a>Eigenaren verwijderen
Net zoals de eigenaar van een activum collega eigenaren toevoegen kunt, kan de eigenaar van een activa een mede-eigenaar kunt verwijderen.

Als de eigenaar van een activum Hiermee verwijdert u zichzelf als een eigenaar, kan hij niet langer de activa beheren. Als de eigenaar van een activum zichzelf als een eigenaar verwijderd en er geen andere collega eigenaren, wordt de activa teruggezet naar een zonder eigenaar status.

## <a name="visibility"></a>Zichtbaarheid
Gegevens activa eigenaren kunnen de zichtbaarheid van de gegevens activa die ze eigenaar bepalen. Als u wilt beperken zichtbaarheid van de standaard-waar alle gebruikers van de gegevenscatalogus kunnen ontdekken en bekijken van de activa van gegevens: de eigenaar van de activa kunt schakelen tussen de instelling voor zichtbaarheid van 'Iedereen' naar 'Eigenaren en deze gebruikers' in de eigenschappen voor de activa. Eigenaren kunnen vervolgens specifieke gebruikers en beveiligingsgroepen toevoegen.

> [AZURE.NOTE] Indien mogelijk, moeten activa eigendom en zichtbaarheid machtigingen worden toegewezen aan beveiligingsgroepen en niet voor individuele gebruikers.

## <a name="catalog-administrators"></a>Beheerders van de catalogus
Beheerders van de gegevenscatalogus zijn impliciet collega eigenaren van alle activa in de catalogus. Activa eigenaren zichtbaarheid niet verwijderen uit de catalogus beheerders en beheerders eigendom en zichtbaarheid voor alle activa van de gegevens in de catalogus kunnen beheren.

## <a name="summary"></a>Overzicht
Van de gegevenscatalogus crowdsourcing model naar metagegevens en gegevens activa discovery kan alle catalogus gebruikers bijdragen en u ontdekt. De standaard-editie van gegevenscatalogus biedt mogelijkheden voor het eigendom en beheer om de zichtbaarheid en gebruik van de activa van specifieke gegevens te beperken.
