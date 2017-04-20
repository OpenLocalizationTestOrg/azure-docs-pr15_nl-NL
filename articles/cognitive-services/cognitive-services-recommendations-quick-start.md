<properties
    pageTitle="Aan de slag met: Machine Learning aanbevelingen API | Microsoft Azure"
    description="Azure Machine Learning-aanbevelingen - aan de slag met"
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
    ms.date="08/22/2016"
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-cognitive-services-recommendations-api"></a>Snel aan de slag-handleiding voor de cognitieve Services aanbevelingen-API

In dit document wordt beschreven hoe naar ingebouwde uw service of de toepassing voor het gebruik van de [Aanbevelingen API](http://go.microsoft.com/fwlink/?LinkId=759710).
U vindt meer informatie over de API aanbevelingen en andere cognitieve Services [hier](http://go.microsoft.com/fwlink/?LinkId=759709). In deze handleiding vindt u mogelijk ook de [Aanbevelingen API Reference](http://go.microsoft.com/fwlink/?LinkId=759348) handige.


<a name="Overview"></a>
## <a name="general-overview"></a>Algemeen overzicht

Dit document is een stapsgewijze handleiding. Ons doel is Doorloop de stappen die nodig zijn voor een model trainen en wijst u resources waarmee u het model van uw productieomgeving in beslag neemt.

Deze oefening duurt ongeveer 30 minuten.

Als u wilt [Aanbevelingen API](http://go.microsoft.com/fwlink/?LinkId=759710)gebruiken, moet u de volgende stappen uit:

1. Een model maken: een model is een container met uw gegevens over zoekgebruik, catalogusgegevens en het model aanbeveling.
1. Catalogus importgegevens - een catalogus bevat metagegevens informatie over de items.
1. Gebruik importgegevens - gebruiksgegevens kunnen worden geüpload in 2 manieren (of beide):
  -  Door het uploaden van een bestand met de gebruiksgegevens.
  -  Gegevens acquisition gebeurtenissen verzenden.
  U uploaden meestal een gebruik-bestand om te kunnen maken een eerste aanbeveling-model (bootstrap) en deze gebruiken totdat het systeem voldoende gegevens worden verzameld met behulp van de overname gegevensindeling.
1. Maken van een model aanbeveling: dit is een asynchrone bewerking waarin het systeem aanbeveling duurt de gebruiksgegevens en maakt een model aanbeveling. Deze bewerking kan enkele minuten of enkele uren afhankelijk van de grootte van de gegevens en de parameters van de configuratie opbouwen nemen. Wanneer het bouwen activeert, krijgt u een opbouwen-ID. Gebruik deze om te controleren wanneer het proces opbouwen voordat u begint met het gebruiken van aanbevelingen is beëindigd.
1. Gebruik aanbevelingen - Get-aanbevelingen voor een specifiek item of de lijst met items.

<a name="GetStarted"></a>
### <a name="lets-get-started"></a>Laten we aan de slag!

U wilt beginnen met het samenstellen van een model aanbevelingen. Vervolgens gaat we u gids over het gebruik van de resultaten gegenereerd door het model power aanbevelingen op een e-commerce-site.

<a name="Ex1Task1"></a>
#### <a name="task-1---signing-up-for-the-recommendations-api"></a>Taak 1 - ondertekend voor de API aanbevelingen ####

In deze taak moet u zich registreren voor een de aanbevelingen API-service en maken van een model aanbevelingen.

1. Ga naar de **Portal van Azure** op [http://portal.azure.com/](http://portal.azure.com/) en u aanmelden met uw Azure-account.

1.  Klik op **+ nieuwe**.

1. Selecteer de optie **Intelligence** .

1. Selecteer het product **Cognitieve Services-API's** .
Voor dit product kunt u een abonnement voor alle cognitieve services API's (nominale, tekst analyses, Computer Vision, enzovoort). Vandaag gaan we in op de API aanbevelingen.

1. Voer de **naam van het Account** voor uw abonnement aanbevelingen op de openingspagina cognitieve Services-API. (Voor instace: "MyRecommendations"). Deze naam mogen geen spaties in deze.

1. Selecteer op de **API-type**, **aanbevelingen**.

1. Klik op **prijzen laag**, kunt u een abonnement selecteren. U kunt de laag **gratis** voor 10.000 transacties/maand kunt selecteren. Dit is een gratis abonnement, dus het is een goede manier om te beginnen met het evalueren van het systeem. Als u naar productie gaat, wordt aangeraden u kunt u het volume van de aanvraag en wijzigt het type dienovereenkomstig gewijzigd. (Notitie: U kunt slechts één gratis laag abonnement per keer hebt)

1. Selecteer een **Resourcegroep**of maak een nieuw als u nog niet hebt.

1. Andere elementen in het dialoogvenster maken, kunt u wijzigen. We even op wijzen dat de resource-provider vandaag wordt alleen ondersteund in de Verenigde Staten-datacenters.

1. Als u klaar bent met selecties, klikt u op **maken**.

1. Wacht enkele minuten voor de resource kan worden geïmplementeerd.
Zodra deze is geïmplementeerd, kunt u naar de sectie **toetsen** in het blad **Instellingen** waar u een primaire en secundaire sleutel krijgt in de API gaan.  De primaire sleutel, moet u dit bij het maken van uw eerste model kopiëren.

<a name="Ex1Task2"></a>
#### <a name="task-2---did-you-bring-your-data"></a>Taak 2: hebt u brengen uw gegevens? ####

De API aanbevelingen leert uit de catalogus en uw transacties kan alleen worden aangeboden goede aanbevelingen. Dat betekent dat u moet deze feed met goede gegevens over uw producten (verwijst naar dit een **catalogus** -bestand) en een reeks transacties groot genoeg is om te zoeken naar interessante patronen van verbruik (verwijst naar deze **Gebruik**).

1. Meestal wilt u uw database transacties voor deze soorten informatie zoeken.
Als u geen van deze handige, hebben we enkele voorbeeldgegevens verstrekt voor u op basis van Microsoft Store transactiegegevens.

 U kunt de gegevens downloaden van [hier](http://aka.ms/RecoSampleData). Kopiëren en de MsStoreData.Zip uitpakken naar een map op uw lokale computer.

 > **Notitie:** De voorbeeldcode die u kunt downloaden en uitvoeren in taak 3 heeft voorbeeldgegevens al ingesloten hierin--zodat deze taak optioneel is.  Echter wel verwijzen, deze taak kunt u meer natuurlijke gegevenssets downloaden en kunt u meer informatie over de invoer in een de aanbevelingen van betere API.

1.  Nu we even verder kijken aan het catalogusbestand. Navigeer naar de locatie waar u de gegevens hebt gekopieerd.
 Open het catalogusbestand in **Kladblok**.

 U ziet dat het catalogusbestand heel eenvoudig. Deze heeft de volgende indeling`<itemid>,<item name>,<product category>`

 >  AAA-04294, OfficeLangPack 2013 32/64 E34 Online DwnLd, Office <br>
 > AAA-04303, OfficeLangPack 2013 32/64 en Online DwnLd, Office  <br>
 > C9F-00168, KRUSELL Kiruna spiegelen omslag voor Nokia Lumia 635 - kamelen, Bureau-accessoires

 We moet Wijs een catalogusbestand kan veel rijkere, dat u kunt bijvoorbeeld metagegevens over de producten (verwijst naar deze *item functies*) toevoegen. Klik op de indeling voor catalogusitem ziet u de sectie [indeling voor catalogusitem](http://go.microsoft.com/fwlink/?LinkID=760716) in de verwijzing API voor meer informatie.

1. Laten we Doe dit ook met de gegevens over zoekgebruik. U ziet dat de datum van het gebruik van de indeling `<User Id>,<Item Id>,<Time Stamp>,<Event>`.

  > 00037FFEA61FCA16, 288186200, 2015-08/04T11:02:52, aankoop 0003BFFDD4C2148C, 297833400, 2015-08/04T11:02:50, aankoop 0003BFFDD4C2118D, 297833300, 2015-08/04T11:02:40, aankoop 00030000D16C4237, 297833300, 2015-08/04T11:02:37, aankoop 0003BFFDD4C20B63, 297833400, 2015-08/04T11:02:12, aankoop 00037FFEC8567FB8, 297833400, 2015-08/04T11:02:04, aanschaffen

U ziet dat de eerste drie elementen verplicht. Het gebeurtenistype is optioneel. U kunt de [Opmaak van Resourcegebruik](http://go.microsoft.com/fwlink/?LinkID=760712) voor meer informatie over dit onderwerp raadplegen.

 > **Hoeveel gegevens hebt u nodig?**
 <p>
Ook dit echt is afhankelijk van de gegevens over zoekgebruik zelf. Het systeem leert wanneer gebruikers kopen voor verschillende items. Voor sommige builds zoals FBT is het belangrijk te weten welke items worden gekocht in de dezelfde transacties. (Verwijst naar deze *collega exemplaren*). Een goede vuistregel is dat de meeste items zich op 20 transacties of meer, dus als u had 10.000 items in de catalogus, zou te raden u ten minste 20 maal het nummer van transacties of ongeveer 200.000 transacties.
Nogmaals, is dit een vuistregel. U wilt experimenteren met uw gegevens.
</p>

<a name="Ex1Task3"></a>
####Taak 3: een model aanbevelingen maken####

Nu dat u een account hebt en u gegevens hebt, laten we uw eerste model te maken.

In deze taak gebruikt u de voorbeeldtoepassing maken van uw eerste model.

1. Ten eerste moet u rekening houden met de [Aanbevelingen API verwijzing](http://go.microsoft.com/fwlink/?LinkId=759348).

1. De [Toepassing van de steekproef](http://go.microsoft.com/fwlink/?LinkID=759344) naar een lokale map downloaden.

1. Open in Visual Studio de **RecommendationsSample.sln** -oplossing zich in de map **C#** .

1. Open het bestand **SampleApp.cs** . Opmerking dat de volgende stappen uit in het bestand:
 + Een Model maken
 + Een catalogusbestand toevoegen
 + Een bestand gebruik toevoegen
 + Een opbouwen activeren voor het Model
 + Een aanbeveling op basis van een paar items ophalen
<p></p>

1. Vervang de waarde voor het veld **AccountKey** door de sleutel uit taak 1.

1. De oplossing te doorlopen en u wilt zien hoe een model wordt gemaakt.

1. Probeer de catalogus en het gebruik bestanden die u zojuist hebt gedownload als u wilt maken van een nieuw model voor de Microsoft Store of voor adresboek aanbevelingen vervangen. U moet ook de naam van het model en de items waarvoor u aanbevelingen aanvragen wijzigen.

1. Als het model wordt gemaakt, moet u rekening met het **model-ID** als wordt nodig bij het aanvragen van aanbevelingen in uw productieomgeving.

>  Meer informatie over het typen en hoe u het evalueren van de kwaliteit van builds bouwen [hier](cognitive-services-recommendations-buildtypes.md).

<a name="Ex1Task4"></a>
### <a name="putting-your-model-in-production"></a>Zet uw model op productie! ###

Nu dat u hoe weet u een model maken en gebruiken van aanbevelingen, is de volgende stap in productie op uw website mobiele toepassing plaatsen of integreren met uw CRM of ERP-systeem.
Duidelijk, elk van deze implementaties zou verschillende. Aangezien de API aanbevelingen om worden gevraagd een webservice, moet u mogelijk zijn deze eenvoudig bellen vanuit een van deze verschillende omgevingen.

**Belangrijke:**  Als u wilt weergeven van aanbevelingen van een openbare client (bijvoorbeeld uw e-commerce-site), moet u een proxyserver om aan te bieden de aanbevelingen maken. Dit is belangrijk dat uw API-sleutel wordt niet blootgesteld aan externe (mogelijk niet worden vertrouwd) entiteiten.

Hier volgen enkele tips locaties waar kunt u aanbevelingen gebruiken:

### <a name="product-page"></a>Productpagina

**Item aanbevelingen**
<p>Als het model is trainen op aanschaffen gegevens, kan deze de klant te *vinden van producten die zijn waarschijnlijk van belang zijn voor personen die het bronitem hebt gekocht*.</p>
<p>Klik op gegevens, als het model is training op het uw klant te *vinden van producten die waarschijnlijk te bezoeken door personen die het bronitem hebt bezocht zijn*toegestaan. Dit soort model kan vergelijkbare items retourneren.</p>

**Vaak hebt gekocht bij elkaar aanbevelingen**
<p>A vaak hebt gekocht bij elkaar opbouwen kan worden training, zodat u toegang hebt sets met items die kunnen worden aangeschaft samen met dit item zijn.</p>

### <a name="check-out-page"></a>Bekijk de pagina

**Item aanbevelingen**
<p>Een aanbevelingen model kan duren als invoer voor een lijst met items. Zo kunt u alle items in de Favorieten doorgeven als invoer voor aanbevelingen krijgen.
Het model, in dit geval vindt u aanbevelingen die van belang dat alle items in de Favorieten gegeven.
</p>

### <a name="landing-page"></a>Dialoogvenster aantekening toevoegen

**Gebruiker aanbevelingen**
<p>
Een aanbevelingen model kan duren als invoer voor de gebruikers-id.  Hiermee wordt de geschiedenis van transacties die de gebruiker gebruiken om persoonlijke aanbevelingen aan de gebruiker opgegeven.
</p>

Bekijk de [Documentatie voor Item aanbevelingen ophalen](http://go.microsoft.com/fwlink/?LinkID=760719).

<a name="Ex1Task6"></a>
### <a name="whats-next"></a>Hoe nu verder?
Gefeliciteerd als u deze dit hebt aangebracht uiterst! Meer informatie over dat meer u kunt de volledige [Aanbevelingen API verwijzing](http://go.microsoft.com/fwlink/?LinkId=759348) als u vragen hebt, u altijd contact met ons opnemen opmlapi@microsoft.com
