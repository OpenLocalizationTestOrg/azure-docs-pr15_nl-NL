<properties 
    pageTitle="Het gebruik van scoren gebruikersprofielen in Azure zoeken | Microsoft Azure | De zoekservice gehoste cloud" 
    description="Afstemmen zoeken via profielen scoren in Azure zoeken, een gehoste cloud-zoekservice op Microsoft Azure classificeren." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="how-to-use-scoring-profiles-in-azure-search"></a>Het gebruik van profielen score in Azure zoeken

Score profielen zijn een functie van Microsoft Azure zoeken die aanpassen van de berekening van de zoekopdracht scores, invloed op hoe items zijn gerangschikt in een lijst met zoekresultaten. U kunt zien scores voor profielen als een manier om te model relevantie te verkrijgen, door te verhogen van items die voldoen aan vooraf gedefinieerde criteria. Stel dat uw toepassing is een online hotel reservering-site. Door te verhogen de `location` veld zoekopdracht met een term zoals Seattle leidt tot hogere scores voor items die Seattle hebben in de `location` veld. Houd er rekening mee dat kan er meer dan één score profiel of geen helemaal, als het standaard scoren voldoende voor uw toepassing.

Om te experimenteren met score profielen, kunt u een voorbeeldtoepassing die gebruikmaakt van score profielen naar de rang volgorde van zoekresultaten wijzigen downloaden. De steekproef is een consoletoepassing – misschien niet erg realistische voor de ontwikkeling echte toepassingen – maar handig toch als een hulpmiddel onderwijs. 

De voorbeeldtoepassing ziet u score gedrag fictieve gegevens ofwel met de `musicstoreindex`. De eenvoudig van de steekproef-app kunt u gemakkelijk wijzigen score profielen en query's en raadpleegt u de onmiddellijke gevolgen voor rang volgorde wanneer het programma wordt uitgevoerd.

<a id="sub-1"></a>
## <a name="prerequisites"></a>Vereisten voor

De toepassing van de steekproef is geschreven in C# Gebruik Visual Studio-2013. Als u een kopie van Visual Studio nog geen hebt, kunt u de gratis [Visual Studio-2013 Express edition](http://www.visualstudio.com/products/visual-studio-express-vs.aspx) proberen.

Moet u een Azure-abonnement en een Azure-zoekservice de zelfstudie voltooien. Zie [een Search-service in de portal maken](search-create-service-portal.md) voor hulp bij het instellen van de service.

[AZURE. OPNEMEN [moet u een Azure-account om te voltooien van deze zelfstudie:](../../includes/free-trial-note.md)]

<a id="sub-2"></a>
## <a name="download-the-sample-application"></a>Downloaden van de steekproef-toepassing

