<properties
    title="required"
    pageTitle="Aangepaste korting extensies gebruikt in onze technische artikelen"
    description="Hier staan de aangepaste korting uitbreidingen waarmee insluiten van video's, notities en tips herbruikbare inhoud en ander item in de technische artikelen azure.microsoft.com."
    services=""
    solutions=""
    documentationCenter=""
    authors="tysonn"
    manager="carolz"
    editor=""/>

<tags
    ms.service="contributor-guide"
    ms.devlang=""
    ms.topic="article"
    ms.tgt_pltfrm=""
    ms.workload=""
    ms.date="01/22/2015"
    ms.author="tysonn"/>

## <a name="markdown-for-azuremicrosoftcom"></a>Korting voor Azure.microsoft.com

Zie voor algemene korting tips [Korting basisbeginselen](https://help.github.com/articles/markdown-basics/) en onze [Cheatsheet voor de korting](./media/documents/markdown-cheatsheet.pdf?raw=true). Als u maken van artikel kruiskoppelingen in korting wilt, raadpleegt u de [koppelen richtlijnen] (. / create-links-markdown.md#markdown-syntax-for-acom-relative-links.md/).

Azure.Microsoft.com ondersteunt [omheind codeblokken](https://help.github.com/articles/github-flavored-markdown/#fenced-code-blocks) en [syntaxis van de markering](https://help.github.com/articles/github-flavored-markdown/#syntax-highlighting). ACOM ondersteunt echter slechts één syntaxis van de markering kleurenschema, ongeacht de taal die u in een codeblok opgeeft.

## <a name="custom-markdown-extensions-used-in-our-technical-articles"></a>Aangepaste korting extensies gebruikt in onze technische artikelen

Onze artikelen GitHub flavored korting gebruiken voor de meeste artikel opmaak - van alinea's, koppelingen, lijsten, koppen, enzovoort. Maar we gebruiken aangepaste korting extensies waar we nodig rijkere opmaak in de weergegeven pagina's op azure.microsoft.com. Hier ziet u het selectievakje extensies die we momenteel gebruikt:

+ [Notities en tips]
+ [Bevat]
+ [Ingesloten video 's]
+ [Technologie en platform-selectors]

## <a name="notes-and-tips"></a>Notities en tips

U kunt kiezen uit 4 soorten notities en tips:

- AZURE. OPMERKING
- AZURE. WAARSCHUWING
- AZURE. TIPss
- AZURE. BELANGRIJKE

###<a name="usage"></a>Gebruik
Gebruik in het algemeen, notities en tips Wees spaarzaam met overal in uw artikelen. Wanneer u deze gebruikt, kiest u het juiste type notitie of tip:

- AZURE gebruikt. Opmerking neutrale of positieve informatie die wordt benadrukt of aanvullingen belangrijkste punten van de belangrijkste tekst markeren. Een notitie verstrekt die van toepassing alleen in speciale gevallen is.

  ![](./media/custom-markdown-extensions/Notes-note.PNG)

- AZURE gebruikt. Waarschuwing over de gebruiker aan een voorwaarde die ertoe dat een probleem in de toekomst leiden kan. Bijvoorbeeld een bepaalde optie te selecteren of het maken van een bepaalde keus mogelijk permanent vergrendelen u in een bepaald scenario.

  ![](./media/custom-markdown-extensions/Notes-warning.PNG)

- AZURE gebruikt. TIP om uw gebruikers de technieken beschreven en procedures in de tekst aan de behoeften van hun specifieke toepassen. Een tip mogelijk ook alternatieve methoden die mogelijk niet duidelijk voorstellen. Tips, zijn echter niet essentieel voor de basiskennis van de tekst.

  ![](./media/custom-markdown-extensions/Notes-tip.PNG)

- AZURE gebruikt. BELANGRIJKE informatie die essentieel is voor de voltooiing van een taak te verstrekken.

  ![](./media/custom-markdown-extensions/Notes-important.PNG)

Terwijl deze notities en tips wordt ondersteund in codeblokken, probeert afbeeldingen, lijsten en koppelingen, u te houden van uw notities en tips eenvoudig. Als u merkt dat complexe notities maken met een groot aantal opmaak, mogelijk die een teken dat u hoeft u alleen maar een andere sectie in de hoofdtekst van het artikel. En te veel van uw notities in een artikel kunnen zijn storende en moeilijk te scannen of lezen.

###<a name="sample-markdown"></a>Voorbeeld korting

De alle voorbeelden wordt een AZURE. HOUD ER REKENING MEE. Als u wilt een TIP, waarschuwing of belangrijk gebruikt, vervangt u "NOTITIE" in de korting:

    > [AZURE.TIP]

    > [AZURE.WARNING]

    > [AZURE.IMPORTANT]

Eén alinea:

    > [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Microsoft Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten.

Multiparagraph:

    > [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Microsoft Azure-account hebt.
    >
    > Als u geen account hebt, kunt u [een gratis proefabonnement-account maken](http://www.windowsazure.com/pricing/free-trial/) in een paar minuten.

## <a name="includes"></a>Bevat

Herbruikbare tekst in onze GitHub-bibliotheek bevindt zich in de bestanden die verwijst naar de "bevat". Wanneer u de tekst die moet worden gebruikt in meerdere artikelen hebt, kunt u een verwijzing naar dat bestand van herbruikbare gegevens opnemen. Het zelf is een eenvoudige korting (.md)-bestand. Een geldige korting, inclusief tekst, koppelingen en afbeeldingen kan bevatten. Alle bestanden moeten zich in korting opnemen [de / bevat map](https://github.com/Azure/azure-content/tree/master/includes) in de hoofdmap van de bibliotheek. Wanneer het artikel wordt gepubliceerd, wordt de tekst opnemen is volledig geïntegreerd in de gepubliceerde onderwerp.

- We een specifieke syntaxis gebruiken om te verwijzen naar een opnemen.

- Media-bestanden die u hebt opgeslagen in een opnemen moeten worden gemaakt in een map media specifiek zijn voor het opnemen. Media-mappen voor bevat deel uitmaakt van [de](https://github.com/Azure/azure-content/tree/master/includes/media)map azure-inhoud/bevat/media. De map media mogen geen afbeeldingen in de hoofdmap. Als er geen afbeeldingen voor het opnemen, klikt u vervolgens is een bijbehorende media-map niet vereist.

###<a name="usage"></a>Gebruik

- Gebruik bevat waar u dezelfde tekst wordt weergegeven in meerdere artikelen nodig hebt.

- Bevat zijn bedoeld voor grote hoeveelheden inhoud - een alinea of twee, een gedeelde procedure of een gedeelde sectie moet worden gebruikt. Gebruik deze niet voor alles wat kleiner is dan een zin; **ze zijn niet voor productnamen**.

- Controleer of alle de tekst in een opnemen in de volledige zinnen of zinnen die niet afhankelijk voorgaande tekst of het volgende tekstvak in het artikel dat verwijst naar de opnemen is geschreven. Deze instructies negeren Hiermee maakt u een untranslatable tekenreeks in het artikel die de gelokaliseerde ervaring eindemarkeringen. 

- Niet insluiten vanuit andere bevat bevat. Ze worden niet ondersteund door de DPS systeem publiceren.

- Niet media tussen bestanden delen. Gebruik een afzonderlijk bestand met een unieke naam voor elke opnemen en artikel. Sla het mediabestand in de media-map die is gekoppeld aan het opnemen.

- Gebruik geen een opnemen als alleen uit de inhoud van een artikel.  Bevat zijn bedoeld voor de inhoud in de rest van het artikel aanvullende.

- Omdat alle bevat, moeten zich in de / directory, bevat het pad naar een opnemen van een artikel is altijd

    .. / bevat

- De verwijzing naar een koppeling of afbeelding bestandsnaam in het artikel zowel de opnemen niet worden herhaald. Toevoegen '-opnemen ' op de koppeling verwijzing of een media bestandsnaam herhaling van de verwijzing:

 **Verwijzing naar de koppeling**

 Wijzigen: odata.org naar: odata.org opnemen

 **Afbeelding van de verwijzing**

 Wijzigen: table.png naar: tabel-include.png

###<a name="sample-markdown"></a>Voorbeeld korting
De syntaxis voor het toevoegen van een opnemen naar een artikel documentatie is:

    [AZURE.INCLUDE [include-short-name](../includes/include-file-name.md)]

Voorbeeld

    [AZURE.INCLUDE [howto-blob-storage](../includes/howto-blob-storage.md)]

Het eerste deel van het opnemen is de naam opnemen zonder het pad en zonder de extensie .md. Het tweede onderdeel is het relatieve pad naar de opnemen in de / bevat map, met de extensie .md.

###<a name="rendering"></a>Weergave

In de pagina GitHub, wordt de opnemen als volgt weergegeven:

 [AZURE. Procedure-blobopslag opnemen]

In de gegenereerde HTML op azure.microsoft.com, de HTML-code van de bevat de rest van HTML-code van het document is samengevoegd. De HTML-code bevat echter een HTML opmerking met de oorspronkelijke korting bestandsnaam en de GitHub doorvoeren hash opnemen. Deze opmerking is inbegrepen voor het oplossen van problemen, zodat de broninhoud gemakkelijk kan worden geïdentificeerd en zijn gevonden in GitHub:

  ![](./media/custom-markdown-extensions/include.png)


## <a name="embedded-videos"></a>Ingesloten video 's

Als de video's op Microsofts [kanaal 9](http://channel9.msdn.com/) -site zijn, onze technische artikelen embeddeded video's ondersteunen in technische artikelen. De video's van kanaal 9 moeten worden geïntegreerd met [het azure.microsoft.com Video Center](http://azure.microsoft.com/documentation/videos/home/). We ondersteuning momenteel geen voor ingesloten YouTube-video's; Als u een inzender community bent, bent u Welkom bij de koppeling naar YouTube als de video die u wilt aanbevelen er is geplaatst. Microsoft inzenders moeten gebruiken kanaal 9 en de Video-beheercentrum.

### <a name="usage"></a>Gebruik

- Zorg ervoor dat de video op het Video-beheercentrum.

- Kopieer de video-ID van de beschrijvende URL van de video over het kanaal 9 of van het midden van de Video Azure. De video-ID voor de video op [http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/](http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/) is bijvoorbeeld **azure-scheduler-ongebruikelijke-planningen**.

### <a name="syntax"></a>Syntaxis

    > [AZURE.VIDEO video-id-string]

### <a name="rendering"></a>Weergave

Aan GitHub: [https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md](https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md)

Gepubliceerde artikel: [http://azure.microsoft.com/documentation/articles/web-sites-backup/](http://azure.microsoft.com/documentation/articles/web-sites-backup/)


## <a name="technology-and-platform-selectors"></a>Technologie en platform-selectors

Gebruik technologie en platform switchers in technische artikelen wanneer u meerdere versies van hetzelfde artikel adres verschillen in uitvoering over technologieën of platforms creëert. Dit is meestal meest van toepassing op de inhoud van onze mobiele platform voor ontwikkelaars. Er zijn twee verschillende soorten selectors, [eenvoudige selectors](#simple-selectors) en [twee richtingen selectors](#two-way-selectors).

Omdat de dezelfde Gegevenskiezer korting vindt u in elk onderwerp in de selectie, wordt aangeraden de kiezer voor uw onderwerp plaatsen in een opnemen en klik vervolgens verwijst naar die opnemen in al uw onderwerpen die de dezelfde Gegevenskiezer gebruiken.

###<a id="simple-selectors"></a>Eenvoudige selectors

Eenvoudige (één richting) selectors worden weergegeven als een set met keuzerondjes direct onder de titel. Gebruik deze knoppen wanneer klanten moeten van onderwerpen in een enkel platform of technologie set, zoals .NET, Node.js en Java kiezen.  De aangepaste korting-extensie voor elke selectors - gebruik geen HTML voor selectors.  

Zie [aan de slag met melding Hubs](http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started/) om te zien hoe de auteur 8 versies van de dezelfde artikel, maar de gebruikte selectors om te navigeren via deze allemaal gemaakt.

![Voorbeeld van de eenvoudige Gegevenskiezer](./media/custom-markdown-extensions/selectors.PNG)

####<a name="syntax"></a>Syntaxis

    > [AZURE.SELECTOR]
    - [Koppeling #1-Label](link #1 url)
    - [koppeling #2-Label](link #2 url)

Voorbeeld:

    > [AZURE.SELECTOR]
    - [Universele Windows](../articles/notification-hubs-windows-store-dotnet-get-started/)
    - [Windows Phone](../articles/notification-hubs-windows-phone-get-started/)
    - [iOS](../articles/notification-hubs-ios-get-started/)
    - [Android](../articles/notification-hubs-android-get-started/)
    - [Kindle](../articles/notification-hubs-kindle-get-started/)
    - [Baidu](../articles/notification-hubs-baidu-get-started/)
    - [Xamarin.iOS](../articles/partner-xamarin-notification-hubs-ios-get-started/)
    - [Xamarin.Android](../articles/partner-xamarin-notification-hubs-android-get-started/)

#### <a name="rendering"></a>Weergave

De bovenstaande afbeelding ziet de weergave op azure.microsoft.com. Op de weergegeven GitHub-pagina's, worden de selectors weergegeven als een lijst met opsommingstekens van koppelingen.

###<a id="two-way-selectors"></a>Twee richtingen selectors

Twee richtingen selectors kunnen gebruikers een onderwerpen selecteren uit een matrix twee manier. Dit is essentieel wanneer een Azure technologie, zoals Mobile-Services, meerdere backend platforms, evenals meerdere klanten ondersteunt. Houd rekening met de volgende:

- Terwijl deze is ontworpen als `(Platform | Backend)`, de tekst dropwdown kan nu worden aangepast.
- U hoeft niet een lijstitem voor elke punt in de matrix, maar alleen een item waar een onderwerp URL bestaat en niet een duplicaat moet hebben.
- De koppeling is een URL zijn, hoewel het algemeen een ander GitHub onderwerp.

Zie [aan de slag met mobiele Services](http://azure.microsoft.com/en-us/documentation/articles/mobile-services-ios-get-started/) om te zien hoe de auteur 15 versies van de dezelfde artikel (9 mobiele clientplatforms en 2 backend platforms), maar de gebruikte selectors om te navigeren via deze allemaal gemaakt. Houd er rekening mee dat 3 artikelen beide backend-versies niet hebben.

![Voorbeeld van de twee variabelen selectors](./media/custom-markdown-extensions/selector-list.png)

####<a name="syntax"></a>Syntaxis

    > [AZURE. GEGEVENSKIEZER-lijst (Dropdown1 | Dropdown2)]     -  [(Dropdown1Text1 | Dropdown2Text1)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text1 | Dropdown2Text2)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text2 | Dropdown2Text3)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text3 | Dropdown2Text4)](../articles/dropdown1-text1-dropdown2-text1.md)

Voorbeeld:

    > [AZURE. GEGEVENSKIEZER-lijst (Platform | Back-end)]     -  [(iOS | .NET)](./mobile-services-dotnet-backend-ios-get-started-push.md)
    - [(iOS | JavaScript)](./mobile-services-javascript-backend-ios-get-started-push.md)
    - [(Windows universele C# | .NET)](./mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows universele C# | JavaScript)](./mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows Phone | .NET)](./mobile-services-dotnet-backend-windows-phone-get-started-push.md)
    - [(Windows Phone | JavaScript)](./mobile-services-javascript-backend-windows-phone-get-started-push.md)
    - [(Android | .NET)](./mobile-services-dotnet-backend-android-get-started-push.md)
    - [(Android | JavaScript)](./mobile-services-javascript-backend-android-get-started-push.md)
    - [(Xamarin iOS | JavaScript)](./partner-xamarin-mobile-services-ios-get-started-push.md)
    - [(Xamarin Android | JavaScript)](./partner-xamarin-mobile-services-android-get-started-push.md)

#### <a name="rendering"></a>Weergave

De bovenstaande afbeelding ziet de weergave op azure.microsoft.com. Op de weergegeven GitHub-pagina's, worden de selectors weergegeven als een lijst met opsommingstekens van koppelingen.

<!--Anchors-->
[Notities en tips]: #notes-and-tips
[Bevat]: #includes
[Ingesloten video 's]: #embedded-videos
[Technologie en platform-selectors]: #technology-and-platform-selectors

###<a name="contributors-guide-links"></a>Inzenders de handleiding voor koppelingen

- [Van overzichtsartikel](./../README.md)
- [Index van artikelen](./contributor-guide-index.md)
