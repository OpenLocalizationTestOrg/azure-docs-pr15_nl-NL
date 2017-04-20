# <a name="quality-criteria-for-pull-request-review"></a>Kwaliteitscriteria voor halen aanvraag controleren

Deze criteria zijn bedoeld voor auteurs die maken en onderhouden van technische artikelen en voor halen verzoek reviewers die inhoud halen serviceaanvragen bekijken. Als uw aanvraag halen komt niet in aanmerking voor het [automatisch samenvoegen](contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically), wordt deze door een menselijke halen verzoek revisor worden gecontroleerd. Aanvraag revisoren bekijken over het algemeen alleen wat is nieuwe of gewijzigde halen. Halen verzoek revisoren evalueren de wijzigingen in een verzoek om te halen op basis van de blokkeren en niet-blokkeren van kwaliteit objecten bekijken die in dit artikel.

## <a name="blocking-content-quality-items"></a>Blokkeren van inhoud kwaliteit items

De updates in het verzoek halen moeten voldoen aan de volgende criteria voldoet moeten worden samengevoegd. Halen verzoek revisoren feedback in halen verzoek opmerkingen voor deze items en type `#hold-off` in het verzoek halen om terug te keren deze aan u (de auteur) met feedback.

| Categorie | Kwaliteit controleren item |
|----------|---------------------|
|Vereisten voor| De "bij de hand-naar-samenvoeging' en de etiketten"validatie is mislukt"zijn toegewezen aan de PR.|
|Vereisten voor| Het verzoek halen kan niet worden geblokkeerd door een conflict samenvoegen.|
|Cessies‑retrocessies integriteit|    Halen aanvraag bevat geen duidelijke inhoud behalen.|
|Cessies‑retrocessies integriteit|    Halen aanvraag is niet inbegrepen een ingesloten cessies‑retrocessies of ongebruikelijke, overbodige bestanden.|
|Cessies‑retrocessies integriteit |Halen aanvraag bevat minder dan 100 gewijzigde bestanden, tenzij de prijs met opzet een tak release van de basispagina wordt bijgewerkt. (Eigenlijk een prijs ver minder dan die moet bevatten, maar na 100 gewijzigde bestanden, GitHub de verschilbestanden niet wordt weergegeven).|
|Naam geven |Bestandsnamen voor nieuwe bestanden volgt de [richtlijnen voor naamgeving van bestanden](file-names-and-locations.md).|
|Naam geven |Nieuwe mappen die is ingevoerd in het cessies‑retrocessies volgen de [map naming richtlijnen](file-names-and-locations.md#folder-names-in-the-repo).|
|Inhoud    |Het artikel is een technisch document, en dus in de juiste inhoud kanaal. Zie de [Wat waar thuishoren richtlijnen](content-channel-guidance.md).|
|Inhoud    |Het onderwerp in het technische document is geschikt voor een technische artikel. Zie de [Wat waar thuishoren richtlijnen](content-channel-guidance.md).|
|Inhoud    |Het artikel bevat een inleidende alinea en de hoofdtekst van een procedurele of conceptuele van inhoud. Het artikel moet voldoende, voltooid inhoud staan op een eigen als een artikel bevatten. Deze mag geen een kleine fragment van gegevens. (Uitzondering: een onderwerp "Limieten" is in de context van een grote artikel waarin de grenzen van een service alleoffice.)|
|Inhoud| Elementen die genummerde lijsten worden moeten worden genummerd, elementen die moeten worden niet-geordend lijsten met opsommingstekens zijn. Dit is eenvoudige bruikbaarheid.|
|Inhoud| Ongebruikelijke of nieuwe afbeeldingen, informatiearchitectuur of structuren of duidelijk niet-standaard ontwerpen moeten worden goedgekeurd met de potentiële klant prijs revisor. Teams die zijn experimenteren met nieuwe dingen moeten zijn van een probleem/oplossing tekenpapier of een abonnement op hun plaats staan voor het evalueren van experimenten.|
|Siteontwerp functionaliteit| Switchers worden alleen voor het overschakelen voor de verschillende versies van dit artikel gebruikt.|
|Siteontwerp functionaliteit| De titels van switchered artikelen bevatten informatie die elk artikel van de andere artikelen in de set switchered onderscheidt.|
|Siteontwerp functionaliteit| Handmatig geschreven zijn inhoudsopgaven niet toegestaan. Het artikel moet zijn afhankelijk van H2s voor de inhoudsopgave op de pagina.|
|Siteontwerp functionaliteit| Als H2 koppen aanwezig zijn, bevat het artikel ten minste twee H2 koppen. Een H2 kop gebruikt, wordt een artikel met één item inhoudsopgave gemaakt. H2 koppen moeten worden gebruikt voor H3 koppen om ervoor te zorgen dat er wordt een inhoudsopgave gemaakt.|
|Korting| HTML: Broninhoud bevat geen HTML op het niveau van de blok – secundaire inline HTML is toegestaan – zoals superscript, subscript, speciale tekens en andere secundaire dingen die u niet kunt met korting doen. HTML-tabellen zijn toegestaan alleen als de tabel bevat een lijst met opsommingstekens of genummerde lijsten, maar die duidt gewoonlijk die de inhoud moet worden vereenvoudigd zodat de bron kan worden gecodeerd in korting.|
|Korting   |Aangepaste korting elementen worden gebruikt wanneer van toepassing. Ex: Notities worden gecodeerd gebruik van de AZURE. Houd er rekening mee extensie, niet als tekst zonder opmaak.|
|SEO    |Het "& #124; Site-id van Microsoft Azure"is vereist|
|SEO    |De titel van de H1 bevat voldoende gegevens om de inhoud van het artikel, het kunt onderscheiden van andere Azure artikelen en moet worden toegewezen waarschijnlijk klant trefwoorden beschrijven. 'Overzicht' als titel H1 is bijvoorbeeld een fail.|
|Terminologie| Het gebruik van de ARM acroniem, V1 of V2 als verwijzingen naar het klassieke en resourcemanager implementatiemodellen is een blokkeren terminologie-item.|
|Afbeeldingen |GIF-bestanden worden niet in de cessies‑retrocessies geaccepteerd.|
|Afbeeldingen | Afbeeldingen wissen resolutie hebt, zijn gratis van verkeerd gespelde woorden en er worden geen persoonlijke gegevens bevatten | 
|Tijdelijke|De voorbeeldversie artikel moet wissen.Control op tijdelijke. Deze kan niet een duidelijke opmaakfouten bevatten: <br><br>-Een met opsommingstekens of genummerde lijst die wordt weergegeven als een alinea <br> -Code in een blok met code gedeeltelijk in het codeblok en gedeeltelijk buiten deze weergegeven <br>-Genummerde stappen onjuist nummer heeft vanwege defect inspringing|

## <a name="non-blocking-content-quality-items"></a>Blokkeren van niet inhoud van kwaliteit items

Voor deze items bieden halen verzoek revisoren van feedback en instructies voor de auteur dat moet worden opgevolgd met correcties in een verzoek voor een hoger halen. Deze feedback worden echter niet geblokkeerd voor het besluit om samen te voegen. Auteurs moeten opvolgen binnen 3 werkdagen in correcties.

| Categorie | Kwaliteit controleren item |
|----------|---------------------|
|Inhoud|Artikelen moeten een 'Vervolgstappen"hebben aan het einde met 1-3 relevante en aantrekkelijke volgende stappen. Korte tekst moet worden opgenomen die helpt bij de klant begrijpen waarom de volgende stappen relevant zijn. (Nieuwe artikelen alleen). Voorbeeld: <https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-install/><br>![](media/contributor-guide-pr-criteria/nextstepsexample.PNG)|
|Inhoud|Spelling, grammatica en andere problemen schrijven - halen verzoek revisoren mogelijk feedback geven over een paar kleine problemen feedback als niet-blokkeren. Als er meer dan een paar redactionele problemen, revisoren Meld u aan een verzoek bewerken voor het artikel voor een post publicatie bewerken.|
|Afbeeldingen|Afbeeldingen gebruiken de juiste bijschriftstijl en kleur en schermafbeeldingen gebruiken de juiste stijl voor de rand en de tijdelijke aanduiding. [Zie de richtlijnen van de afbeelding](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Afbeeldingen|Afbeeldingen bevatten alternatieve tekst. [Zie de richtlijnen van de afbeelding](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Siteontwerp functionaliteit|De koppen H2 wanneer weergegeven in de inhoudsopgave wordt weergegeven op de pagina, moeten in het ideale geval laten teruglopen in niet meer dan 2 regels. Langer koppen moeilijker worden de TOC van het artikel om te scannen.|
|Stijl conventies|Alle titels en koppen zijn in een zin, per Azure stijl.|
|Proces|Als het verzoek halen kan eenvoudig opnieuw geconfigureerd om te profiteren van PRmerger automatisering, geven halen verzoek revisoren feedback de auteur over het gebruik van vertakkingen zodat de wijzigingen automatisch kunnen worden samengevoegd. Zie [het artikel met prijs mailetiquette](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically).|
|Proces|Wanneer u verwijderen of de naam van een artikel wijzigen, zorg er dan voor dat u de stappen. Aanvraag revisoren moeten de volgende opmerking en een koppeling toevoegen in een opmerking halen:<br><br>*Controleer of u het proces in de inzenders de handleiding voor het verwijderen van artikelen gevolgd: <https://github.com/Azure/azure-content/blob/master/contributor-guide/retire-or-rename-an-article.md> .*|

## <a name="related"></a>Gerelateerd

- [Halen verzoek mailetiquette en aanbevolen procedures voor het Microsoft-inzenders](contributor-guide-pull-request-etiquette.md)

- [Aanvraag Opmerking automatisering halen](contributor-guide-pull-request-comments.md)
