<properties 
    pageTitle="Machine Learning aanbevelingen: JavaScript-integratie | Microsoft Azure" 
    description="Azure Machine Learning aanbevelingen - integratie JavaScript - documentatie" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="javascript" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Azure Machine Learning-aanbevelingen - JavaScript-integratie

>[AZURE.NOTE]U moet beginnen met de Service van aanbevelingen-API-cognitieve in plaats van deze versie. De aanbevelingen cognitieve Service wordt deze service worden vervangen, en de nieuwe functies worden er worden ontwikkeld. Er worden nieuwe mogelijkheden, zoals batchen ondersteuning, een betere API Explorer, een duidelijkere API oppervlak, consistenter registratie/facturering ervaring, enzovoort.
> Meer informatie over het [migreren naar de nieuwe cognitieve Service](http://aka.ms/recomigrate)


In dit document weer het integreren van uw site via JavaScript. De JavaScript kunt u gegevens oplevert gebeurtenissen verzenden en aanbevelingen gebruiken als u een model aanbeveling maakt. Alle bewerkingen uitgevoerd via JS kunnen ook worden gedaan van serverkant.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="1-general-overview"></a>1. algemene overzicht
Uw site integreren met Azure ML aanbevelingen bestaan op 2 fasen:

1.  Gebeurtenissen in Azure ML aanbevelingen verzenden. Hiermee schakelt u het maken van een model aanbeveling.
2.  Gebruik de aanbevelingen. Nadat het model is gebouwd, kunt u de aanbevelingen gebruiken. (Dit document niet wordt uitgelegd hoe u een model, maakt het aan de slag met voor meer informatie over het lezen).


<ins>Fase I</ins>

In de eerste fase invoegen u in uw html-pagina's een kleine JavaScript-bibliotheek waarmee de pagina gebeurtenissen verzenden terwijl ze in de HTML-pagina naar Azure ML aanbevelingen servers (via Datamarket voorkomen):

![Tekening1][1]

<ins>Fase II</ins>

In de tweede fase wanneer u wilt weergeven van de aanbevelingen op de pagina Selecteer u een van de volgende opties:

1. uw server (op de fase van de paginaweergave) oproepen Azure ML aanbevelingen Server (via Datamarket) om aanbevelingen weer. De resultaten bevatten een lijst met items-id. Uw server moet verrijken de resultaten met de items metagegevens (bijvoorbeeld afbeeldingen, beschrijving) en de gemaakte pagina verzenden naar de browser.

![Drawing2][2]

2. de andere optie is het kleine JavaScript-bestand uit fase één met een eenvoudige lijst met aanbevolen items opgehaald. De gegevens die zijn ontvangen hier is minder dan de tabel in de eerste optie complex.

![Drawing3][3]

##<a name="2-prerequisites"></a>2. vereisten voor

1. Maak een nieuw model met de API's. Zie de handleiding snel aan de slag op de realisatie ervan.
2. Coderen uw &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; met base64. (Dit wordt worden gebruikt voor de basisverificatie om in te schakelen van de code JS om te bellen van de API's).


##<a name="3-send-data-acquisition-events-using-javascript"></a>3. verzenden gegevensverzameling gebeurtenissen met JavaScript
De volgende stappen vergemakkelijken verzendende gebeurtenissen:

1.  JQuery-bibliotheek in de code opnemen. U kunt het downloaden van nuget in de volgende URL.

        http://www.nuget.org/packages/jQuery/1.8.2
2.  Opnemen van de bibliotheek JavaScript aanbevelingen van de volgende URL: http://aka.ms/RecoJSLib1

3.  Geïnitialiseerd Azure ML aanbevelingen bibliotheek met de juiste parameters.

        <script>
            AzureMLRecommendationsStart("<base64encoding of username:key>",
            "<model_id>");
        </script>

4.  Stuur de gewenste gebeurtenis. Zie gedetailleerde sectie onder op alle typen gebeurtenissen (voorbeeld van Klik op gebeurtenis)

        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") {      
                        AzureMLRecommendationsEvent = [];
                    }
            AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" });
        </script>


###<a name="31-limitations-and-browser-support"></a>3.1. Beperkingen en browserondersteuning
Dit is een verwijzing-implementatie en deze wordt uitgedrukt als is. Er moet ondersteuning voor alle belangrijke browsers.

###<a name="32-type-of-events"></a>3,2. Type gebeurtenissen
Er zijn 5 soorten gebeurtenis die ondersteuning biedt voor de bibliotheek: klik op, aanbeveling op hebt geklikt, toevoegen aan via een kopieerbedrijf winkelwagentje verwijderen uit via een kopieerbedrijf winkelwagen en aanschaffen. Er is een extra gebeurtenis die wordt gebruikt voor het instellen van de gebruikerscontext Login genoemd.

####<a name="321-click-event"></a>3.2.1. Klik op gebeurtenis
Deze gebeurtenis moet worden gebruikt als elk gewenst moment een gebruiker op een item hebt geklikt. Meestal wanneer de gebruiker op een item wordt een nieuwe pagina geopend met de details van het item; Deze gebeurtenis moet worden geactiveerd op deze pagina.

Parameters:
- gebeurtenis (tekenreeks, verplicht) - 'Klik hier'
- item (tekenreeks, verplicht) - unieke id van het item
- Itemnaam (tekenreeks, optioneel) - de naam van het item
- itemDescription (tekenreeks, optioneel) - de beschrijving van het item
- itemCategory (tekenreeks, optioneel) - de categorie van het item
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

Of met optionele gegevens:

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


####<a name="322-recommendation-click-event"></a>3.2.2. Aanbeveling Klik op gebeurtenis
Deze gebeurtenis moet worden gebruikt als elk gewenst moment een gebruiker op een item dat is ontvangen van Azure ML aanbevelingen als een aanbevolen item hebt geklikt. Meestal wanneer de gebruiker op een item wordt een nieuwe pagina geopend met de details van het item; Deze gebeurtenis moet worden geactiveerd op deze pagina.

Parameters:
- gebeurtenis (tekenreeks, verplicht) - "recommendationclick"
- item (tekenreeks, verplicht) - unieke id van het item
- Itemnaam (tekenreeks, optioneel) - de naam van het item
- itemDescription (tekenreeks, optioneel) - de beschrijving van het item
- itemCategory (tekenreeks, optioneel) - de categorie van het item
- seeding (tekenreeksmatrix, optioneel) - de zaden die de query aanbeveling gegenereerd.
- recoList (tekenreeksmatrix, optioneel) - het resultaat van de aanvraag aanbeveling die het item dat is geklikt gegenereerd.
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

Of met optionele gegevens:

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]               });
        </script>