Ga naar [Demo scoren profielen van Azure zoeken](https://azuresearchscoringprofiles.codeplex.com/) op codeplex de voorbeeldtoepassing die worden beschreven in deze zelfstudie te downloaden.

Klik op het tabblad broncode klikt u op **downloaden** om te downloaden van een zip-bestand van de oplossing. 

 ![][12]

<a id="sub-3"></a>
## <a name="edit-appconfig"></a>App.config bewerken

1. Nadat u de bestanden hebt geëxtraheerd, opent u de oplossing in Visual Studio om de configuratiebestand te bewerken.
1. Dubbelklik op **app.config**in Solution Explorer. Dit bestand Hiermee geeft u de service-eindpunt en een `api-key` gebruikt om te verifiëren van het verzoek. U kunt deze waarden in de klassieke Portal aanvragen.
1. Meld u aan bij de [Portal van Azure](https://portal.azure.com).
1. Ga naar het servicedashboard voor Azure zoeken.
1. Klik op de tegel **Eigenschappen** als u wilt de service-URL kopiëren
1. Klik op de tegel **toetsen** om te kopiëren de `api-key`.

Wanneer u hebt toegevoegd de URL en `api-key` naar app.config, toepassingsinstellingen ziet er als volgt:

   ![][11]


<a id="sub-4"></a>
## <a name="explore-the-application"></a>De toepassing verkennen

U bent bijna klaar om te maken en uitvoeren van de app, maar voordat u dit doet, gaat u naar de JSON-bestanden maken en de index te vullen.

**Schema.JSON** Hiermee definieert u de index, inclusief de score profielen die zijn benadrukt in deze demo. Melding dat het schema alle velden die worden gebruikt in de index, waaronder niet-doorzoekbare velden, zoals gedefinieerd `margin`, die u kunt gebruiken in een score profiel. De syntaxis van de profiel scoren wordt beschreven in [toevoegen een score profiel aan een Azure-zoekindex](http://msdn.microsoft.com/library/azure/dn798928.aspx).

**Data1-3.json** vindt u de gegevens, 246 albums over een klein aantal genres. De gegevens is een combinatie van werkelijke album en de artiest informatie, met fictieve velden, zoals `price` en `margin` gebruikt om te illustreren zoekopdrachten. De gegevensbestanden voldoen aan de index en zijn geüpload naar uw Azure Search-service. Nadat de gegevens worden geüpload en geïndexeerde, kunt u query's op deze uitgeven.

**Program.cs** voert de volgende bewerkingen:

- Een consolevenster wordt geopend.

- Maakt verbinding met Azure zoekprogramma's via de URL van de service en `api-key`.

- Hiermee verwijdert u de `musicstoreindex` indien aanwezig.

- Hiermee maakt u een nieuwe `musicstoreindex` met behulp van het bestand schema.json.

- De index met de bestanden worden.

- De index met vier query's in een query. Zoals u ziet dat de score profielen zijn opgegeven als een queryparameter. Alle query's gezocht 'best' naar dezelfde periode. De eerste query ziet u standaard scoren. De resterende drie query's gebruik een score profiel.

<a id="sub-5"></a>
## <a name="build-and-run-the-application"></a>Maken en uitvoeren van de toepassing

Als u wilt weten connectivity of constructie verwijzing problemen, maken en uitvoeren van de toepassing om ervoor te zorgen er zijn geen problemen werken eerst. Hier ziet u een consoletoepassing openen in de achtergrond. Alle vier query's uitvoeren in reeks weergeven zonder te onderbreken. Op vele systemen, wordt het hele programma uitgevoerd binnen 15 seconden. Als de consoletoepassing bevat een bericht 'voltooid. Druk op enter als u wilt doorgaan', het programma is voltooid. 

Als u wilt vergelijken query wordt uitgevoerd, u kunt markeren / kopiëren / plakken de queryresultaten van de console en plak deze in een Excel-bestand. 

De volgende afbeelding ziet u resultaten van de eerste drie query's naast elkaar. Alle query's gebruikt u dezelfde zoekterm, 'best', die wordt weergegeven in veel albumtitels.

   ![][10]

De eerste query gebruikt standaard scoren. Aangezien de zoekterm wordt alleen in albumtitels weergegeven en geen andere criteria is opgegeven, worden artikelen met 'best' in de albumtitel in de volgorde waarin de zoekservice ze vindt geretourneerd. 

De tweede query een score profiel gebruikt, maar u ziet dat het profiel geen effect heeft. De resultaten zijn identiek aan die van de eerste query. Dit is omdat het score profiel verhoogt een veld dat niet aan de query belangrijkste ('nieuwe'). De zoekterm "beste" bestaat niet in elk veld 'nieuwe' van een document. Wanneer een score profiel geen effect bevat, wordt de resultaten zijn hetzelfde als het standaard scoren.  

De derde query is de eerste bewijs scores profiel impact. De zoekterm is nog steeds aanbevolen zodat we met dezelfde reeks albums werkt, maar omdat het score profiel biedt aanvullende criteria die verhoogt 'classificatie' en 'laatst bijgewerkt', sommige items zijn voortbewogen hoger in de lijst.

De volgende afbeelding ziet de vierde en laatste query, verhoogd door 'marge'. Het veld 'marge' niet-doorzoekbaar is en kan niet worden geretourneerd in de lijst met zoekresultaten. De margewaarde '' is handmatig worden toegevoegd aan het werkblad om te illustreren de komma die items met hogere marges hoger in de lijst met zoekresultaten weergegeven. 

   ![][9]

Nu dat u hebt geëxperimenteerd met score profielen, te wijzigen van het programma voor het gebruik van de syntaxis van de andere query, scoren profielen of uitgebreidere gegevens. Koppelingen in het volgende gedeelte vindt u meer informatie.

<a id="next-steps"></a>
## <a name="next-steps"></a>Volgende stappen

Meer informatie over profielen scoren. Zie [toevoegen een score profiel aan een Azure zoekindex](http://msdn.microsoft.com/library/azure/dn798928.aspx) voor meer informatie.

Meer informatie over de syntaxis en het query zoekparameters. Zie [Zoeken-documenten (Azure zoeken REST API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) voor meer informatie.

Moet u een stap terug en meer informatie over indexen maken? [Bekijk deze video](http://channel9.msdn.com/Shows/Cloud+Cover/Cloud-Cover-152-Azure-Search-with-Liam-Cavanagh) voor meer informatie over de basisbeginselen.

<!--Anchors-->
[Prerequisites]: #sub-1
[Download the sample application]: #sub-2
[Edit app.config]: #sub-3
[Explore the application]: #sub-4
[Build and run the application]: #sub-5
[Next steps]: #next-steps

<!--Image references-->
[12]: ./media/search-get-started-scoring-profiles/AzureSearch_CodeplexDownload.PNG
[11]: ./media/search-get-started-scoring-profiles/AzureSearch_Scoring_AppConfig.PNG
[10]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX1.PNG
[9]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX2.PNG 