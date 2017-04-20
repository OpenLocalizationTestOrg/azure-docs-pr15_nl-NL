<properties
    pageTitle="Het instellen van de woordenlijst voor bedrijven voor geregeld labelen | Microsoft Azure"
    description="Op How-to artikel de woordenlijst voor bedrijven in de gegevenscatalogus Azure markeren voor definiëren en gebruiken van een algemene business-woordenlijst label geregistreerd gegevens activa."
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

# <a name="how-to-set-up-the-business-glossary-for-governed-tagging"></a>Het instellen van de woordenlijst voor bedrijven voor geregeld labelen

## <a name="introduction"></a>Inleiding

Azure gegevenscatalogus biedt mogelijkheden voor gegevensdetectie bron, zodat gebruikers kunnen eenvoudig ontdekken en begrijpt de gegevensbronnen die zijn vereist voor het uitvoeren van analyse en besluiten te nemen. Deze mogelijkheden discovery maken van de grootste impact wanneer gebruikers kunnen vinden en meer informatie over een breed scala aan beschikbare gegevensbronnen.

Eén gegevenscatalogus-functie die beter inzicht in gegevens van de activa promoot is labelen. Labelen gebruikers kunnen trefwoorden koppelen aan een actief of een kolom, die op zijn beurt gemakkelijker te vinden van de activa via zoeken of bladeren, en gebruikers kunnen de context en het doel van de activa gemakkelijker te begrijpen.

Echter kan labelen problemen veroorzaken eigen. Enkele voorbeelden van problemen die kunnen worden ingevoerd door labelen zijn:

1.  Gebruikers met afkortingen op sommige activa en uitgebreide tekst op anderen terwijl labelen. Hoewel de bedoeling is de activa taggen met dezelfde tag, kunnen deze inconsistenties in het detecteren van activa.
2.  Labels die verschillende dingen in verschillende contexten betekenen. Bijvoorbeeld een tag "Opbrengst" genoemd in een gegevensverzameling klant mogelijk gemiddelde omzet per klant, maar dezelfde tag op een kwartaal verkoop gegevensset kwartaal inkomsten voor het bedrijf kan betekenen.  

Gegevenscatalogus bevat deze en andere soortgelijke uitdagingen adres, zodat een woordenlijst voor bedrijven.

De gegevenscatalogus met zakelijke woordenlijst kunnen organisaties document belangrijke zakelijke voorwaarden en de definities maken van een algemene business-woordenlijst. In dit beheermodel kunt consistentie in gegevensgebruik binnen de organisatie. Zodra termen zijn gedefinieerd in de woordenlijst voor bedrijven, ze kunnen worden toegewezen aan elementen van de gegevens in de catalogus met dezelfde aanpak als labelen, waardoor _moet voldoen aan labelen_.

> [AZURE.NOTE] De functionaliteit voor beschreven in dit artikel zijn alleen beschikbaar in de standaard-editie van Azure gegevenscatalogus. De gratis versie biedt geen mogelijkheden voor geregeld labelen of een woordenlijst voor bedrijven.

## <a name="glossary-availability-and-privileges"></a>Beschikbaarheid van de woordenlijst en bevoegdheden

*De woordenlijst voor bedrijven is beschikbaar in de standaard-editie van Azure gegevenscatalogus. De gratis Edition van gegevenscatalogus bevat geen een woordenlijst.*

De woordenlijst voor bedrijven kan worden geraadpleegd via de optie 'Woordenlijst' in het navigatiemenu van de portal van de gegevenscatalogus.  

![Toegang krijgen tot de woordenlijst voor bedrijven](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)


Beheerders van de catalogus met gegevens en leden van de rol van de woordenlijst voor beheerders kunnen maken, bewerken en verwijderen van termen uit de woordenlijst in de woordenlijst voor bedrijven. Alle gebruikers van de gegevenscatalogus de term definities kunnen bekijken en kunnen labelen activa met termen uit de woordenlijst.

