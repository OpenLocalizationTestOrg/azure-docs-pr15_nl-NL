<properties
    pageTitle="Indexen Azure zoeken met behulp van de Azure-Portal maken | Microsoft Azure | De zoekservice gehoste cloud"
    description="Een index met behulp van de Azure-Portal maken."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Indexen Azure zoeken met behulp van de Azure-Portal maken
> [AZURE.SELECTOR]
- [Overzicht](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)

In dit artikel wordt u begeleid bij het proces van het maken van een Azure zoeken [index](search-what-is-an-index.md) met behulp van de Azure-Portal.

Voordat u deze handleiding volgen en een index maken, hebt u al [gemaakt van een Azure-zoekservice](search-create-service-portal.md).


## <a name="i-go-to-your-azure-search-blade"></a>Ik. Ga naar uw blade Azure zoeken
1. Klik op 'Alle resources' in het menu aan de linkerkant van de [Azure-Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Selecteer de zoekservice van Azure

## <a name="ii-add-and-name-your-index"></a>II. Toevoegen en de naam van uw index
1. Klik op de knop "Toevoegen index"
2. Geef een naam de zoekindex van Azure. Omdat u een index om te zoeken naar hotels in deze handleiding, hebben we onze index "hotels" naam.
  * De indexnaam van de moet beginnen met een letter en alleen kleine letters, cijfers of streepjes bevatten ("-").
  * Vergelijkbaar met de servicenaam van uw, de naam van de index die u kiest worden ook deel uit van de URL van het eindpunt waar u uw HTTP-aanvragen voor de API voor het zoeken van Azure verzendt
3. Klik op het fragment 'Velden' openen van een nieuwe blade

![](./media/search-create-index-portal/add-index.png)


## <a name="iii-create-and-define-the-fields-of-your-index"></a>III. Maken en de velden van de index te definiëren
1. Als u het fragment "Velden" selecteert, wordt een nieuwe blade geopend met een formulier voor het invoeren van de indexdefinitie van uw.
2. Velden toevoegen aan de Zijtabs met letter toe aan het formulier.

  * *Een sleutelveld van het type Edm.String* is verplicht voor elke Azure zoekindex. In dit veld met de sleutel wordt gemaakt met het veld naam "id" standaard. We hebben deze naar "hotelId" gewijzigd in onze index.
  * Bepaalde eigenschappen van uw schema index slechts eenmaal kunnen worden ingesteld en in de toekomst kunnen niet worden bijgewerkt. Reden zijn schema updates waarvoor het opnieuw indexeren zoals het wijzigen van veldtypen niet momenteel mogelijk na de eerste configuratie.
  * We hebt de waarden van de eigenschappen voor elk veld op basis van hoe we dat ze worden gebruikt in een toepassing zorgvuldig gekozen. Houd er rekening mee uw zoekopdracht gebruiker ervaring en zakelijke behoeften bij het ontwerpen van de Zijtabs met letter zoals elk veld dat de [gewenste eigenschappen](https://msdn.microsoft.com/library/azure/dn798941.aspx)moet worden toegewezen. Deze eigenschappen bepalen welke functies (filteren, faceting, sorteren, zoeken in volledige tekst, enzovoort) zoeken van toepassing op welke velden. Bijvoorbeeld, is het waarschijnlijk personen zoeken naar hotels geïnteresseerd trefwoord overeenkomsten op het beschrijvingsveld '', zodat we zoeken in volledige tekst voor dat veld met de eigenschap "Searchable" inschakelen.
    * U kunt ook de [taal analyzer](https://msdn.microsoft.com/en-us/library/azure/dn879793.aspx) voor elk veld instellen door te klikken op het tabblad "Analyzer" aan de bovenkant van het blad. U kunt zien onder dat we een Franse analyzer voor een veld in onze index bedoeld voor Franse tekst hebt geselecteerd.

3. Klik op **OK** in het blad 'Velden' om te bevestigen van uw velddefinities
4. Klik op **OK** op het blad "Toevoegen index" op te slaan en het maken van de index die u zojuist hebt gedefinieerd.

In de onderstaande schermafbeeldingen ziet u hoe we hebben met de naam en de velden voor onze index 'hotels' gedefinieerd.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next"></a>Volgende
Na het maken van een Azure zoekindex, bent u klaar uw inhoud in de index te [uploaden](search-what-is-data-import.md) zodat u kunt beginnen met het zoeken van uw gegevens.
