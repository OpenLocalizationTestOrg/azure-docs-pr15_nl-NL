<properties
   pageTitle="Koppelingen maken in korting artikelen" description="Wordt uitgelegd hoe u de code kruiskoppelingen in korting." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="02/03/2015" ms.author="tysonn" />

# <a name="linking-guidance-for-azure-technical-content"></a>Richtlijnen voor het Azure technische inhoud koppelen

### <a name="links-from-one-acom-article-to-another"></a>Koppelingen van het ene ACOM artikel naar een andere

Een inline als koppeling wilt maken van een artikel over ACOM technische op een ander ACOM technische artikel, gebruikt u de syntaxis van de volgende koppeling:  

- Een artikel in een service directory-koppelingen naar een ander artikel in dezelfde servicemap:

  `[link text](article-name.md)`

- Een artikel bevat koppelingen van een submap service naar een artikel in de hoofdmap:

  `[link text](../article-name.md)`

- Een artikel in de hoofdmap directory koppelingen naar een artikel in een submap service: 

  `[link text](./service-directory/article-name.md)`

- Een artikel in een service submap koppelingen naar een artikel in een andere service submap:

  `[link text](../service-directory/article-name.md)`
 

## <a name="links-to-anchors"></a>Koppelingen naar ankers

U hoeft niet te maken van ankers - ze worden automatisch gegenereerd bij het publiceren van tijd voor alle H2 koppen. Het enige wat dat u moet doen is koppelingen naar de secties H2 maken.

- Koppelen aan een kop in het artikel hetzelfde:

  `[link](#the-text-of-the-H2-section-separated-by-hyphens)`  
  `[Create cache](#create-cache)`

- Koppeling naar een anker in het volgende artikel in de dezelfde submap:

  `[link text](article-name.md#anchor-name)`
  `[Configure your profile](media-services-create-account.md#configure-your-profile)`

- Koppelen aan een anker in een andere service submap:

  `[link text](../service-directory/article-name.md#anchor-name)`
  `[Configure your profile](../service-directory/media-services-create-account.md#configure-your-profile)`

