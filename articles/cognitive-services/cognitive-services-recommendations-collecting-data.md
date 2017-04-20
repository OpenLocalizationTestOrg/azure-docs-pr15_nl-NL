<properties
    pageTitle="Verzamelt gegevens om te trainen uw Model: Machine Learning aanbevelingen API | Microsoft Azure"
    description="Azure Machine Learning aanbevelingen - verzamelt gegevens om te trainen uw Model"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="luisca"/>

#  <a name="collecting-data-to-train-your-model"></a>Verzamelen van gegevens naar uw Model trainen #

De API aanbevelingen hoort van uw oude transacties om te zoeken welke items moeten worden aanbevolen voor een bepaalde gebruiker.

Nadat u een model hebt gemaakt, moet u twee stukje informatie opgeven voordat u eventuele training doen kunt: een catalogusbestand en gegevens over zoekgebruik.

>   Als u nog niet gedaan hebt, raden we u aan het voltooien van de [handleiding snel aan de slag](cognitive-services-recommendations-quick-start.md).


## <a name="catalog-data"></a>Gegevens in de catalogus ##

### <a name="catalog-file-format"></a>Indeling voor catalogusitem-bestand ###

De catalogusbestand bevat informatie over de items die u naar de klant zijn aanbod.
De gegevens in de catalogus Voer de volgende indeling:

- Zonder functies:`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- Met functies-`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

#### <a name="sample-rows-in-a-catalog-file"></a>Voorbeeld rijen in een catalogusbestand

Zonder functies:

    AAA04294,Office Language Pack Online DwnLd,Office
    AAA04303,Minecraft Download Game,Games
    C9F00168,Kiruna Flip Cover,Accessories

Met functies:

    AAA04294,Office Language Pack Online DwnLd,Office,, softwaretype=productivity, compatibility=Windows
    BAB04303,Minecraft DwnLd,Games, softwaretype=gaming,, compatibility=iOS, agegroup=all
    C9F00168,Kiruna Flip Cover,Accessories, compatibility=lumia,, hardwaretype=mobile

#### <a name="format-details"></a>Opmaak details

| Naam | Verplicht | Type |  Beschrijving |
|:---|:---|:---|:---|
| Lijstitem-Id |Ja | [A-z], [a-z], [0-9], [_] & #40; Onderstrepingsteken & #41; [-] & #40; streepje & #41;<br> Maximale lengte: 50 | Unieke id van een item. |
| Itemnaam | Ja | Alle alfanumerieke tekens<br> Maximale lengte: 255 | Itemnaam. |
| Item categorie | Ja | Alle alfanumerieke tekens <br> Maximale lengte: 255 | Categorie waarbij deze hoort (bijvoorbeeld koken boeken, Drama...); kan niet leeg zijn. |
| Beschrijving | Nee, tenzij functies zijn presenteren (maar kan leeg zijn) | Alle alfanumerieke tekens <br> Maximale lengte: 4000 | Beschrijving van dit item. |
| Lijst met functies | Nee | Alle alfanumerieke tekens <br> Maximale lengte: 4000; Maximumaantal functies: 20 | Door komma's gescheiden lijst met de functienaam van de = functie waarde die kan worden gebruikt voor het verbeteren van de aanbeveling model.|

#### <a name="uploading-a-catalog-file"></a>Een catalogusbestand uploaden

Bekijk de [API-naslaggids](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e1) voor het uploaden van een catalogusbestand.  

Houd er rekening mee dat de inhoud van het catalogusbestand moet worden doorgegeven als de hoofdtekst van het verzoek.

Als u verschillende catalogusbestanden naar hetzelfde model met verschillende oproepen uploaden, wordt we alleen de nieuwe catalogusitems ingevoegd. Bestaande items blijft met de oorspronkelijke waarden. U kunt gegevens in de catalogus niet bijwerken met behulp van deze methode.

>   Opmerking: De maximale bestandsgrootte is 200MB.
>   Het maximum aantal items in de catalogus ondersteund is 100.000 items.


## <a name="why-add-features-to-the-catalog"></a>Waarom functies toevoegen aan de catalogus?

De engine aanbevelingen Hiermee maakt u een statistische model dat u welke items ziet moeten worden leuk vindt of gekocht door een klant waarschijnlijk zijn. Als er is een nieuw product dat heeft nooit is gecommuniceerd met deze niet mogelijk is om u te maken van een model op collega exemplaren alleen. Stel dat u start een nieuwe aanbod "kinderen thuis violin' in de winkel, aangezien u die violin nooit verkocht hebt voordat u niet kunt zien welke andere items met die violin aanbevelen.

Die gezegd, als de engine bekend is informatie over die violin (dat wil zeggen een musical instrument is, is het kinderen leeftijden 7-10, is niet een dure violin, enz.), en vervolgens de engine kunt Leer van andere producten met vergelijkbare functies. Bijvoorbeeld, u hebt de violin verkocht in het verleden en meestal personen die violen kopen meestal kopen klassieke muziek-cd's en blad muziek staat.  Het systeem kunt zoeken naar deze verbindingen tussen de functies en aanbevelingen op basis van de functies terwijl uw nieuwe violin weinig gebruik heeft opgeven.

Functies als onderdeel van de catalogusgegevens zijn geïmporteerd, en klik vervolgens hun rang (of het belang van de functie in het model) is gekoppeld wanneer een rang opbouwen klaar is. Functie rang kunt wijzigen op basis van het patroon van gegevens over het gebruik en het type items. Maar voor consistent gebruik/items, de rangorde van alleen kleine schommelingen moet hebben. De rang van functies is een negatief getal. Het getal 0 betekent dat de functie niet is rangschikking (gebeurt als u deze API vóór de voltooiing van de eerste rang opbouwen aanroepen). De datum waarop de rangorde van is toegekend, wordt de fris score genoemd.


