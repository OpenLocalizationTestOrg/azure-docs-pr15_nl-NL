<properties
   pageTitle="Een index voor documenten maken in meerdere talen in Azure zoeken | Microsoft Azure | De zoekservice gehoste cloud"
   description=" Azure zoeken ondersteunt 56 talen, gebruikmaken van taal analyzers uit Lucene en natuurlijke taal Processing technologie van Microsoft."
   services="search"
   documentationCenter=""
   authors="yahnoosh"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="07/14/2016"
   ms.author="jlembicz"/>

# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Een index voor documenten maken in meerdere talen in Azure zoeken
> [AZURE.SELECTOR]
- [Portal](search-language-support.md)
- [REST](https://msdn.microsoft.com/library/azure/dn879793.aspx)
- [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)

De kracht van taal analyzers unleashing net zo eenvoudig als één eigenschap voor het instellen van een doorzoekbare veld in de definitie van de index. Nu kunt u deze stap in de portal doen.

Hieronder vindt u schermafbeeldingen van de bladen Azure-Portal voor zoekprogramma's Azure waarmee gebruikers om een schema index te definiëren. Gebruikers kunnen vanuit deze blade, alle velden maken en stelt u de eigenschap analyzer voor elk van deze.

> [AZURE.IMPORTANT] U kunt alleen een analyzer taal instellen tijdens de definitie van velden, zoals in bij het maken van een nieuwe index helemaal omhoog of wanneer u een nieuw veld toevoegt aan een bestaande index. Zorg ervoor dat u alle kenmerken, zoals de analyzer, tijdens het maken van het veld volledig opgeeft. Niet mogelijk de kenmerken bewerken of wijzigen van het type analyzer nadat u uw wijzigingen opslaan.

## <a name="define-a-new-field-definition"></a>Een nieuw velddefinitie definiëren

1. Meld u aan bij de [Portal van Azure](https://portal.azure.com) en open het blad service van de zoekservice.
2. Klik op **toevoegen index** in de opdrachtenbalk boven aan het servicedashboard om te beginnen als u een nieuwe index of open een bestaande index instellen van een analyzer voor de nieuwe velden die u aan een bestaande index toevoegt.
3. Het blad velden wordt weergegeven, zodat u de opties voor het definiëren van het schema van de index, waaronder het tabblad Analyzer gebruikt voor het kiezen van een taal analyzer.
4. In de velden, start u de velddefinitie van een door een naam, het gegevenstype te kiezen en instellen van kenmerken wilt instellen voor het veld als volledige tekst doorzoekbare, opvraagbare in zoekresultaten, kan worden gebruikt in facet navigatie structuren, sorteerbaar, enzovoort. 
5. Voordat u verdergaat met het veld next, open het tabblad **Analyzer** . 

   
![][1]
*Als u wilt een analyzer hebt geselecteerd, klikt u op het tabblad Analyzer op het blad velden*

## <a name="choose-an-analyzer"></a>Kies een analyzer

6. Blader naar het veld dat u definieert. 
7. Als u het veld als doorzoekbaar nog niet hebt gemarkeerd, klikt u op het selectievakje nu als u wilt markeren als **Searchable**.
8. Klik op het gebied Analyzer om de lijst met beschikbare analyzers weer te geven.
9. Kies de analyzer die u wilt gebruiken.

![][2]
*Selecteer een van de ondersteunde analyzers voor elk veld*

Standaard gebruik alle doorzoekbare velden de [standaard Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) namelijk taal agnostic. Zie [Ondersteuning voor talen in Azure zoeken](https://msdn.microsoft.com/library/azure/dn879793.aspx)de volledige lijst met ondersteunde analyzers.

Zodra de analyzer taal is ingeschakeld voor een veld, wordt deze met elk verzoek om een indexeren en zoeken worden gebruikt voor dat veld. Wanneer u een query ten opzichte van meerdere velden met verschillende analyzers verzendt, wordt de query onafhankelijk worden verwerkt door het juiste analyzers voor elk veld.

Veel web en mobiele toepassingen fungeren gebruikers over de hele wereld met verschillende talen. Het is mogelijk een index voor een scenario als volgt definiëren door te maken van een veld voor elke taal die wordt ondersteund.

![][3]
*De definitie van de index met het beschrijvingsveld voor elke taal die wordt ondersteund*

Als de taal van een query uitvoert agent bekend is, kan een zoekopdracht worden beperkt tot een bepaald veld met de parameter van de query **searchFields** . De volgende query wordt uitgevoerd alleen ten opzichte van de beschrijving in het Pools:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2015-02-28`

U kunt de index in de portal **Search explorer** gebruiken om te plakken in een query die vergelijkbaar is met de bovenstaande query. Zoeken explorer is beschikbaar via de balk met opdrachten in het blad service. Raadpleeg de [Query de zoekindex van Azure in de portal](search-explorer.md) voor meer informatie.

Soms is de taal van een query uitvoert agent niet bekend, zodat de query kan worden verleend ten opzichte van alle velden tegelijk. Indien nodig, kan voorkeur voor de resultaten in een bepaalde taal worden gedefinieerd met [scoren profielen](https://msdn.microsoft.com/library/azure/dn798928.aspx). Klik in het onderstaande voorbeeld overeenkomsten zijn gevonden in de beschrijving in het Engels score hoger ten opzichte van de overeenkomsten in Pools en Frans:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2015-02-28`

Als u een .NET-ontwikkelaar bent, houd er rekening mee dat u taal analyzers met behulp van de [Azure zoeken .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search)kunt configureren. De meest recente versie biedt ondersteuning voor de Microsoft taal analyzers ook.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