####<a name="323-add-shopping-cart-event"></a>3.2.3. Toevoegen winkelwagentje winkelwagen gebeurtenis
Deze gebeurtenis moet worden gebruikt wanneer de gebruiker een item toevoegt aan het winkelwagentje.
Parameters:
* gebeurtenis (tekenreeks, verplicht) - "addshopcart"
* item (tekenreeks, verplicht) - unieke id van het item
* Itemnaam (tekenreeks, optioneel) - de naam van het item
* itemDescription (tekenreeks, optioneel) - de beschrijving van het item
* itemCategory (tekenreeks, optioneel) - de categorie van het item
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

####<a name="324-remove-shopping-cart-event"></a>3.2.4. Winkelen voor winkelwagen gebeurtenis verwijderen
Deze gebeurtenis moet worden gebruikt wanneer de gebruiker een item aan de winkelwagen verwijdert.

Parameters:
* gebeurtenis (tekenreeks, verplicht) - "removeshopcart"
* item (tekenreeks, verplicht) - unieke id van het item
* Itemnaam (tekenreeks, optioneel) - de naam van het item
* itemDescription (tekenreeks, optioneel) - de beschrijving van het item
* itemCategory (tekenreeks, optioneel) - de categorie van het item
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

####<a name="325-purchase-event"></a>3.2.5. Aankoop-gebeurtenis
Deze gebeurtenis moet worden gebruikt als de gebruiker zijn winkelwagen gekocht.

