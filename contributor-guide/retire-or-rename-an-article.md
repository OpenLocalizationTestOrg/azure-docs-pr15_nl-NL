# <a name="steps-to-follow-when-you-retire-or-change-the-name-of-an-acom-technical-article"></a>Volgende stappen uitvoeren wanneer u intrekken of wijzig de naam van een ACOM technische artikel

Deze richtlijnen zijn voor kleine en middelgrote ondernemingen die worden vermeld als auteur van een artikel dat moet worden teruggetrokken in de sectie technische documentatie van azure.microsoft.com. De stappen zijn ook van toepassing als de naam van een bestand is gewijzigd.

Als u een lid van onze Azure community bent en u denkt dat een artikel moet worden teruggetrokken voor welke reden dan ook, laat u een opmerking in de stroom van de opmerking Disqus voor het artikel om te laten weten is iets mis met het artikel auteur.

Kleine of middelgrote onderneming auteurs moeten meerdere stappen uitvoeren om te probleemloos inhoud intrekken, zodat gebruikers van de website geen een ongeldige ervaring als we de inhoud van de site intrekken. Het artikel te verwijderen of wijzigen van de naam moet het laatste wat gebeurt er!

## <a name="step-1-set-the-article-to-no-indexno-follow-and-republish-it-recommended"></a>Stap 1: Het artikel ingesteld op Nee-index/Nee-volgen en opnieuw publiceren (aanbevolen)

Het eerste wat dat u moet doen, is het artikel publiceren als geen-index/Nee-volgen een paar weken voordat u deze daadwerkelijk verwijderen. Hiermee wordt beschouwd als het beste oefenen 'oude werk' voor het intrekken van inhoud. Hierdoor Hiermee verwijdert u het artikel uit search engine indexen zodat mensen het artikel niet in zoekresultaten gevonden. [Zie de interne wiki voor meer informatie.](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx)

## <a name="step-2-manage-inbound-links-required"></a>Stap 2: Binnenkomende koppelingen (vereist) beheren

Bepaal als er niet-Microsoft binnenkomende koppelingen aan uw inhoud zijn. Vaak blogs-forums en andere inhoud op het web doorverwezen naar artikelen. Vaak, u kunt werken met blog eigenaars die u wilt wijzigen van deze koppelingen en u kunt verwijderen of koppelingen uit berichten forum bijwerken. Web analytics-hulpprogramma's kunnen u vertellen als er binnenkomende koppelingen moet u mogelijk beheren op deze manier hoog verkeer zijn.

## <a name="step-3-remove-all-crosslinks-to-the-article-from-the-technical-content-repository-required"></a>Stap 3: Alle kruiskoppelingen naar het artikel verwijderen uit de technische inhoud opslagplaats (vereist)

Niet te vertrouwen op omleidingen naar kruiskoppelingen uit andere artikelen kunt verrichten. Bijwerken of verwijderen de meerdere verwijzingen naar het artikel u de naam wijzigen of verwijderen, inclusief in artikelen eigendom van andere personen.

