# <a name="azure-technical-documentation-contributor-guide"></a>Azure technische documentatie Inzender handleiding

U kunt de GitHub opslagplaats waarin bevinden zich de bron voor de technische documentatie die wordt gepubliceerd naar het beheercentrum Azure documentatie bij [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation)hebt gevonden.

Deze bibliotheek bevat ook richtlijnen u bijdragen aan onze technische documentatie.  Zie voor een lijst met de artikelen in de inzenders handleiding, [de index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).

## <a name="contribute-to-azure-documentation"></a>Bijdragen aan Azure documentatie

Bedankt voor uw belangstelling Azure documentatie.

* [Manieren om mee te](#ways-to-contribute)
* [Code van leiden](#code-of-conduct)
* [Informatie over uw bijdragen aan goede doelen tot Azure inhoud](#about-your-contributions-to-azure-content)
* [Opslagplaats organisatie](#repository-organization)
* [GitHub, cijfer en deze opslagplaats gebruiken](#use-github-git-and-this-repository)
* [Hoe u uw onderwerp opmaken met korting](#how-to-use-markdown-to-format-your-topic)
* [Feedback, opmerkingen en ondersteuning](./contributor-guide/feedback-and-comments.md)
* [Meer informatiebronnen](#more-resources)
* [Index van alle inzenders handleiding artikelen](./contributor-guide/contributor-guide-index.md) (nieuwe pagina wordt geopend)

## <a name="ways-to-contribute"></a>Manieren om mee te 

U kunt [Azure](http://azure.microsoft.com/documentation/) documentatie bijdragen in op verschillende manieren:

* Bijdragen aan een [discussie forum](http://social.msdn.microsoft.com/Forums/windowsazure/home).
* Disqus opmerkingen onderaan artikelen op verzenden.
* U kunt eenvoudig bijdragen aan technische artikelen in de gebruikersinterface GitHub. Het artikel in deze opslagplaats, zoeken of Ga naar het artikel over [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation) en klik op de koppeling in het artikel naar de bron GitHub voor het artikel.
* Als u belangrijke wijzigingen in een bestaand artikel aanbrengt, moet toevoegen of wijzigen van afbeeldingen of die bijdragen een nieuw artikel, u zich splitsen deze opslagplaats, cijfer we vaker doen, korting toetsenblok, installeren en informatie over sommige opdrachten cijfer.

##<a name="code-of-conduct"></a>Code van leiden

Dit project heeft aangenomen dat de [Microsoft Open bron Code van leiden](https://opensource.microsoft.com/codeofconduct/). Zie voor meer informatie de [Code van leiden Veelgestelde vragen](https://opensource.microsoft.com/codeofconduct/faq/) of contactpersoon [opencode@microsoft.com](mailto:opencode@microsoft.com) met extra vragen of opmerkingen.

##<a name="about-your-contributions-to-azure-content"></a>Informatie over uw bijdragen aan goede doelen tot Azure inhoud

###<a name="minor-corrections"></a>Secundaire correcties

Secundaire correcties of toelichtingen voor documentatie en code voorbeelden in deze cessies‑retrocessies foutenrapporten worden bedekt door het [Azure Website gebruiksvoorwaarden (gebruiksvoorwaarden)](http://azure.microsoft.com/support/legal/website-terms-of-use/).


###<a name="larger-submissions"></a>Grotere ingediende items

Als u een verzoek om te halen met nieuwe of belangrijke wijzigingen in de documentatie en codevoorbeelden indient, sturen we een opmerking in GitHub vraagt u om in te dienen een online bijdrage licentie overeenkomst (CLA) als u zich in een van deze groepen:

* Leden van de groep Microsoft Open-technologieën.
* Inzenders die niet voor Microsoft werken.

We nodig u om het online formulier voordat we uw aanvraag halen kunt accepteren.

Volledige gegevens zijn beschikbaar op [http://azure.github.io/guidelines/#cla](http://azure.github.io/guidelines/#cla).

## <a name="repository-organization"></a>Opslagplaats organisatie

De inhoud in de bibliotheek azure-inhoud volgt de organisatie van documentatie op [Azure.Microsoft.com](http://azure.microsoft.com). Deze bibliotheek bevat twee hoofdsite mappen:

### <a name="articles"></a>\articles

De map *\articles* bevat de documentatie-artikelen die zijn opgemaakt als korting bestanden met de extensie *.md* .

Artikelen in de hoofdmap zijn gepubliceerd naar Azure.Microsoft.com in het pad *http://azure.microsoft.com/documentation/articles/ {artikel-naam-zonder-md} /*.

* **Bestandsnamen-artikel:** Zie [de richtlijnen naamgeving van onze bestanden](./contributor-guide/file-names-and-locations.md).

Artikelen binnen hun eigen servicemap zijn gepubliceerd naar Azure.Microsoft.com in het pad *http://azure.microsoft.com/documentation/articles/service-folder/ {artikel-naam-zonder-md} /*

* **Media submappen:** De map *\articles* bevat de map *\media* voor hoofdmap directory artikel media-bestanden, in welke submappen met de afbeeldingen voor elk artikel.  De service-mappen bevatten een afzonderlijke media-map voor de artikelen in elke servicemap. De mappen van de afbeelding artikel dezelfde naam hebben als het bestand artikel, min de bestandsextensie *.md* .

### <a name="includes"></a>\Includes

U kunt herbruikbare inhoud secties wilt opnemen in een of meer artikelen maken. Zie [aangepaste uitbreidingen gebruikt in onze technische inhoud](./contributor-guide/custom-markdown-extensions.md).

### <a name="markdown-templates"></a>\markdown-sjablonen

Deze map bevat onze standaard korting-sjabloon met de eenvoudige korting opmaak die u nodig voor een artikel hebt.

### <a name="contributor-guide"></a>\contributor-Guide

Deze map bevat artikelen die deel van de onze medewerkers handleiding uitmaken.  

## <a name="use-github-git-and-this-repository"></a>GitHub, cijfer en deze opslagplaats gebruiken

Zie voor informatie over hoe u bijdragen, hoe u met de UI GitHub bijdragen kleine wijzigingen en hoe u zich splitsen en de opslagplaats voor meer aanzienlijk bijdragen aan goede doelen klonen [installeren en instellen van hulpmiddelen voor het beschikbaar is in GitHub](./contributor-guide/tools-and-setup.md).

Als u GitBash installeren en kies lokaal werken, wordt de stappen voor het maken van een nieuwe lokale werken tak, het aanbrengen van wijzigingen, en het versturen van dat de wijzigingen weer met de belangrijkste tak staan in [cijfer opdrachten voor het maken van een nieuw artikel of een bestaand artikel bijwerken](./contributor-guide/git-commands-for-master.md)

### <a name="branches"></a>Vertakkingen

Het is raadzaam dat u maakt lokale werken vertakkingen die zijn gericht op een bepaald bereik van wijzigen. Elke tak moet worden beperkt tot één concept/artikel zowel naar stroomlijnen werkstroom en de mogelijkheid van conflicten bij het samenvoegen te verkleinen.  De volgende inspanningen zijn van het gewenste bereik voor een nieuwe tak:

* Een nieuw artikel (en gekoppelde afbeeldingen)
* Spelling en grammatica bewerkingen van een artikel.
* Een enkel opmaakwijzigen toepassen via een groot aantal artikelen (bijvoorbeeld nieuwe copyright voettekst).

## <a name="how-to-use-markdown-to-format-your-topic"></a>Hoe u uw onderwerp opmaken met korting

De artikelen in deze bibliotheek gebruik GitHub flavored korting.  Hier volgt een lijst met bronnen.

- [Basisbeginselen van korting](https://help.github.com/articles/markdown-basics/)

- [Afdrukbare korting Cheatsheet voor](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

- Zie de [hulpprogramma's en het onderwerp van de instelling](./contributor-guide/tools-and-setup.md#install-a-markdown-editor)voor onze lijst met korting editors.

## <a name="article-metadata"></a>Metagegevens van artikel

Artikel metagegevens kan bepaalde functies op de website azure.microsoft.com, zoals auteur afschrijving, Inzender afschrijving breadcrumbs, artikel beschrijvingen en SEO optimalisaties toe, evenals rapportage Microsoft gebruikt de prestaties van de inhoud wordt berekend. Zo is, is de metagegevens belangrijke! [Hier volgt de richtlijnen ervoor te zorgen dat de metagegevens van uw rechts klaar is](./contributor-guide/article-metadata.md).

### <a name="labels"></a>Labels

Geautomatiseerde etiketten zijn toegewezen om op te halen verzoeken om help ons te beheren van de werkstroom van de aanvraag halen en om te laten weten wat er gebeurt met uw aanvraag halen:

* Licentieovereenkomst voor bijdrage gerelateerd
    * CLA niet vereist: de wijziging is relatief klein en hoeft u niet dat u zich een CLA aanmeldt.
    * CLA vereist: het bereik van de wijziging is relatief groot en is vereist dat u zich een CLA aanmeldt.
    * CLA ondertekend: de inzender ondertekend de CLA, zodat het verzoek halen nu naar voren voor revisie verplaatsen kunt.
* Labels voor pijler: etiketten, zoals PnP, moderne Apps en TDC te categoriseren met de aanvragen halen door de interne organisatie die vereist zijn voor het controleren van de aanvraag halen.
* Wijzigen die zijn verzonden naar de auteur: de auteur is gesteld van de aanvraag in behandeling halen.

## <a name="more-resources"></a>Meer informatiebronnen

Zie de [index van onze medewerker handleiding](./contributor-guide/contributor-guide-index.md) voor alle onderwerpen in onze richtlijnen.