![Een nieuwe term in de woordenlijst toevoegen](./media/data-catalog-how-to-business-glossary/02-new-term.png)


## <a name="creating-glossary-terms"></a>Termen maken

Beheerders van de catalogus met gegevens en woordenlijst-beheerders kunnen termen in de nieuwe maken door te klikken op de nieuwe Term' knop termen maken met de volgende velden:

* Een business-definitie voor uw nieuwe term
* Een beschrijving die wordt vastgelegd de bestemming of bedrijfsregels voor de activa/kolom
* Een lijst met belanghebbenden die het meest over de term kennen
* De bovenliggende term, waarin de hiërarchie waarin de term is ingedeeld


## <a name="glossary-term-hierarchies"></a>Woordenlijst term hiërarchieën

De gegevenscatalogus bedrijven woordenlijst biedt de mogelijkheid om te beschrijven van uw zakelijke woordenlijst als een hiërarchie van termen. Hierdoor organisaties een classificatie van termen waarmee beter hun taxonomie bedrijven maken.

De naam van een term moet uniek zijn op een bepaald niveau van de hiërarchie - dubbele namen zijn niet toegestaan. Er is geen beperking voor het aantal niveaus in een hiërarchie, maar een hiërarchie is vaak gemakkelijker te begrijpen wanneer er drie niveaus of minder zijn.

Het gebruik van hiërarchieën in de woordenlijst voor bedrijven is optioneel. De bovenliggende term veld leeg blijft voor termen in maakt een platte (niet-hiërarchische)-lijst met voorwaarden in de woordenlijst.  

## <a name="tagging-assets-with-glossary-terms"></a>Sociaalnetwerklabels activa met termen uit de woordenlijst

Zodra termen uit de woordenlijst zijn gedefinieerd in de catalogus, wordt de ervaring van activa labelen is geoptimaliseerd voor het zoeken van de woordenlijst voor de gebruiker typt hun tag. De gegevenscatalogus-portal bevat een overzicht van het vergelijken van termen uit de woordenlijst voor de gebruiker en kies uit. Als de gebruiker selecteert een woordenlijst-term in de lijst worden toegevoegd aan de activa als een tag (ook woordenlijst-code). De gebruiker kunt ook een nieuwe tag maken door een term die niet in de woordenlijst (ook typen gebruikerscode).

![Gegevens van activa die is gelabeld met één gebruiker tag en twee woordenlijst tags](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [AZURE.NOTE] Labels van de gebruiker zijn de enige type markering worden ondersteund in de gratis Edition van gegevenscatalogus.

### <a name="hover-behavior-on-tags"></a>Plaats de muisaanwijzer gedrag van labels
In de gegevenscatalogus-portal zijn de twee soorten tags visueel distinct, met verschillende hover invoegen. Wanneer de gebruiker op een tag gebruiker rust kunnen ze de labeltekst en de gebruiker of gebruikers die de tag toegevoegd zien. Wanneer de gebruiker op een tag woordenlijst rust, zien ze ook de definitie van de term en een koppeling naar de woordenlijst voor bedrijven om weer te geven van de volledige definitie van de term te openen.

### <a name="search-filters-for-tags"></a>Zoekfilters naar tags
Zowel de woordenlijst tags en de gebruiker tags doorzoekbaar zijn en kunnen worden toegepast als filters in een zoekopdracht.

## <a name="summary"></a>Overzicht
De woordenlijst voor bedrijven in de gegevenscatalogus Azure en de geregeld labelen kunt, kunt gegevens activa kunnen worden geïdentificeerd, beheerd en ontdekt op consistente wijze. De woordenlijst voor bedrijven kunt promoveren learning van de woordenlijst bedrijven mee gebruikers van een organisatie en betekenisvolle metagegevens om te worden opgenomen, ondersteunt activa discovery bellen, en informatie over heel gemakkelijk.

## <a name="see-also"></a>Zie ook

- [REST API-documentatie voor zakelijke woordenlijst bewerkingen](https://msdn.microsoft.com/library/mt708855.aspx)
