<properties
   pageTitle="Hoe u gegevensbronnen ontdekken | Microsoft Azure"
   description="Hoe kan ik artikel hoe te vinden van de activa geregistreerde gegevens met de gegevenscatalogus Azure, met inbegrip van zoeken en filteren en gebruiken van de mogelijkheden van de portal Azure-gegevenscatalogus markering treffer markeren."
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

# <a name="how-to-discover-data-sources"></a>Hoe u gegevensbronnen ontdekken

## <a name="introduction"></a>Inleiding
**Microsoft Azure-gegevenscatalogus** is een volledig beheerde cloudservice die als een systeem van registratie en systeem van discovery voor enterprise-gegevensbronnen fungeert. **Azure-gegevenscatalogus** is met andere woorden, helpen bij het personen ontdekken, inzichtelijk maken en gegevensbronnen, en helpt organisaties gebruikt om meer waarde van de bestaande gegevens. Als een gegevensbron met **De gegevenscatalogus Azure**is geregistreerd, wordt de metagegevens geïndexeerd door de service, zodat gebruikers eenvoudig zoeken kunnen als u wilt ontdekken van de gegevens die zij nodig hebben.

## <a name="searching-and-filtering"></a>Zoeken en filteren

Discovery in **Azure-gegevenscatalogus** gebruikt twee primaire regelingen: zoeken en filteren.

Zoeken is ontworpen om krachtige – zowel intuïtieve al dan niet standaard, zoektermen worden vergeleken met een eigenschap in de catalogus, met inbegrip van de gebruiker opgegeven aantekeningen.

Filteren is ontworpen als aanvulling op zoeken. Gebruikers kunnen specifieke kenmerken zoals experts, gegevensbrontype objecttype en tags weer te geven alleen overeenkomende gegevens activa en om te beperken zoekresultaten op overeenkomende activa ook selecteren.

Gebruikers kunnen met behulp van een combinatie van zoeken en filteren, snel navigeren de gegevensbronnen die zijn geregistreerd met **Azure-gegevenscatalogus** te vinden van de gegevensbronnen die zij nodig hebben.

## <a name="search-syntax"></a>De syntaxis van de zoekopdracht

Hoewel het gratis zoeken standaard eenvoudige en intuïtieve is, kunnen gebruikers ook syntaxis van de **Catalogus van Azure-gegevens**van zoeken gebruiken om te laten betere controle over de lijst met zoekresultaten. Zoeken in **Gegevenscatalogus van Azure-gegevens** worden ondersteund in de volgende technieken:

| Methode                 | Gebruik                                                                                                                                     | Voorbeeld                                                   |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Basiszoekopdracht              | Eenvoudige zoekprogramma's via een of meer zoektermen. Resultaten zijn elementen die overeenkomen met op een eigenschap met een of meer van de opgegeven voorwaarden. | verkoopgegevens                                                |
| Een bereik eigenschap instellen          | Retourneert alleen gegevensbronnen waar de zoekterm wordt vergeleken met de opgegeven eigenschap                                                   | naam: Financiën                                              |
| Booleaans operatoren         | Uitbreiden of beperken een zoekprogramma's via Booleaanse bewerkingen                                                                                     | Financiën niet zakelijke                                     |
| Groeperen met haakjes | Haakjes om de groep onderdelen van de query gebruiken om te bereiken van logische moeten worden geïsoleerd, met name in combinatie met Booleaans operatoren              | naam: Financiën en (tags: Q1 of labels: k2) |
| Vergelijkingsoperatoren      | Vergelijkingen dan gelijke gebruiken voor de gegevenstypen Numeriek en datum eigenschappen                                                | modifiedTime > "11-05-2014"                                 |

Zie voor meer informatie over zoeken in **Gegevenscatalogus van Azure-gegevens** , [https://msdn.microsoft.com/library/azure/mt267594.aspx](https://msdn.microsoft.com/library/azure/mt267594.aspx).

## <a name="hit-highlighting"></a>Klik op markeren
Bij het weergeven van zoekresultaten weergegeven eigenschappen die overeenkomen met de opgegeven zoektermen – zoals de naam van de activa gegevens, beschrijving en labels – gemarkeerd om aan te geven waarom de activa van een bepaalde gegevens zijn geretourneerd door een bepaalde zoekactie te vereenvoudigen.

> [AZURE.NOTE] Gebruikers kunnen veranderen treffers markering uitschakelen indien gewenst, met de schakeloptie "Markeren" in de portal **Azure-gegevenscatalogus** .

Bij het weergeven van zoekresultaten, het niet altijd mogelijk de hand liggende waarom een activum gegevens opgenomen, is ook niet met treffers markering is ingeschakeld. Omdat alle eigenschappen worden doorzocht al dan niet standaard, kan een activum gegevens vanwege een overeenkomst worden weergegeven op de eigenschap van een kolom op resourceniveau. En omdat meerdere gebruikers kunnen aantekeningen maken bij geregistreerde gegevens activa met hun eigen labels en beschrijvingen, niet alle metagegevens kan worden weergegeven in de lijst met zoekresultaten.

Naast elkaar weergeven in de standaardweergave, elke tegel weergegeven in de lijst met zoekresultaten een pictogram 'zoekterm weergave komt overeen met', zodat de gebruiker die u het aantal overeenkomsten en hun locatie snel kunt bekijken en om naar te springen ze desgewenst bevat.

 ![Druk op markeren en zoeken komt overeen met in de gegevenscatalogus van Azure-portal](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Overzicht
Een gegevensbron met **De gegevenscatalogus Azure** registreren vergemakkelijkt die gegevensbron om te detecteren en begrijpen, door structurele en beschrijvende metagegevens van de gegevensbron te kopiëren naar de catalogus-service. Als een gegevensbron is geregistreerd, kunnen gebruikers deze via filteren en zoeken vanuit binnen de portal **Azure-gegevenscatalogus** detecteren.

## <a name="see-also"></a>Zie ook
- [Aan de slag met Azure-gegevenscatalogus](data-catalog-get-started.md) zelfstudie voor meer informatie over hoe u gegevensbronnen te detecteren.
