<properties
    pageTitle="Gebruik van de steekproef gegevensverzamelingen in Machine Learning Studio | Microsoft Azure"
    description="Beschrijvingen van de gegevens die wordt gebruikt in de steekproef-modellen die zijn opgenomen in ML Studio. U kunt deze steekproeven gebruiken voor uw experimenten."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="use-the-sample-data-sets-in-azure-machine-learning-studio"></a>Gebruik van de steekproef gegevensverzamelingen in Azure Machine Learning Studio

[top]: #machine-learning-sample-datasets

Wanneer u een nieuwe werkruimte in Azure Machine Learning maken, kan een aantal steekproeven en experimenten standaard zijn opgenomen. Veel van deze steekproeven worden gebruikt door de voorbeeld-modellen in de [Galerie met Azure Cortana Intelligence](http://gallery.cortanaintelligence.com/)en anderen worden opgenomen als voorbeelden van verschillende soorten gegevens die worden meestal gebruikt in machine learning.

Sommige gegevens zijn beschikbaar in Azure-blobopslag. Voor deze gegevenssets biedt de volgende tabel een directe koppeling. U kunt deze gegevenssets gebruiken in uw experimenten met behulp van de [Gegevens importeren] [ import-data] module.

De rest van deze steekproeven worden vermeld onder **Gegevenssets opgeslagen** in het palet module aan de linkerkant van het tekenpapier experiment wanneer u open of maak een nieuwe experiment in ML Studio.
U kunt een van deze gegevenssets in uw eigen experiment gebruiken door deze te slepen naar uw experiment canvas.


<!--
For a list of sample experiments available in ML Studio, see [Machine Learning Sample Experiments][sample-experiments].

[sample-experiments]: machine-learning-sample-experiments.md
-->

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<table>

<tr>
  <th align=left>De naam van de gegevensset</th>
  <th align=left>Beschrijving van de gegevensset</th>
</tr>

<tr>
  <td valign=top>Volwassen telling inkomsten binaire indeling gegevensset</td>
  <td valign=top>
Een subset van de database 1994 telling, werken volwassenen met ouder zijn dan 16 met een index gecorrigeerde inkomsten > 100 bedraagt.<p> </p><b>Syntaxis:</b> personen doelgroepen gebruiken om te voorspellen of een persoon meer dan 50K per jaar verdient classificeren.<p> </p><b>Gerelateerde onderzoek:</b> Kohavi, R., Becker, B., (1996). UCI Machine Learning-bibliotheek <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School-van-informatie en Computer wetenschappelijk </td>
</tr>

<tr ID=airport-codes-dataset>
  <td valign=top>Airport Codes gegevensset</td>
  <td valign=top>
Amerikaans airport codes.<p> </p>Deze dataset bevat één rij voor elke Amerikaans luchthaven, bieden de airport-id-nummer en de naam samen met de locatie-plaats en provincie.
  </td>
</tr>

<tr>
  <td valign=top>Auto prijsgegevens (onbewerkte)</td>
  <td valign=top>
Informatie over auto door te maken en modelleren, inclusief de prijs op basis van functies zoals het aantal flessen en MPG, evenals een risicoscore verzekeringen.<p> </p>Risicoscore is in eerste instantie gekoppeld aan de prijs voor automatisch en vervolgens aangepast voor werkelijke risico's in een proces actuarissen als symboling voordoen. Een waarde van + 3 geeft aan dat het automatisch risicogevoelig gevonden, en een waarde van-3 dat deze waarschijnlijk heel veilig is.<p> </p><b>Syntaxis:</b> risicoscore voorspellen door functies, met regressie of multivariate classificatie. <p> </p><b>Gerelateerde onderzoek:</b> Schlimmer, J.C. (1987). UCI Machine Learning-bibliotheek <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School-van-informatie en Computer wetenschappelijk </td>
</tr>

<tr ID=bike-rental-uci-dataset>
  <td valign=top>Fietsen huur UCI gegevensset</td>
  <td valign=top>
UCI fietsen huur gegevensset die is gebaseerd op reële gegevens uit kapitaal Bikeshare bedrijf dat een netwerk van de huur fietsen in Washington domeincontroller onderhoudt.<p> </p>De gegevensset heeft één rij voor elk uur aan elke dag in 2011 en 2012 voor een totaal van 17,379 rijen. Het bereik van elk uur fietsen huur ligt tussen 1 en 977.

  </td>
</tr>

<tr ID=bill-gates-rgb-image>
  <td valign=top>Bill Gates RGB-afbeelding</td>
  <td valign=top>
Openbaar beschikbaar afbeeldingsbestand geconverteerd naar CSV-gegevens.<p> </p>De code voor het converteren van de afbeelding wordt weergegeven in de detailpagina model <strong>quantization K-geeft cluster met kleur</strong> .
  </td>
</tr>

<tr>
  <td valign=top>Bloeddrukmeter donatie gegevens</td>
  <td valign=top>
Een subset van gegevens uit de database Bloeddrukmeter donor van de objecten die zijn transfusie Service Center van Hsin Chu City, Taiwan.<p> </p>Donor gegevens bevatten de maanden sinds de laatste donatie), en frequentie of het totale aantal donaties, tijd sinds de laatste donatie en hoeveelheid Bloeddrukmeter donated.<p> </p><b>Syntaxis:</b> het doel is om te voorspellen via classificatie of de donor donated Bloeddrukmeter in maart 2007, waarbij 1 een donor tijdens de doelperiode en 0, geeft een niet-donor. <p> </p><b>Gerelateerde onderzoek:</b> Yeh, I.C., (2008). UCI Machine Learning-bibliotheek <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School-van-informatie en Computer wetenschappelijk <p> </p>Yeh, ik-Cheng, Yang, koning-Jang, en Ting, tik op-Ming, "Knowledge discovery op handleiding bevat model Bernoulli toetsencombinatie," Expert-systemen met toepassingen, 2008, <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr ID=book-reviews-from-amazon>
  <td valign=top>Adresboek recensies van Amazon</td>
  <td valign=top>