1. Controleer of u werkt in een bijgewerkt lokale tak – uitvoeren `git pull upstream master` (of de gewenste variatie in deze weer te geven.

2.  Scan de map azure-inhoud-prijs/artikelen en de map azure-inhoud-prijs/bevat naar een of meer artikelen en deze koppeling naar het artikel die u wilt intrekken en een van beide verwijderen de kruiskoppelingen of vervang deze velden door een juiste nieuwe kruiskoppelingen. U kunt een zoekopdracht gebruiken en vervang hulpprogramma voor het vinden van de kruiskoppelingen als u geïnstalleerd hebt. Als u niet hebt, kunt u Windows PowerShell gratis! Hier wordt uitgelegd hoe PowerShell gebruiken om de kruiskoppelingen:

 een. Start Windows PowerShell.

 b. Klik bij de prompt PowerShell wijzigen naar de map azure-inhoud-pr\articles:

 `cd azure-content-pr\articles`

 c. Typ de volgende opdracht waarin wordt een lijst met alle bestanden die bevatten een verwijzing naar het artikel die u wilt verwijderen:

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  Als u liever de lijst met namen van bestanden verzenden naar een tekstbestand (in dit geval, benoemde psoutput.txt), kunt u het volgende doen:

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Toevoegen en alle wijzigingen doorvoeren, push ze aan uw zich splitsen en een aanvraag halen verplaatsen van de wijzigingen van uw zich splitsen naar de basispagina tak van de belangrijkste opslagplaats maken.

## <a name="step-4-update-the-fwlink-tool-required"></a>Stap 4: Het FWLink hulpmiddel (vereist) bijwerken

Het hulpmiddel FWLink controleren op eventuele FWLinks die naar het artikel verwijzen kunnen. Wijs een FWLinks zelf vervangende inhoud; Als u niet over de alias die eigenaar is van de koppeling, neem deel aan deze. Als de eigenaren niet om de koppeling bijgewerkt, het bestand een tickets met MSCOM wilt dat de koppeling gewijzigd. Meer informatie - [interne wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-5-remove-all-crosslinks-to-the-article-from-other-pages-on-azuremicrosoftcom-and-create-a-redirect-for-the-retired-page-if-appropriate-required"></a>Stap 5: Alle kruiskoppelingen naar het artikel verwijderen uit andere pagina's op azure.microsoft.com en maken van een omleiding voor de pagina teruggetrokken als juiste (vereist)

U moet werken met de persoon die u onderhoudt en updates van de openingspagina van documentatie voor uw service voor dit onderdeel. Neem contact op met uw partner inhoud team als u niet weet wie die persoon is. De persoon die u onderhoudt en updates van de openingspagina van het document moet twee dingen doen:

1. Scannen in Visual Studio, de **hele** ACOM web oplossing voor kruisverwijzingen naar het bestand wilt intrekken. De kruisverwijzingen verwijderen of vervang deze velden door een bijgewerkte kruisverwijzing. U moet de HTML-koppelingen, evenals de gerelateerde resource tekenreeksen voor de HTML-koppelingen verwijderen. Meer informatie - raadpleegt u de [interne wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Create%20or%20edit%20a%20service%20landing%20page%20or%20left%20nav.aspx)

2. Als u een vervangende-artikel bestaat, maakt u een omleiding. Meer informatie - raadpleegt u de [interne wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx).

3. Controleer de wijzigingen in de bibliotheek.

## <a name="step-6-retire-the-article"></a>Stap 6: Het artikel intrekken

Nadat u de voorgaande stappen hebt uitgevoerd en deze wijzigingen live zijn, kunt u het artikel verwijderen uit de bibliotheek. 

**Belangrijke:** Wanneer u bestanden hebt verwijderd, moet u de `git add --all` opdracht.

## <a name="step-7-remove-links-from-msdn-required"></a>Stap 7: Koppelingen verwijderen uit MSDN (vereist)

Controleer de inhoud q & a-hulpprogramma voor verbroken koppelingen naar het onderwerp ingetrokken of gewijzigde en verwijderen/fix de koppelingen in alle MSDN-onderwerpen beïnvloed.

## <a name="step-8-remove-cached-pages-from-search-engines-only-if-absolutely-necessary"></a>Stap 8: In de cache opgeslagen pagina's verwijderen uit zoekprogramma's (alleen als absoluut nodig)

In dit alleen doen als de inhoud moet snel vanwege een juridische of slechte klantproblemen worden verwijderd. Per aanbevelingen van Google, moeten normale prioriteit pagina verwijderingen alleen worden verwerkt door natuurlijke search engine processen. Ga naar deze webpagina's in de cache opgeslagen webpagina's van zoekprogramma's verwijderen:

[Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Inzenders de handleiding voor koppelingen

- [Van overzichtsartikel](./../README.md)
- [Index van artikelen](./contributor-guide-index.md)