Een manier om het automatiseren van koppelingen maken in uw artikelen naar de koppelingen automatisch gegenereerde anker is [MarkdownAnchorLinkGenerator - een hulpmiddel voor het genereren van anker koppelingen voor ACOM in de juiste indeling](https://github.com/Azure/Azure-CSI-Content-Tools/tree/master/Tools/ACOMMarkdownAnchorLinkGenerator).

## <a name="links-from-includes"></a>Koppelingen van bevat

Aangezien opnemen bestanden bevinden zich in een andere map, moet u meer relatieve paden gebruiken zoals hieronder wordt weergegeven. Gebruik deze indeling om een koppeling naar een artikel uit een bestand opnemen:

    [link text](../articles/service-folder/article-name.md)
    
Meer informatie over het gebruik van een ingesloten bestand in de [aangepaste korting extensies richtlijnen](custom-markdown-extensions.md#includes).

## <a name="links-in-selectors"></a>Koppelingen in selectors

Als er selectors die zijn ingesloten in een opnemen, gebruikt u deze sorteren te koppelen: 

    > [AZURE. GEGEVENSKIEZER-lijst (Dropdown1 | Dropdown2)]     -  [(Tekst1 | Voorbeeld1)](../articles/service-folder/article-name1.md)
    - [(Tekst1 | Voorbeeld2)](../articles/service-folder/article-name2.md)
    - [(Tekst2 | Voorbeeld3)](../articles/service-folder/article-name3.md)
    - [(Tekst2 | Voorbeeld4)](../articles/service-folder/article-name4.md)


## <a name="reference-style-links"></a>Verwijzingstype koppelingen

U kunt de stijl referentiekoppelingen om uw broninhoud gemakkelijker te lezen. De stijl referentiekoppelingen worden de syntaxis van de inline-koppeling vervangen door vereenvoudigd syntaxis die u kunt de lange URL's verplaatsen naar het einde van het artikel. Hier volgt durf Vuurbal van voorbeeld:

Tekst van inline:

    I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].

Koppelingsverwijzingen aan het einde van het artikel:

    <!--Reference links in article-->
    [1]: http://google.com/
    [2]: http://search.yahoo.com/  
    [3]: http://search.msn.com/

Zorg ervoor dat u de ruimte na de dubbele punt, voordat u de koppeling opnemen. Als u naar andere technische artikelen koppelingen als u bent vergeten spatie te typen, wordt de koppeling in de gepubliceerde artikel opgesplitst. 

## <a name="link-to-acom-pages-that-are-not-part-of-the-technical-documentation-set"></a>Koppeling naar ACOM-pagina's die geen deel uitmaken van het venster met technische documentatie

Als u wilt koppelen aan een pagina op ACOM (zoals een prijzen pagina, SLA pagina of iets anders dat is niet een artikel documentatie), een absolute URL gebruiken, maar de landinstelling weglaat. Het doel hier is die werken met koppelingen in GitHub en klik op de site weergegeven:

    [link text](http://azure.microsoft.com/pricing/details/virtual-machines/)


## <a name="link-to-msdn-or-technet"></a>Koppeling naar MSDN- of TechNet

Wanneer u nodig hebt om de koppeling naar MSDN- of TechNet, gebruikt u de volledige koppeling naar het onderwerp en verwijderen van de en-us taalinstelling van de koppeling. 

### <a name="use-friendly-link-text-for-all-links"></a>Beschrijvende koppelingstekst gebruiken voor alle koppelingen

De woorden die u in een koppeling opnemen moet beschrijvende - met andere woorden, deze moeten worden normale Engelse woorden of de titel van de pagina die u wilt koppelen. Gebruik geen 'Klik hier'. Dit is ongeldige voor SEO en niet voldoen voor het doel.

**Corrigeren:**

- `For more information, see the [contributor guide index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`

- `For more details, see the [SET TRANSACTION ISOLATION LEVEL](https://msdn.microsoft.com/library/ms173763.aspx) reference.`

**Onjuiste:**

- `For more details, see [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).`

- `For more information, click [here](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`


## <a name="fwlinks"></a>FWLinks

Voorkom FWLinks (onze omleiding systeem) in azure.microsoft.com inhoud. Ze moeten worden gebruikt als noodgevallen alleen wanneer u nodig hebt om een koppeling voor een pagina waarvan u nog niet weet wie de URL te maken. Bijna nooit echt nodig zijn. Voor ACOM definieert u de bestandsnaam, zodat u weet welke deze worden tijd vooraf. U kunt een koppeling met het onderwerp GUID, zodat u niet hoeft te gebruiken van een FWLink maken voor een bibliotheek onderwerp die nog niet is gepubliceerd.

Als u een FWLink op een webpagina gebruiken moet, zijn de parameter P zodat het een permanente redirect:

    http://go.microsoft.com/fwlink/p/?LinkId=389595

Wanneer u de doel-URL in het hulpmiddel FWLink plakt, moet u de landinstelling verwijderen als de Doelkoppeling ACOM, of de MSDN- of TechNet-bibliotheek is.

## <a name="remember-the-azure-library-chrome"></a>Vergeet niet de chrome Azure-bibliotheek.
Als u koppelen aan een onderwerp Azure bibliotheek die zich bevindt in onder [deze knooppunt wilt](https://msdn.microsoft.com/library/azure), moet u opgeven van de Azure chrome in de koppeling (/ azure /). De Azure chrome deelt u de ACOM Navigatieopties te klikken en wordt alleen de Azure inhoud van de MSDN-bibliotheek weergegeven. Een goed bereik koppeling ziet er zo uit:

    http://msdn.microsoft.com/library/azure/dd163896.aspx

Anders worden de pagina weergegeven in de standaardweergave van MSDN, met de volledige MSDN-structuur weergegeven.

### <a name="contributors-guide-links"></a>Inzenders de handleiding voor koppelingen

- [Van overzichtsartikel](./../README.md)
- [Index van artikelen](./contributor-guide-index.md)

<!--image references-->
[1]: ./media/create-tables-markdown/table-markdown.png
[2]: ./media/create-tables-markdown/break-tables.png