Recensies van boeken in Amazon, die u hebt gemaakt via de website amazon.com door universiteit van Pennsylvania onderzoekers (<a href="http://www.cs.jhu.edu/~mdredze/datasets/sentiment/">sentiment</a>). Zie het paper onderzoek "biografieën, Bollywood, giek-vakken en Blenders: domein aanpassing voor Sentiment classificatie" door John Blitzer, markeren Dredze en Fernando Pereira; Koppeling van rekenkundige taalkundige (ACL), 2007.<p> </p>De oorspronkelijke gegevensset heeft 975K beoordelingen met classificatie 1, 2, 3, 4 of 5. De beoordelingen zijn geschreven in het Engels en zijn van de periode 1997-2007. Deze dataset is gedownsampled op 10K beoordelingen.
  </td>
</tr>

<tr>
  <td valign=top>En kanker gegevens</td>
  <td valign=top>
Een van de drie kanker-gerelateerde gegevenssets verstrekt door het Oncology Institute die vaak in machine learning documentatie wordt weergegeven. Diagnostische gegevens met functies van laboratorium analysis van ongeveer 300 automatische voorbeelden gecombineerd.<p> </p><b>Syntaxis:</b> classificeren van het type van kanker, op basis van 9 kenmerken, waarvan sommige zijn lineaire en andere bestaan. <p> </p><b>Gerelateerde onderzoek:</b> Wohlberg, W.H., straat, W.N. en Mangasarian, O.L. (1995). UCI Machine Learning-bibliotheek <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School-van-informatie en Computer wetenschappelijk </td>
</tr>

<tr ID=breast-cancer-features>
  <td valign=top>En kanker functies <td valign=top>
De gegevensset bevat informatie voor 102K verdachte regio's (kandidaten) van de volledige afbeeldingen, door 117 functies beschreven. De functies zijn eigen en hun betekenis niet worden weergegeven door de makers van de gegevensset (Siemens gezondheidszorg). 
  </td>
</tr>

<tr ID=breast-cancer-info>
  <td valign=top>En kanker Info</td>
  <td valign=top>