Parameters:
* gebeurtenis (tekenreeks) - 'aanschaffen'
* items (gekocht []) - matrix een vermelding vasthouden voor elk item hebt gekocht.<br><br>
Gekochte opmaken:
    * item (tekenreeks) - unieke id van het item.
    * Tel (integer of tekenreeks) - aantal items die zijn gekocht.
    * prijs (toegestane achterstand of tekenreeks) - optioneel veld: de prijs van het item.

In het onderstaande voorbeeld ziet aankoop van 3 items (33, 34, 35), twee met alle velden zijn ingevuld (een item, aantal, prijs) en een (item 34) zonder een prijs in.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

####<a name="326-user-login-event"></a>3.2.6. Gebruiker Login gebeurtenis
Azure ML aanbevelingen gebeurtenis bibliotheek wordt gemaakt en gebruiken van een cookie om te kunnen identificeren gebeurtenissen die afkomstig zijn dezelfde browser. Om te kunnen de resultaten van de model die Azure ML aanbevelingen voor het instellen van een unieke gebruikers-id die wordt overschreven door de cookie-gebruik kunt verbeteren.

Deze gebeurtenis moet worden gebruikt na de gebruikersaanmelding aan uw site.

Parameters:
* gebeurtenis (tekenreeks) - "userlogin"
* gebruiker (tekenreeks) - unieke identificatie van de gebruiker.
        <script>
  Als (typeof AzureMLRecommendationsEvent == "ongedefinieerd") {AzureMLRecommendationsEvent = [];} AzureMLRecommendationsEvent.push ({gebeurtenis: 'userlogin', gebruiker: "ABCD10AA"});      </script>

##<a name="4-consume-recommendations-via-javascript"></a>4. aanbevelingen via JavaScript gebruiken
De code die de aanbeveling in beslag wordt geactiveerd door sommige JavaScript-gebeurtenis door de webpagina van de klant. Het antwoord aanbeveling bevat de aanbevolen items-id's, hun namen en hun classificaties. Gebruik deze optie alleen voor een lijst weergeven van de aanbevolen items het beste - complexere afhandeling (zoals het toevoegen van het artikel metagegevens) moet worden uitgevoerd op de server kant-integratie.

###<a name="41-consume-recommendations"></a>4.1 aanbevelingen gebruiken
In beslag neemt zijn aanbevelingen u moet de vereiste JavaScript-bibliotheken op uw pagina en AzureMLRecommendationsStart bellen. Zie de sectie 2.

Aanbevelingen voor een of meer items die u moet bellen van een methode genoemd in beslag neemt: AzureMLRecommendationsGetI2IRecommendation.

Parameters:
* (matrix van reeksen) - een of meer items om aanbevelingen voor items. Als u een opbouwen Fbt gebruiken kunt vervolgens u instellen hier slechts één item.
* numberOfResults (int) - aantal vereiste resultaten.
* includeMetadata (Booleaanse waarde, optioneel) - als ingesteld op 'waar' geeft aan dat het veld metagegevens in het resultaat moet worden ingevuld.
* Verwerking van de functie, een functie waarmee de aanbevelingen worden verwerkt als resultaat gegeven. De gegevens wordt weergegeven als een matrix van:
    * Artikel - item unieke id
    * name - item naam (als bestaat niet in de catalogus)
    * beoordeling - aanbeveling classificatie
    * metagegevens - een tekenreeks met de metagegevens van het item

Voorbeeld: De volgende code aanvraagt 8 aanbevelingen voor item "64f6eb0d-947a-4c18-a16c-888da9e228ba" (en door te geven niet includeMetadata - impliciet staat dat geen metagegevens moet ondernomen worden), deze daarna de resultaten samengevoegd in een buffer.

        <script>
            var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                var buff = "";
                for (var ii = 0; ii < reco.length; ii++) {
                    buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                }
                alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
 