###<a name="features-are-categorical"></a>Functies zijn Categorical

Dit betekent dat u functies die lijken op een categorie moet maken. Bijvoorbeeld prijs = 9.34 is niet een bestaan functie. Aan de andere kant, een functie priceRange zoals = Under5Dollars is een functie bestaan. Een andere algemene fout is de naam van het item als een functie te gebruiken. Hierdoor zou de naam van een item dat moet uniek zijn, zodat deze niet een categorie wilt beschrijven. Zorg ervoor dat de functies vertegenwoordigen categorieën van items.


###<a name="how-manywhich-features-should-i-use"></a>Hoe veel/die functies moet ik gebruiken?


Uiteindelijk willen maken op de aanbevelingen ondersteunt het bouwen van een model met maximaal 20 functies. U kunt meer dan 20 functies toewijzen aan de items in de catalogus, maar u moet doen? een classificatie opbouwen en kiest u alleen de functies met een hoge rangschikking. (Een functie met een rang van 2.0 of meer is een goed functie gebruik!). 


###<a name="when-are-features-actually-used"></a>Wanneer worden functies daadwerkelijk gebruikt?

Functies worden gebruikt door het model als er niet voldoende transactiegegevens op te geven aanbevelingen op transactie informatie alleen. Dus heeft functies de grootste gevolgen voor "koudwatersystemen items" – artikelen met minder transacties. Als u al uw objecten voldoende transactie-informatie hebben moet u mogelijk niet Verrijk uw model met functies.


###<a name="using-product-features"></a>Gebruik van productfuncties

Functies als onderdeel van uw build die u wilt gebruiken:

1. Controleer of dat de catalogus heeft functies wanneer u deze uploaden.

2. Een classificatie opbouwen activeren. Hiermee wordt de analyse op de urgentie/rang van de functies uitvoeren.

3. Een opbouwen aanbevelingen activeren, het instellen van de volgende parameters maken: useFeaturesInModel ingesteld op waar, allowColdItemPlacement op waar, en modelingFeatureList moet worden ingesteld op de door komma's gescheiden lijst met functies die u gebruiken wilt voor het verbeteren van uw model. Zie [aanbevelingen typeparameters maken](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d0) voor meer informatie.





## <a name="usage-data"></a>Gebruiksgegevens ##
Een bestand gebruik bevat informatie over hoe deze items worden gebruikt, of de transacties van uw bedrijf.

#### <a name="usage-format-details"></a>Gebruik opmaak details
Een bestand gebruik is een CSV-(door komma's gescheiden waarde)-bestand waarbij elke rij in een bestand gebruik de interactie tussen een gebruiker en een item vertegenwoordigt. Elke rij opgemaakt als volgt:<br>
`<User Id>,<Item Id>,<Time>,[<Event>]`



| Naam  | Verplicht | Type | Beschrijving
|-------|------------|------|---------------
|Gebruikers-Id|         Ja|[A-z], [a-z], [0-9], [_] & #40; Onderstrepingsteken & #41; [-] & #40; streepje & #41;<br> Maximale lengte: 255 |Unieke id van een gebruiker.
|Lijstitem-Id|Ja|[A-z], [a-z], [0-9], [& #95;] & #40; Onderstrepingsteken & #41; [-] & #40; streepje & #41;<br> Maximale lengte: 50|Unieke id van een item.
|Tijd|Ja|Datum in de notatie: jjjj/MM/ddTUU (bijvoorbeeld 2013/06/20T10:00:00)|De tijd van gegevens.
|Gebeurtenis|Nee | Een van de volgende handelingen uit:<br>• Klik<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Aanschaffen| Het type transactie. |

#### <a name="sample-rows-in-a-usage-file"></a>Voorbeeld rijen in een bestand gebruik

    00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase
    0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase
    0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase
    00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase
    0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase
    00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase

#### <a name="uploading-a-usage-file"></a>Uploaden van een bestand gebruik

Bekijk de [API-naslaggids](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e2) voor het uploaden van bestanden voor gebruik.
Houd er rekening mee dat u moet de inhoud van het bestand gebruik als de hoofdtekst van het gesprek HTTP doorgeven.

>  Opmerking:

>  Maximale bestandsgrootte: 200MB. U kunt verschillende gebruik bestanden uploaden.

>  U moet een catalogusbestand uploaden, voordat u begint met het van gebruiksgegevens toevoegen aan uw model. Alleen de items in de catalogusbestand wordt gebruikt tijdens de fase training. Alle andere items worden genegeerd.

## <a name="how-much-data-do-you-need"></a>Hoeveel gegevens hebt u nodig?

De kwaliteit van het model is sterk afhankelijk van de kwaliteit en de hoeveelheid van uw gegevens.
Het systeem leert wanneer gebruikers kopen voor verschillende items (verwijst naar deze collega exemplaren). FBT-versies is het ook belangrijk te weten welke items worden gekocht in de dezelfde transacties. 

Een goede vuistregel is dat de meeste items zich op 20 transacties of meer, dus als u had 10.000 items in de catalogus, zou te raden u ten minste 20 maal het nummer van transacties of ongeveer 200.000 transacties. Nogmaals, is dit een vuistregel. U wilt experimenteren met uw gegevens.

Als u een model hebt gemaakt, kunt u een [offline evaluatie](cognitive-services-recommendations-buildtypes.md) om te controleren hoe goed uw model is waarschijnlijk om uit te voeren kunt uitvoeren.