De gegevensset bevat extra informatie voor elke verdachte gebied van de volledige afbeelding. Elk voorbeeld vindt u informatie (bijvoorbeeld etiket patiënten-ID, coördinaten van patch ten opzichte van de hele afbeelding) over het nummer van de bijbehorende rij in de gegevensset en kanker functies. Elke geduld heeft een aantal voorbeelden. Enkele voorbeelden zijn positieve voor patiënten die een kanker hebben, en sommige negatief zijn. Voor patiënten die geen een kanker hebben, zijn alle voorbeelden negatief. De gegevensset heeft 102K voorbeelden. De gegevensset is gericht, 0,6% van de punten zijn positieve, de rest negatief zijn. De gegevensset is beschikbaar gesteld door Siemens gezondheidszorg.
  </td>
</tr>

<tr ID=crm-appetency-labels-shared>
  <td valign=top>CRM Appetency etiketten gedeeld</td>
  <td valign=top>
Labels van de KDD Cup 2009 klant relatie tekstvoorspelling uitdaging (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr ID=crm-churn-labels-shared>
  <td valign=top>CRM lospeuteren etiketten gedeeld</td>
  <td valign=top>
Labels van de KDD Cup 2009 klant relatie tekstvoorspelling uitdaging (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr ID=crm-dataset-shared>
  <td valign=top>CRM gegevensset gedeeld</td>
  <td valign=top>
Deze gegevens afkomstig van de KDD Cup 2009 klant relatie tekstvoorspelling uitdaging (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).<p> </p>De gegevensset bevat 50K klanten uit het bedrijf Frans Telecom oranje. Elke klant heeft 230 anoniem functies 190 waarvan numerieke worden en 40 zijn bestaan. De functies zijn zeer verspreid.
  </td>
</tr>

<tr ID=crm-upselling-labels-shared>
  <td valign=top>CRM Upselling etiketten gedeeld</td>
  <td valign=top>
Labels van de KDD Cup 2009 klant relatie tekstvoorspelling uitdaging (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td valign=top>Energie Efficiency regressie gegevens</td>
  <td valign=top>
Een verzameling gesimuleerd energie profielen, op basis van 12 ander gebouw vormen. De gebouwen verschillen met betrekking tot met 8 functies, zoals glas gebied, ruiten gebied verdeling en afdrukstand onderscheiden.<p> </p><b>Syntaxis:</b> regressie of classificatie gebruiken om te voorspellen de energie-efficiëntie beoordelen op basis als een van twee werkelijke waarden antwoorden. Voor meerdere klasse-indeling, is afronden de variabele antwoord op het dichtstbijzijnde gehele getal. <p> </p><b>Gerelateerde onderzoek:</b> Xifara, A. & Tsanas, A. (2012). UCI Machine Learning-bibliotheek <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School-van-informatie en Computer wetenschappelijk </td>
</tr>

<tr ID=flight-delays-data>
  <td valign=top>Flight vertraagd gegevens</td>
  <td valign=top>
Personen op tijd prestaties vluchtgegevens die u hebt gemaakt van de gegevensverzameling TranStats van de VS afdeling van transport (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">Op tijd</a>).<p> </p>De gegevensset behandelt de periode April oktober 2013. Voordat u uploadt naar Azure ML Studio, is de gegevensset als volgt verwerkt:<ul><li>De gegevensset is gefilterd, zodat alleen de 70 drukste luchthavens in de Verenigde Amerikaans voorblad</li><li>Geannuleerde vluchten zijn gelabelde vertraging door meer dan 15 minuten</li><li>Omgereden vluchten zijn gefilterd.</li><li>De volgende kolommen zijn geselecteerd: jaar, maand, DayofMonth, dag van de week, Carrier, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, geannuleerd</li></ul>
</td>
</tr>

<tr>
  <td valign=top>Flight op tijd prestaties (onbewerkte)</td>
  <td valign=top>
Records van vliegtuig flight aankomsten en vertrekken binnen de Verenigde Staten van October 2011.<p> </p><b>Syntaxis:</b> voorspellen vluchtvertragingen te zien zijn. <p> </p><b>Gerelateerde onderzoek:</b> van Amerikaanse afdeling van transport <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>.
  </td>
</tr>

<tr>
  <td valign=top>Bos gegevens wordt uitgevoerd</td>
  <td valign=top>
Weergegevens, zoals temperatuur en vochtigheid indexen en windsnelheid, uit een gebied in de regio Noord-Oost Portugal, gecombineerd met de records van bosbranden bevat.<p> </p><b>Syntaxis:</b> dit is een taak moeilijk regressie waar het doel is om te voorspellen het gebrande gebied van bos wordt gestart. <p> </p><b>Gerelateerde onderzoek:</b> Cortez, P., & Morais, A. (2008). UCI Machine Learning-bibliotheek <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School-van-informatie en Computer wetenschappelijk <p> </p>[Cortez en Morais, 2007] P. Cortez en A. Morais. Een Data Mining Approach to voorspellen bos wordt gestart met meteorologische gegevens. In J. Neves, M.f. Santos en J. Machado Eds., nieuwe Trends in kunstmatige Intelligence, procedure van de 13 EPIA 2007 - Portugese vergadering op kunstmatige Intelligence, December, Guimarães, Portugal, p. 512-523, 2007. APPIA, ISBN-13 978-989-95618-0-9. Beschikbaar op: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr ID=german-credit-card-uci-dataset>
  <td valign=top>Duits creditcard UCI gegevensset</td>
  <td valign=top>
De gegevensset UCI Statlog (creditcard Duits) (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + Duits + krediet + gegevens</a>), met behulp van het bestand german.data.<p> </p>De gegevensset classificeert personen, door een verzameling kenmerken, zoals hoog of creditcard risico's beschreven. Elke voorbeeld van voorstelt een persoon. Er zijn 20 functies, numerieke en bestaan, en een binaire label (de waarde krediet risico). Hoge krediet risico vermeldingen hebt label = 2, lage krediet risico vermeldingen hebt label = 1. De kosten van een voorbeeld van de laag risico zo hoog onjuiste is 1, terwijl de kosten van een voorbeeld hoog risico zo klein onjuiste 5.
  </td>
</tr>

<tr ID=imdb-movie-titles>
  <td valign=top>IMDB Filmtitels</td>
  <td valign=top>
De gegevensset bevat informatie over films die zijn geclassificeerd in Twitter tweets: IMDB film--ID, de naam van de film en nieuwe, productie jaar. Er zijn 17K films in de gegevensset. De gegevensset is geïntroduceerd in het papier "S. Dooms, T. De Pessemier en L. Martens. MovieTweetings: een film beoordeling gegevensset die worden verzameld uit Twitter. Workshop over Crowdsourcing en menselijke berekening voor Recommender systemen, CrowdRec met RecSys 2013."
  </td>
</tr>

<tr>
  <td valign=top>Pascaline twee class gegevens</td>
  <td valign=top>
Dit is misschien de beste bekende database worden gevonden in de documentatie van de herkenning patroon. De gegevensset is relatief klein, met van 50 voorbeelden van Bloemblad afmetingen drie Pascaline worden onderscheiden van.<p> </p><b>Syntaxis:</b> voorspellen van het type Pascaline uit de afmetingen.  <p> </p><b>Gerelateerde onderzoek:</b> Fisher, R.A. (1988). UCI Machine Learning-bibliotheek <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School-van-informatie en Computer wetenschappelijk </td>
</tr>

<tr ID=movie-tweets>
  <td valign=top>Film Tweets</td>
  <td valign=top>
De gegevensset is een uitgebreide versie van de gegevensset film Tweetings. De gegevensset scoort 170K voor films opgehaald uit gestructureerde tweets op Twitter. Elk exemplaar vertegenwoordigt een tweet en is van een tupel: gebruikers-ID, IMDB film-ID, classificatie, tijdstempel, aantal favorieten voor dit tweet en aantal retweets van deze tweet. De gegevensset is ter beschikking gesteld door A. gezegd, S. Dooms, B. Loni en D. Tikk voor Recommender systemen uitdaging 2014.
  </td>
</tr>

<tr>
  <td valign=top>MPG gegevens voor verschillende auto</td>
  <td valign=top>
Deze dataset is een iets gewijzigd versie van de gegevensset die is verstrekt door de bibliotheek StatLib van Carnegie Mellon University. De gegevensset is in het 1983 American statistische koppeling gebruikt.<p> </p>De gegevens bevat brandstofverbruik voor verschillende auto in mijl per gallon, samen met informatie, zoals het aantal flessen, engine verplaatsing paardenkracht, totaal gewicht en hardwareversnelling.<p> </p><b>Syntaxis:</b> voorspellen brandstofverbruik op basis van 3 met meerdere waarden aparte kenmerken en 5 continue kenmerken. <p> </p><b>Gerelateerde onderzoek:</b> StatLib, Carnegie Mellon University (1993). UCI Machine Learning-bibliotheek <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School-van-informatie en Computer wetenschappelijk </td>
</tr>

<tr>
  <td valign=top>Pima Indianen Diabetes binaire indeling gegevensset</td>
  <td valign=top>
Een subset van gegevens uit de nationale Institute van Diabetes en spijsverteringskanaal en nieren toegekend database. De gegevensset is gefilterd op focus op vrouw patiënten van Indiase erfgoed Pima. De gegevens bevat medische gegevens zoals glucose en of niveaus, evenals leven factoren.<p> </p><b>Syntaxis:</b> voorspellen of het onderwerp diabetes (binaire classificatie) heeft. <p> </p><b>Gerelateerde onderzoek:</b> Sigillito, tegen (1990). UCI Machine Learning-bibliotheek <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml "</a>. Irvine, CA: Universiteit van Californië, School-van-informatie en Computer wetenschappelijk </td>
</tr>

<tr>
  <td valign=top>Gegevens van de klant restaurant</td>
  <td valign=top>
Een reeks metagegevens over klanten, waaronder doelgroepen en voorkeuren.<p> </p><b>Syntaxis:</b> deze dataset, gebruiken in combinatie met de andere twee restaurant gegevenssets, trainen en test u een recommender-systeem. <p> </p><b>Gerelateerde onderzoek:</b> Bache, K. en Lichman, M. (2013). UCI Machine Learning-bibliotheek <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en Computer wetenschappelijk.
  </td>
</tr>

<tr>
  <td valign=top>Gegevens van restaurant-functie</td>
  <td valign=top>
Een reeks metagegevens over restaurants en hun functies, zoals eten type, dineren stijl en locatie.<p> </p><b>Syntaxis:</b> deze dataset, gebruiken in combinatie met de andere twee restaurant gegevenssets, trainen en test u een recommender-systeem. <p> </p><b>Gerelateerde onderzoek:</b> Bache, K. en Lichman, M. (2013). UCI Machine Learning-bibliotheek <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en Computer wetenschappelijk.
  </td>
</tr>

<tr>
  <td valign=top>Restaurant classificaties</td>
  <td valign=top>
Classificaties gegeven door gebruikers restaurants op schaal van 0 naar 2 bevat.<p> </p><b>Syntaxis:</b> deze dataset, gebruiken in combinatie met de andere twee restaurant gegevenssets, trainen en test u een recommender-systeem. <p> </p><b>Gerelateerde onderzoek:</b> Bache, K. en Lichman, M. (2013). UCI Machine Learning-bibliotheek <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en Computer wetenschappelijk.
  </td>
</tr>

<tr>
  <td valign=top>Staal Annealing meerdere class gegevensset</td>
  <td valign=top>
Deze dataset bevat een reeks records uit staal aanhechten experimenten met de fysieke kenmerken (breedte, dikte of type (spiraal, blad, enzovoort) de resulterende staal typen.<p> </p><b>Syntaxis:</b> voorspellen twee numerieke class kenmerken; hardheid of sterkte. U kunt ook correlatie tussen kenmerken analyseren.<p> </p>Een set-standaard, volgt u staal cijfers gedefinieerd door SAE en andere organisaties. U zoekt van een specifiek 'grade' (de klassevariabele) en wilt u meer informatie over de waarden die nodig zijn. <p> </p><b>Gerelateerde onderzoek:</b> pond, D. & Buntine, W., (NA). UCI Machine Learning-bibliotheek <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School-van-informatie en Computer wetenschappelijk <p> </p>Een handige handleiding voor het staal cijfers vindt u hier: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td valign=top>Telescoop gegevens</td>
  <td valign=top>
Records van hoge energie gamma onderdeel bursts samen met achtergrondruis, die beide gesimuleerd, met behulp van een Monte Carlo proces.<p> </p>Het doel van de simulatie is om de nauwkeurigheid van grond gebaseerde de Cherenkov gamma kijkers, statistische methoden gebruiken om onderscheid tussen de gewenste signaal (Cherenkov straling douches) en de achtergrondruis (hadronic douches gestart door cosmic stralen in de rechterbovenhoek lucht) te vergroten.<p> </p>De gegevens zijn vooraf verwerkte voor het maken van een lange cluster waarbij de long as is gericht op het camera-beheercentrum. De kenmerken van dit ellips, (vaak Hillas parameters genoemd) behoren tot de parameters van de afbeelding die kunnen worden gebruikt voor onderscheid.<p> </p><b>Syntaxis:</b> voorspellen of afbeelding van Welkom ruis signaal of achtergrondafbeelding vertegenwoordigt.<p> </p><b>Opmerkingen:</b> eenvoudige classificatie nauwkeurigheid is niet zinvol voor deze gegevens sinds de classificatie van een gebeurtenis achtergrond, zoals signaalniveau is dan de classificatie van een signaalgebeurtenis als achtergrond. Voor vergelijking van verschillende classificaties moet in de grafiek ROC worden gebruikt. De kans van het accepteren van een gebeurtenis achtergrond, zoals signaalniveau lager dan een van de volgende drempels moet: 0,01, 0,02, 0,05, 0,1 of 0,2.<p> </p>Ook, houd er rekening mee dat wordt het aantal achtergrond gebeurtenissen (h, voor hadronic douches) onderschat, terwijl in reële afmetingen: de klas h of ruis staat voor de meeste gebeurtenissen. <p> </p><b>Gerelateerde onderzoek:</b> Bock, R.K. (1995). UCI Machine Learning-bibliotheek <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van informatie </td>
</tr>

<tr ID=weather-dataset>
  <td valign=top>Weer gegevensset</td>
  <td valign=top>
Ieder uur weer grond-metingen uit NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">samengevoegde gegevens uit 201304 naar 201310</a>).<p> </p>De weergegevens behandelt observaties door de airport weer stations, die betrekking hebben op de periode April oktober 2013. Voordat u uploadt naar Azure ML Studio, is de gegevensset als volgt verwerkt:<ul><li>Weer station-id's zijn toegewezen aan de bijbehorende airport id 's</li><li>Weer stations niet zijn gekoppeld aan de 70 drukste luchthavens zijn gefilterd.</li><li>De datumkolom is in aparte jaar, maand en dag kolommen gesplitst</li><li>De volgende kolommen zijn geselecteerd: AirportID, jaar, maand, dag, tijd, Tijdzoneverschil, SkyCondition, zichtbaarheid, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, windsnelheid, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Altimeter</li></ul>
  </td>
</tr>

<tr ID=wikipedia-sp-500-dataset>
  <td valign=top>Wikipedia SP 500 gegevensset</td>
  <td valign=top>
Gegevens wordt afgeleid van Wikipedia (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) op basis van artikelen van elk bedrijf S & P 500, die zijn opgeslagen als XML-gegevens.<p> </p>Voordat u uploadt naar Azure ML Studio, is de gegevensset als volgt verwerkt:<ul><li>Tekst extraheren voor elk specifiek bedrijf</li><li>Wiki-opmaak verwijderen</li><li>Niet-alfanumerieke tekens verwijderen</li><li>Alle tekst omzetten in kleine letters</li><li>Bekende bedrijf categorieën zijn toegevoegd</li></ul><p> </p>Opmerking dat voor sommige bedrijven van een artikel kan niet worden gevonden, zodat het aantal records minder dan 500 is.
  </td>
</tr>





<tr ID=direct-marketing>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td valign=top>
De gegevensset bevat klantgegevens en aanduidingen over hun antwoord op een directe postadres campagne. Elke rij vertegenwoordigt een klant. De gegevensset 9 functies over gebruiker doelgroepen en eerdere gedrag en 3 label kolommen bevat (bezoekt, conversie, en besteedt).  Bezoek is een binaire kolom waarin wordt aangegeven dat een klant na de marketingcampagne bezocht, conversie geeft aan een klant een ander nummer hebt gekocht en besteedt is het bedrag dat is besteed.  De gegevensset is beschikbaar gesteld door Kevin Hillstrom voor MineThatData e-analyses en Data Mining uitdaging.
  </td>
</tr>

<tr ID=lyrl2004-tokens-test>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td valign=top>
Functies van test voorbeelden in de RCV1-V2 Reuters nieuws gegevensset. De gegevensset bevat 781K nieuwsartikelen samen met hun id's (eerste kolom van de gegevensset). Elk artikel is ge? exeerd, stopworded, en voort. De gegevensset is beschikbaar gesteld door David. D TE DRUKKEN. Leistra.
  </td>
</tr>

<tr ID=lyrl2004-tokens-train>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td valign=top>
Functies van training voorbeelden in de RCV1-V2 Reuters nieuws gegevensset. De gegevensset bevat 23K nieuwsartikelen samen met hun id's (eerste kolom van de gegevensset). Elk artikel is ge? exeerd, stopworded, en voort. De gegevensset is beschikbaar gesteld door David. D TE DRUKKEN. Leistra.
  </td>
</tr>

<tr ID=intrusion-detection>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td valign=top>
DataSet van de KDD Cup 1999 Knowledge Discovery en hulpmiddelen voor concurrenten (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>) voor datamining.<p> </p>De gegevensset is gedownload en die zijn opgeslagen in Azure-blobopslag (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) en bevat zowel training en gegevenssets testen. De gegevensset training heeft ongeveer 126K rijen en 43 kolommen, inclusief de kolomlabels; 3 kolommen maken deel uit van de labelgegevens en 40 kolommen, bestaande uit de functies van numerieke en tekenreeks/bestaan, zijn beschikbaar voor het model training. De testgegevens heeft ongeveer 22,5 K voorbeelden met dezelfde 43 kolommen zoals in de trainingsgegevens testen.

  </td>
</tr>

<tr ID=rcv1-v2-topics-qrels>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1-v2.topics.qrels.csv</a></td>
  <td valign=top>
Onderwerp toewijzingen voor nieuwsartikelen in de RCV1-V2 Reuters nieuws gegevensset. Een nieuwsartikel kan worden toegewezen aan verschillende onderwerpen. De indeling van elke rij is '<topic name>  <document id> 1". De gegevensset bevat 2.6M onderwerp toewijzingen. De gegevensset is beschikbaar gesteld door David. D TE DRUKKEN. Leistra.
  </td>
</tr>

<tr ID=student-performance>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td valign=top>
Deze gegevens afkomstig van de KDD Cup 2010 studenten prestaties evaluatie uitdaging (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">studenten prestaties evaluatie</a>). De gegevens die worden gebruikt, is de Algebra_2008_2009 trainingsset (Stamper, J., Niculescu-Mizil, A., Ritter, S., drempel, G.J. en Koedinger, K.R. (2010). Wiskundige ik 2008-2009. Vraag gegevensverzameling uit KDD Cup 2010 educatieve Data Mining uitdaging. Lees deze via <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> of <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.<p> </p>De gegevensset is gedownload en die zijn opgeslagen in Azure-blobopslag (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) en logboekbestanden van een student Bijles systeem bevat. De opgegeven functies zijn probleem-ID en een korte beschrijving, student-ID, tijdstempel en hoeveel pogingen de studenten voor het oplossen van het probleem in de juiste manier uitgevoerd. De oorspronkelijke gegevensset 8,9 M records heeft, is deze dataset gedownsampled naar de eerste 100K rijen. De gegevensset 23 tabs gescheiden kolommen met verschillende typen bevat: numerieke waarden, bestaan, en tijdstempel.

  </td>
</tr>




</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
