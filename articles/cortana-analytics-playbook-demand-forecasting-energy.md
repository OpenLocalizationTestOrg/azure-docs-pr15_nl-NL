<properties
    pageTitle="Cortana Intelligence oplossing sjabloon Playbook voor aanvraag prognoses van energie | Microsoft Azure"
    description="Een oplossing-sjabloon met Microsoft Cortana Intelligence waarmee de aanvraag voor een bedrijf, energie hulpprogramma prognose."
    services="cortana-analytics"
    documentationCenter=""
    authors="ilanr9"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/24/2016"
    ms.author="ilanr9;yijichen;garye"/>

# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>Cortana Intelligence oplossing sjabloon Playbook voor aanvraag prognoses van energie  

## <a name="executive-summary"></a>Samenvatting voor leidinggevenden  

In de afgelopen jaren, hebt Internet van dingen (IoT), alternatieve energiebronnen en groot gegevens samengevoegd tot vast verkoopkansen in het hulpprogramma en energie-domein. Tegelijkertijd, hebben het hulpprogramma en de energiesector hele verbruik samenvoegen af met consumenten vragen betere manieren om te bepalen van het gebruik van energie gezien. Daarom zijn het hulpprogramma en bedrijven die slimme raster uitstekende nodig zijn om te gebruiken voor vernieuwing en verlengen zelf. Groot aantal hulpprogramma's en power rasters worden bovendien steeds verouderde en duur onderhouden en beheren. Tijdens het laatste jaar werkt het team op een aantal afspraken binnen het domein energie. Tijdens deze afspraken, er is opgetreden veel gevallen waarin de hulpprogramma's of ISV's (onafhankelijke softwareleveranciers) bij de prognoses voor toekomstige energie vraag hebt is gevonden. Deze prognoses een belangrijke rol spelen in hun huidige en toekomstige bedrijf en de basis voor verschillende gebruik gevallen zijn geworden. Het gaat hierbij om korte en lange power laden prognose, handelsdag, taakverdeling, raster optimalisatie, enzovoort. BIG data en geavanceerde analyses (AA) methoden zoals Machine Learning (ML) zijn de belangrijkste om voor nauwkeurige en betrouwbare prognoses produceren.  

In dit playbook, we samenvoegen de business en analytical richtlijnen die u nodig hebt voor een succesvolle ontwikkeling en implementatie van energie aanvraag voorspellen-oplossing. Aan de hand van deze voorgestelde richtlijnen kunt hulpprogramma's, gegevens wetenschappers en gegevens engineers bij het vaststellen van volledig geoperationaliseerd, cloudgebaseerde, voorspellen van de vraag oplossingen. Voor bedrijven die hun big data en geavanceerde analyses reis net beginnen, kan een dergelijke oplossing het basisgetal in hun langdurige slimme raster strategie vertegenwoordigen.

>[AZURE.TIP] Als u wilt downloaden van een diagram dat de architectuur van deze sjabloon overzicht, raadpleegt u [Cortana Intelligence oplossing sjabloon architectuur voor aanvraag prognoses van energie](cortana-analytics-architecture-demand-forecasting-energy.md).  

## <a name="overview"></a>Overzicht  

In dit document behandelt de bedrijven, gegevens en technische aspecten van het gebruik van Cortana Intelligence en in bepaalde Azure Machine Learning (AML) voor de uitvoering en implementeren van energie prognoses oplossingen. Het document bestaat uit drie hoofdonderdelen:  

1. Bedrijven begrijpen  
2. Lidmaatschap van gegevens  
3. Technische-implementatie

Het **Lidmaatschap van de bedrijven** deel vindt u een overzicht van het bedrijf aspect één moet begrijpen en houd rekening met voordat u een investering overgaat. Dit wordt uitgelegd hoe u het probleem bedrijven waaraan u bezig bent om ervoor te zorgen dat Bekijk analyses en machine learning daadwerkelijk effectieve en van toepassing zijn. Verder in het document wordt uitgelegd dat de basisbeginselen van machine learning en hoe deze worden gebruikt om de problemen energie prognoses. Deze bevat de vereisten en de criteria voor hoger onderwijs van een use-case. Sommige steekproef gebruik gevallen en bedrijfsscenario scenario's zijn ook beschikbaar.

Gegevens is het hoofdbestanddeel voor elke machine learning-oplossing. Het **Lidmaatschap van de gegevens** -deel van dit document worden enkele belangrijke aspecten van de gegevens. Het overzicht van het type gegevens dat nodig is voor energie prognoses, gegevens kwaliteitsvereisten en welke gegevensbronnen meestal bestaat. Ook wordt uitgelegd hoe de onbewerkte gegevens worden gebruikt voor het voorbereiden van gegevensfuncties die daadwerkelijk station het deel achter de modellering.

Het derde onderdeel van het document behandelt de **Implementatie van technische** aspecten van een oplossing. Technische functie en modellering zijn de basis van het proces van de wetenschappelijke gegevens en daarom worden besproken in sommige details. Deze het concept van het webservices een belangrijk voertuig voor cloud-implementatie van oplossingen Bekijk analyses zijn, bedekt. We ook een overzicht maken van een typisch architectuur van een end-to-end operationalized oplossing.

Het document bevat bovendien referentiemateriaal die u gebruiken kunt om te krijgen verder begrip van het domein en technologie.

Houd er rekening mee dat we niet wilt begeleidende in dit document het grondigere proces in de wetenschappelijke gegevens, is de wiskundige en technische aspecten. Deze gegevens vindt u in de [documentatie van Azure ML](http://azure.microsoft.com/services/machine-learning/) en [blogs](http://blogs.microsoft.com/blog/tag/azure-machine-learning/).

### <a name="target-audience"></a>Doelgroep   
De doelgroep voor dit document is zowel zakelijke en technische personeel wilt krijgen knowledge en lidmaatschap van Machine Learning op basis van oplossingen en hoe deze worden gebruikt specifiek binnen het domein energie prognoses.

Gegevens wetenschappers profiteert ook bij het lezen van dit document als u wilt een beter begrip van het hoogste niveau proces dat de implementatie van een oplossing prognoses energie stations krijgen. In deze context kan ook worden gebruikt tot stand brengen van een goede basislijn en het beginpunt voor meer gedetailleerde en geavanceerde materiaal.

### <a name="industry-trends"></a>Trends in de bedrijfstak  
In de afgelopen jaren, hebt IoT, alternatieve energiebronnen en groot gegevens samengevoegd tot vast verkoopkansen in de ruimte hulpprogramma en energie. Tegelijkertijd, hebben het hulpprogramma en de volledige energiesectoren verbruik samenvoegen af met consumenten vragen betere manieren om te bepalen van het gebruik van energie gezien.

Veel hulpprogramma en bedrijven die slimme energie hebben de [slimme raster](https://en.wikipedia.org/wiki/Smart_grid) is pioneering door het implementeren van een aantal zaken waardoor het gebruik van de gegevens die zijn gegenereerd door het raster gebruiken. Gebruik vaak draaien rond de inherent kenmerken van elektriciteit: kan niet worden samengevoegd of Reserveer als voorraad worden opgeslagen. Ja, wat is geproduceerd moet worden gebruikt. Hulpprogramma's die u wilt weten hoe u efficiënter energieverbruik gewoon prognose nodig omdat die deze meer mogelijkheden voor **balance '-en aanbod krijgt**, waardoor energie verliezen, **handel gas uitstoot verkleinen**en besturingselement kosten.

Wanneer het woord van kosten, moet u er een ander belangrijk aspect, dat wil prijs zeggen is. Nieuwe mogelijkheden voor de handel power tussen hulpprogramma's hebben een geweldige nodig zijn om te **voorspellen toekomstige vraag en toekomstige prijs van elektriciteit**gebracht. Dit kan helpen bedrijven hun productievolumes bepalen.

Als we het woord 'slim' gebruikt, Raadpleeg we daadwerkelijk op een raster waarmee u kunt meer informatie en breng vervolgens voorspellingen. Deze kunt seizoensgebonden wijzigingen in verbruik, evenals **tijdelijke overbelasting situaties voorzien en automatisch aanpassen voor deze**anticiperen. Door te bepalen op afstand verbruik (met behulp van deze slimme meter), kunnen gelokaliseerde overbelasting situaties worden verwerkt. **Door eerst voorspellingen te doen en klikt u vervolgens fungeert**, het raster kunt u zelf nog beter kunnen na verloop van tijd.

Voor de rest van dit document gaan we in op een specifieke reeks gebruik gevallen waarin prognoses van toekomst, korte termijn, en de lange termijn energie vraag. We hebt gewerkt in deze gebieden voor een paar maanden en hebt ervaring sommige kennis en vaardigheden die kunnen we industrie grade resultaten wilt produceren. Andere gevallen gebruik worden behandeld als u ook in het document in de nabije toekomst.

## <a name="business-understanding"></a>Bedrijven begrijpen

### <a name="business-goals"></a>Bedrijfsdoelstellingen
Het **Energie Demo** -doel is om te laten zien van een typisch Bekijk analyses en machine learning-oplossing die kan worden geïmplementeerd in een frame zeer korte tijd. Met name is onze huidige focus over het inschakelen van energie aanvraag voorspellen oplossingen, zodat de bedrijfswaarde kan snel worden gerealiseerd en gebruikt na het. De informatie in deze playbook kan de klant voor het uitvoeren van de volgende doelstellingen helpen:
-   Korte tijdnotatie op machine learning-waarde op basis van oplossing
-   Mogelijkheid om uit te vouwen van een pilot use-case naar andere gevallen gebruiken of naar een uitgebreidere bereik op basis van hun zakelijke behoeften
-   Snel krijgen Cortana Intelligence Suite product knowledge

Met deze doelstellingen in gedachten beoogd deze playbook geven van de bedrijven en technische kennis die helpt bij de realisatie van deze doelstellingen.

### <a name="power-load-and-demand-forecasting"></a>Power laden en aanvraag prognoses
Binnen de energiesector, kan er tal van manieren waarop vraag prognoses kan helpen kritieke zakelijke problemen kunt oplossen. In feite kan aanvraag prognoses worden beschouwd de basis voor veel gevallen van core gebruiken in de industrie. In het algemeen, we twee soorten energie aanvraagprognoses overwegen: korte termijn en de lange termijn. Elk mogelijk een ander doel dienen en een andere benadering gebruiken. Het belangrijkste verschil tussen de twee is de prognose horizon, wat betekent dat het bereik van de tijd in de toekomst waarvoor we zou prognose.

#### <a name="short-term-load-forecasting"></a>Korte Term laden prognoses
Korte Term laden prognoses (STLF) wordt binnen de context van energie aanvraag gedefinieerd als de geaggregeerde laden die wordt een prognose in de nabije toekomst bij verschillende gedeelten van het raster (of het raster als geheel). In deze context, wordt de korte termijn gedefinieerd als periode in het bereik van 1 uur tot 24 uur. In sommige gevallen is een periode van 48 uur ook mogelijk. Daarom wordt STLF in een operationele use-case van het raster. Hier volgen enkele voorbeelden van STLF basis van hoeveelheid werk gebruik zaken:
-   Vraag en aanbod verdelen
-   Power handelen ondersteuning
-   Market aanbrengen (instelling power prijs)
-   Raster operationele optimalisatie
-   [Aanvraag antwoord](https://en.wikipedia.org/wiki/Demand_response)
-   Pieksnelheden aanvraag prognoses
-   Vraagbeheer zijde
-   Taakverdeling en preventie overbelastingen
-   Lange termijn laden prognoses
-   Probleem- en afwijking detectie
-   Piek belangrijke inperking/effenen 

STLF model voornamelijk zijn gebaseerd op het nabije verleden (laatste dag of week) verbruik van gegevens en het gebruik prognose temperatuur als een belangrijke voorspelde. Het verkrijgen van nauwkeurige temperatuur prognose voor het volgende uur en omhoog tot 24 uur wordt kleiner van een uitdaging nu dagen. Deze modellen zijn minder gevoelige seizoensgebonden patronen of langdurige verbruik trends.

SLTF oplossingen ook waarschijnlijk het genereren van grote hoeveelheden tekstvoorspelling oproepen (serviceaanvragen) zijn, omdat ze zijn wordt aangeroepen per uur worden berekend en in sommige gevallen ook niet met hogere frequentie. Het is ook zeer algemene om stadium van de innesteling waar elke afzonderlijke onderstation of transformer wordt weergegeven als een zelfstandig product model en daarom het volume van de voorspelling aanvragen zijn nog meer weer te geven.

#### <a name="long-term-load-forecasting"></a>Lange termijn laden prognoses
Het doel van lange Term laden prognoses (LTLF) is power aanvraag prognose met een periode die variëren van 1 week naar meerdere maanden (en in sommige gevallen voor een aantal jaren). In dit bereik van horizon geldt voornamelijk voor planning en investering gevallen gebruiken.

Voor lange scenario's is het belangrijk moet hoge kwaliteit-gegevens die betrekking heeft op een reeks meerdere jaren (minimale 3 jaar). Deze modellen wordt meestal periodieke variaties patronen extraheren uit de historische gegevens en maakt gebruik van externe predicators bijvoorbeeld als weer en klimaat patronen.

Het is belangrijk om aan te geven dat het langer de prognose horizon is, hoe minder nauwkeurig de prognose mogelijk. Daarom moet u enkele intervallen betrouwbaarheid samen met de werkelijke prognose waarmee mensen te houden van de mogelijke variatie in hun planningsproces produceren.

Aangezien het verbruik scenario voor LTLF is voornamelijk planning, zou veel laag tekstvoorspelling volume (in vergelijking met STLF). We deze ingesloten in een visualisatie hulpmiddel zoals Excel of PowerBI voorspellingen standaard wordt weergegeven en handmatig door de gebruiker worden geactiveerd.

### <a name="short-term-vs-long-term-prediction"></a>Korte Term versus lang Term voorspellen
De volgende tabel worden vergeleken van STLF en LTLF met betrekking tot de belangrijkste kenmerken:

|Kenmerk|Voorspellen-korte termijn laden|Lange termijn laden voorspellen|
|---|---|---|
|Voorspellen-Horizon|Van 1 uur en 48 uur|Van 1 tot en met 6 maanden of meer|
|Gegevens granulatie|Ieder uur|Per uur of dagelijks|
|Veelvoorkomend gebruik gevallen|<ul><li>Aanvraag/levering verdelen</li><li>Kies uur prognoses</li><li>Aanvraag antwoord</li></ul>|<ul><li>Lange termijn planning</li><li>Raster activa plannen</li><li>Resourceplanning</li></ul>|
|Normale variabelen|<ul><li>Dag- of weekweergave</li><li>Uur van dag</li><li>Ieder uur temperatuur</li></ul>|<ul><li>Maand van jaar</li><li>Dag van maand</li><li>Langdurig temperaturen en klimaatsomstandigheden</li></ul>|
|Historische gegevensbereik|Twee of drie jaar zijn natuurlijk van gegevens|Vijf tot en met 10 jaar zijn natuurlijk van gegevens|
|Gemiddelde nauwkeurigheid|MAPE * van 5% of lager|MAPE * van 25% of lager|
|Frequentie van voorspellen|Geproduceerd per uur of elke 24 uur|Zodra de maand, kwartaal of jaarlijks geproduceerd|
\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – Mean gemiddelde percentage Error

Zoals uit deze tabel is zichtbaar, is heel belangrijk onderscheid maken tussen de korte en de lange termijn prognoses scenario's zoals deze verschillende zakelijke behoeften vertegenwoordigen en kunnen verschillende implementatie en verbruik patronen hebben.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Gebruiksvoorbeeld van voorbeeld 1: eSmart systemen – overbelasting optimalisatie
Een belangrijke rol een [slimme raster](https://en.wikipedia.org/wiki/Smart_grid) is dynamisch en voortdurend optimaliseren en aanpassen voor de veranderende verbruik patronen. Energieverbruik kan worden beïnvloed door de korte termijn wijzigingen die voornamelijk worden veroorzaakt door temperatuur schommelingen (*bijvoorbeeld*meer vermogen wordt gebruikt voor air voorwaarde of verwarming). Tegelijkertijd, wordt ook energieverbruik beïnvloed door langdurige trends. Deze kunnen periodieke variaties effecten, nationale feestdagen langdurige verbruik groei en zelfs economic factoren zoals consumenten index, olive oil prijs en bbp bevatten.

In dit geval gebruik wilde [eSmart](http://www.esmartsystems.com/) implementeren van een cloud-oplossing waarmee voorspellingen te doen de investeringsneiging van overbelasting op een bepaald onderstation van het raster. Met name wilde eSmart onderstations die waarschijnlijk overbelastingen binnen een uur, zodat een onmiddellijke actie genomen kan worden voor voorkomen of op te lossen die situatie identificeren.

Een nauwkeurige en snel uitvoering tekstvoorspelling vereist implementatie van drie blog modellen:
-   Lang term model waarmee prognoses van energieverbruik op elke onderstation tijdens het volgende aantal weken of maanden
-   Korte termijn model waarmee de voorspelling van overbelasting op een bepaald onderstation tijdens het volgende uur
-   Temperatuur-model dat biedt prognoses van toekomstige temperatuur over meerdere scenario 's

Het doel van de lange termijn model is de onderstations rangschikken op basis van hun investeringsneiging overbelastingen (gezien hun power overdracht capaciteit) tijdens de volgende week of maand. Hiermee kunt het maken van een lijst met suggesties van onderstations die u als invoer voor de korte termijn Tekstvoorspelling gebruiken wilt. Zoals temperatuur een belangrijk voorspelde voor de lange termijn model is, is een nodig voortdurend meerdere scenario temperatuur prognoses maken en deze als invoer in voor de lange termijn model-feed. De korte termijn prognose wordt vervolgens aangeroepen om te voorspellen welke onderstation is waarschijnlijk overbelastingen via het volgende uur.

De korte als lange termijn modellen zijn afzonderlijk per elke onderstation geïmplementeerd. Daarom vereist de praktische uitvoering van deze modellen uitgebreide integratie. Als u wilt krijgen hoger tekstvoorspelling nauwkeurigheid in de korte termijn, een uitgebreider model streeft voor elk uur van de dag. Alle deze modellen per uur worden uitgevoerd en worden uitgevoerd binnen enkele minuten toe te staan dat voldoende tijd om te beantwoorden en maatregelen nemen indien nodig hebt. Deze verzameling modellen is actueel houden periodieke opnieuw opleiden met de meest recente gegevens.

Meer informatie over deze use-case vindt u [hier](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Gebruik van hoofdletters/kleine letters voor hoger onderwijs Criteria: vereisten
De belangrijkste sterkte van Cortana Intelligence is in de krachtige mogelijkheid om te implementeren en machine learning centraal oplossingen schaal. Dit is bedoeld om ondersteuning duizendtallen van voorspellingen die gelijktijdig worden uitgevoerd. Dit kan automatisch schalen om te voldoen aan een veranderende verbruik patroon. De focus van een oplossing dus op de nauwkeurigheid en computerprestaties. Een bedrijf hulpprogramma is bijvoorbeeld geïnteresseerd nauwkeurige energie aanvraag voor het volgende uur en voor elk uur van de dag prognose produceren. Aan de andere kant, zijn we minder geïnteresseerd bij het beantwoorden van de vraag van de aanvraag is waarom voorspeld als deze is (het model zelf worden gedaan die).

Daarom Houd er rekening mee dat niet alle gevallen gebruiken en zakelijke problemen effectief kunnen worden opgelost met machine learning.

Het is mogelijk dat Cortana Intelligence en machine learning uiterst effectieve bij het oplossen van een bepaald bedrijfsproces probleem wanneer de volgende criteria wordt voldaan:
-   Het probleem dat bedrijven in hand is **blog** van aard. Een blog gebruik hoofdletters/kleine letters voorbeeld is een hulpprogramma-bedrijf die wilt voorspellen power belasting op een bepaald onderstation tijdens het volgende uur. Aan de andere kant, zou analyseren en classificeren stuurprogramma's van historische aanvraag **beschrijvende** in aard en kunnen daarom minder van toepassing.
-   Er is een wissen **pad van de actie** te nemen zodra de voorspelling beschikbaar is. Bijvoorbeeld kan voorspellingen te doen een overbelasting op een onderstation tijdens het volgende uur resulteren in een proactief actie van afgetrokken laden die is gekoppeld aan die onderstation en zo mogelijk te voorkomen dat een overbelasting.
-   De use-case vertegenwoordigt een **typisch type probleem** zoals deze kan worden opgelost die wanneer kunt bereid u gebruiken voor het oplossen van andere soortgelijke gevallen.
-   De klant kunt instellen dat **kwantitatieve en kwalitatief doelstellingen** om te laten zien een goede oplossing-implementatie. Een goede kwantitatieve doel voor de functie energie aanvraag voorspellen zou bijvoorbeeld de drempelwaarde voor vereiste nauwkeurigheid (*bijvoorbeeld*maximaal 5% fout is toegestaan) of wanneer voorspellingen te doen onderstation overbelastingen vervolgens de precisie (snelheid waar positieve) en intrekken (mate van waar positieve) moet boven een bepaald bedrag. Deze doelstellingen moeten afkomstig zijn van de doelen van de klant.
-   Er is een wissen **integratiescenario voor** met business-werkstroom van het bedrijf. De onderstation laden prognose kan bijvoorbeeld worden geïntegreerd in het midden van het besturingselement raster toe te staan dat overbelasting preventie activiteiten.
-   De klant heeft gereed voor gebruik van **gegevens met voldoende kwaliteit** ter ondersteuning van de use-case (Zie meer in de volgende sectie, **Kwaliteit van de gegevens**van deze playbook).
-   De klant bestrijkt cloud centraal gegevens architectuur of **cloudgebaseerde machine learning**, inclusief Azure ML en andere onderdelen Cortana Intelligence-Suite.
-   De klant is bereid tot stand brengen van **een complete gegevensstroom** die faciliteiten de bezorging van gegevens in de cloud doorlopend, en de oplossing is bereid naar **mogelijk maken** .
-   De klant is klaar voor het **reserveren van resources** die wordt actief bezig zijn tijdens het eerste uitproberen gebruik zodat kennis en eigendom van de oplossing kunnen worden verplaatst naar de klant na het succesvolle afronding.
-   De resource klant moet een **ervaren gegevens professionele**, bij voorkeur een wetenschappelijk gegevens.

Voor hoger onderwijs van een use-case op basis van de bovenstaande criteria kunt enorm verbeteren de tarieven slagen van een use-case en stand brengen van een goede beachhead voor de uitvoering van zaken voor toekomstig gebruik.

### <a name="cloud-based-solutions"></a>Cloud-oplossingen
Cortana Intelligence Suite op Azure is een geïntegreerde omgeving die zich in de cloud bevindt. De implementatie van een oplossing van geavanceerde analyses in een omgeving cloud bevat aanzienlijke voordelen voor bedrijven en op hetzelfde moment groot wijzigen voor bedrijven die nog steeds on-premises IT-oplossingen gebruiken kunnen betekenen. Binnen de energiesector is er een duidelijke trend van geleidelijke migratie van bewerkingen in de cloud. Deze trend gaat gepaard samen met de ontwikkeling van het raster slimme zoals hierboven, wordt in **Industry Trends**beschreven. Als deze playbook is gericht op een cloud-oplossing in het domein energie, is het is belangrijk uitleg over de voordelen en andere relevante informatie van een cloudgebaseerde-oplossing implementeert.

Het grootste voordeel van een cloud-oplossing is misschien de kosten. Als een oplossing maakt gebruik van cloud geïmplementeerd onderdelen, is er opstartkosten of de kosten van omzet (kosten van verkochte goederen) onderdeel die zijn gekoppeld. Dat betekent dat hoeft te investeren in hardware, software en IT-onderhoud, en daarom een aanzienlijke wilt verkleinen in business risico bestaat.

Een ander belangrijk voordeel is de kostenstructuur pay-as-you-go van cloud gebaseerde oplossingen. Cloudgebaseerde servers voor computing of opslag kunnen worden geïmplementeerd en schaal op basis van just-als-nodig. Hiermee wordt het voordeel van de efficiency kosten van een cloud-oplossing.

Er is ten slotte hoeft te investeren in IT onderhoud of toekomstige infrastructuurontwikkeling als dit alles deel uit van de cloudgebaseerde aanbod maakt. Dat opzicht Cortana Intelligence Suite het beste in class services bevat en dat het stappenplan blijft ontwikkeling. Nieuwe functies, onderdelen en functies worden voortdurend worden geïntroduceerd en ontwikkelen.

Naar een bedrijf dat alleen de overgang in de cloud wordt gestart, zijn we ten zeerste geadviseerd om een geleidelijke aanpak door het implementeren van het stappenplan van een cloud-migratie. Wij vinden dat de gevallen gebruiken die worden besproken in deze playbook voor hulpprogramma's en bedrijven in het domein energie, een uitstekende gelegenheid voor de blog analytics-oplossingen in de cloud haalbaarheidsonderzoek vertegenwoordigen.

#### <a name="business-case-justification-considerations"></a>Zakelijke hoofdletters/kleine letters uitvulling overwegingen
In veel gevallen mogelijk de klant in geïnteresseerd om een zakelijke reden voor een bepaald use-case waarin een oplossing op basis van cloud en Machine Learning belangrijke onderdelen zijn. In tegenstelling tot een oplossing on-premises, klikt u in het geval van een oplossing cloudgebaseerde, het onderdeel tevoren alles kosten minimale is en de meeste van de kostenelementen zijn gekoppeld aan de werkelijke gebruik. Wanneer u gaat naar een oplossing op Cortana Intelligence Suite prognoses energie implementeert, kunnen meerdere services worden geïntegreerd met een enkele veelvoorkomende kostenstructuur. Bijvoorbeeld databases (*bijvoorbeeld*SQL Azure) kunnen worden gebruikt om de opslag van de onbewerkte gegevens en klik vervolgens Azure ML voor de werkelijke prognoses wordt gebruikt voor het hosten van de prognose services. In dit voorbeeld, kan de kostenstructuur van de opslag en transactiecomponenten opnemen.

Aan de andere kant, moet een een goed inzicht in de business-waarde van het besturingssysteem van een energie-aanvraag prognoses (korte of lange termijn) hebben. Ja, is het belangrijk voor de waarde van de bedrijven van elke voorspelde bewerking realiseren. Zo nauwkeurig power laden prognoses de komende 24 uur kan voorkomen dat overproduction of overbelastingen in het raster kunt voorkomen en dit kan op de stellen streefcijfers voor kortingen op dagelijks.

Een eenvoudige formule voor het berekenen van de financiële voordelen van de vraag voorspellen-oplossing: ![eenvoudige formule voor het berekenen van de financiële voordelen van de vraag voorspellen-oplossing](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Aangezien Cortana Intelligence Suite een pay-as-you-go prijzen model biedt, moet u er niet nodig voor een vaste kosten-onderdeel aan deze formule actief is. Deze formule kan worden berekend op dagelijks, maandelijks of jaarlijks basis.

Huidige Cortana Intelligence-Suite en Azure ML abonnementen prijzen vindt u [hier](http://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Oplossingsproces ontwikkeling
De ontwikkelingscyclus van een energie-aanvraag prognoses oplossing betekent meestal dat 4 fasen, die we maken gebruik van de cloudgebaseerde technologieën en services binnen de Cortana Intelligence-Suite.

Dit is geïllustreerd in het volgende diagram:

![Slim raster cyclus](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

De volgende alinea worden beschreven in dit proces stap 4:

1.  **Gegevens verzamelen** – eventuele geavanceerde analyses op basis-oplossing is gebaseerd op gegevens (Zie **Lidmaatschap van de gegevens**). Met name als het gaat om Bekijk analyses en prognoses, we zijn afhankelijk van continue, dynamische stroom van gegevens. Bij het voorspellen van energie de vraag, deze gegevens kan worden opgehaald rechtstreeks slimme meter of al op een on-premises-database worden samengevoegd. We zijn ook gebaseerd op andere externe gegevensbronnen zoals weer en temperatuur. Deze lopende stroom van gegevens moet worden ingedeeld, gepland en die zijn opgeslagen. [Azure gegevens Factory](http://azure.microsoft.com/services/data-factory/) (ADF) is onze belangrijkste werkpaard voor het uitvoeren van deze taak.
2.  **Modeling** – voor nauwkeurige en betrouwbare energie prognoses een moet (trein) ontwikkelen en onderhouden van een geweldige model dat maakt gebruik van de historische gegevens en de betekenis te geven en bekijk patronen in de gegevens ophaalt. Het gebied van Machine Learning (ML) heeft groeien snel met meer geavanceerde algoritmen regelmatig worden ontwikkeld. Azure ML Studio biedt gebruikerservaring waarmee gebruikmaken van de meest geavanceerde ML algoritmen binnen een werkstroom voltooid. Deze werkstroom wordt geïllustreerd in een intuïtieve gegevensstroom-diagram en bevat de gegevensvoorbereiding van, de extractie van de functie, modellering en model evaluatie. De gebruiker kunt ophalen in honderden verschillende modellen die zijn opgenomen in deze omgeving. Aan het einde van deze fase hebt een wetenschappelijk gegevens een werkmodel dat is volledig geëvalueerd en klaar voor implementatie.

    In het volgende diagram wordt een voorbeeld van een doorsnee werkstroom:

    ![Modellering van een werkstroom](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)

3.  **Implementatie** – met een werkmodel in een hand om aan de volgende stap is implementatie. Hier wordt het model geconverteerd naar een webservice die een RESTful API die tegelijk kunnen worden aangeroepen via Internet vanuit verschillende verbruik clients beschrijft. Azure ML biedt een eenvoudige manier om een model rechtstreeks vanuit de Studio Azure ML met één klik van een knop implementeren. Het implementatieproces van het hele gebeurt geavanceerde instellingen weergeven. Deze oplossing kan automatisch schalen om te voldoen aan de vereiste verbruik.

4.  **Verbruik** – In deze fase we wilt doorvoeren gebruiken van de prognose model zodat het eindresultaat voorspellingen. Het verbruik van een Gebruikerstoepassing voor de (*bijvoorbeeld*, dashboard) kunnen worden bepaald of rechtstreeks vanuit een operationele systeem zoals aanvraag/levering verdelen systeem of een raster optimalisatie-oplossing. Meerdere gebruik zaken kunnen worden bepaald van één model.

## <a name="data-understanding"></a>Lidmaatschap van gegevens
Na die betrekking hebben op de zakelijke overwegingen (Zie **Bedrijven lidmaatschap**) van een oplossing prognoses energie-aanvraag, gaan we nu het gegevensonderdeel te bespreken. Een blog analytics-oplossing is afhankelijk van betrouwbare gegevens. Voor energie aanvraag prognoses, afhankelijk we historische verbruik van gegevens met verschillende niveaus. Die historische gegevens wordt gebruikt als het materiaal dat onbewerkte. Deze worden onderworpen aan een zorgvuldige analyse waarin het wetenschappelijk gegevens variabelen geeft (ook functies genoemd) die in een model bestaat uit de vereiste prognoses uiteindelijk kunnen worden geplaatst.

In de rest van deze sectie, worden de verschillende stappen en overwegingen voor informatie over de gegevens en hoe u zet het in bruikbaar worden beschreven.

### <a name="the-model-development-cycle"></a>De ontwikkelingscyclus Model
Produceert goede prognoses modellen is vereist dat u daarbij voorbereiding en planning. Het proces modellering in meerdere stappen splitsen en concentreren op één stap op een moment kunnen het resultaat van het hele proces aanzienlijk te verbeteren.

In het volgende diagram ziet u hoe het modellering-proces kan worden opgedeeld in verschillende stappen:

![Ontwikkelingscyclus model](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Zoals u kunt zien dat de cyclus bestaat uit zes stappen:
-   Probleem formulering
-   Opname van de gegevens en verkennen
-   Gegevensvoorbereiding van en functie engineering
-   Modeling
-   Model evaluatie
-   Ontwikkeling

In de rest van deze sectie wordt de afzonderlijke stappen en rekening mee moet houden bij elke stap worden beschreven.

### <a name="problem-formulation"></a>Probleem formulering
We kunnen Houd rekening met de formulering probleem als de belangrijkste stap één duren moet voordat de implementatie van een blog analytics-oplossing. Hier wordt het probleem bedrijven zou transformeren en deze aan specifieke elementen die kunnen worden opgelost door middel van gegevens technieken ontleden. Het is een goede gewoonte aan het opstellen van het probleem als een set vragen die we wilt beantwoorden. Hier volgen enkele mogelijke vragen die van toepassing binnen het bereik van energie aanvraag prognoses zijn mogelijk:
-   Wat is de verwachte belasting op een afzonderlijke onderstation in de volgende uur of dag?
-   Op welk tijdstip van de dag ervaart mijn raster piek aanvraag?
-   Wat is de kans mijn raster om op te vangen van de verwachte belasting?
-   Hoeveel power moet het station power genereren tijdens elke uur van de dag?

Het opstellen van deze vragen, kunnen we kunt richten op de juiste gegevens zijn opgehaald en implementeren van een oplossing die volledig wordt uitgelijnd op het probleem bedrijven waaraan u bezig bent. Bovendien kunt we enkele belangrijke doelstellingen waarmee wij de prestaties van het model wordt berekend vervolgens instellen. Bijvoorbeeld hoe nauwkeurig de prognose moet en wat is het bereik van de fout die nog steeds aanvaardbaar door het bedrijf zijn?

### <a name="data-sources"></a>Gegevensbronnen
Het moderne slimme raster verzamelt gegevens uit verschillende onderdelen en de onderdelen van het raster. Deze gegevens staan verschillende aspecten van de bewerking en het gebruik van het raster power. Binnen het bereik van de energie vraag prognose, zijn wij de discussie op gegevensbronnen die overeenkomen met de werkelijke vraag verbruik beperken. Een belangrijke bron van het energieverbruik zijn slimme meter. Hulpprogramma's over de hele wereld implementeert snel slimme meter voor hun consumenten. Slim meter opnemen van de werkelijke energieverbruik en door te sturen voortdurend deze gegevens terug naar het hulpprogramma bedrijf. Gegevens die worden verzameld en verzonden weer met een vast interval, die variëren van elke 5 minuten op 1 uur staan. Meer geavanceerde slimme meter kunnen extern programmeren om te beheren en saldo vanaf het werkelijke verbruik binnen een huishouden. Slim meter gegevens relatief betrouwbaar is en bevat een tijdstempel. Dat is het belangrijk ingrediënt voor vraag prognose. Meter gegevens kunnen worden samengevoegd (opgetelde omhoog) op verschillende niveaus binnen de topologie raster: transformer, onderstation, regio, *enzovoort*. We kunnen kiest u het aggregatieniveau van de vereiste maken van een prognose model voor deze. Bijvoorbeeld als het hulpprogramma bedrijf wilt prognose toekomstige belasting op elk van de onderstations raster kunnen vervolgens alle meter gegevens worden samengevoegd voor elke afzonderlijke onderstation en gebruikt als invoer voor de prognose model. We wordt slimme meter wel een interne gegevensbron.

Een betrouwbare energie aanvraagprognose wordt ook zijn afhankelijk van andere externe gegevensbronnen. Eén belangrijke factor die van invloed is op energieverbruik is de weer of nog nauwkeuriger de temperatuur. Historische gegevens ziet sterke relatie tussen buiten temperatuur en energieverbruik. Tijdens warm zomer dagen, consument te maken van hun airconditioning en tijdens de kracht winter op verwarming gebruiken. Een betrouwbare bron van historische temperatuur op de rasterlocatie is dus sleutel. Bovendien we ook zijn afhankelijk van nauwkeurige voorspelling van temperatuur energieverbruik te voorspellen.

Andere externe gegevensbronnen kunt ook energie aanvraag voorspellen modellen bouwen. Hierbij langdurig klimaat wijzigingen, min kosten indexen (*bijvoorbeeld*bbp) en anderen. In dit document nemen niet we deze andere gegevensbronnen.

### <a name="data-structure"></a>Gegevensstructuur
Nadat u de vereiste gegevensbronnen, graag willen we om ervoor te zorgen dat onbewerkte gegevens die zijn verzameld de onderdelen van de juiste gegevens bevat. Maak een betrouwbare aanvraag voorspellen model door zou doen we nodig om ervoor te zorgen dat de gegevens die worden verzameld gegevenselementen waarmee u kunnen de toekomstige vraag voorspellen bevat. Hier volgen enkele eenvoudige vereisten betreffende de gegevensstructuur (schema) van de onbewerkte gegevens.

De onbewerkte gegevens bestaat uit rijen en kolommen. Elke meting wordt weergegeven als één rij met gegevens. Elke rij met gegevens bevat voor meerdere kolommen (ook wel genoemd functies of de velden).

1.  **Tijdstempel** : het tijdstempelveld geeft de werkelijke tijd waarop de meting is geregistreerd. Deze moet voldoen aan een van de algemene indelingen voor datum/tijd. Datum- en tijdfunctie onderdelen moeten worden opgenomen. In de meeste gevallen moet u er niet nodig voor de tijd moet worden vastgelegd tot het tweede niveau specifieker is. Het is belangrijk om op te geven van de tijdzone waarin de gegevens is opgenomen.
2.  **Meter-ID** - dit veld geeft de meter of het apparaat maateenheden. Dit is een bestaan variabele en een combinatie van cijfers en andere tekens.
3.  **Verbruik waarde** : dit is het werkelijke verbruik bij een bepaalde datum/tijd. Het verbruik kan worden gemeten in kWh (kilowatt-hour) of enig ander voorkeur eenheden. Het is belangrijk te weten dat de maateenheid consistent over alle metingen in de gegevens blijven moet. In sommige gevallen kan verbruik van meer dan 3 power fasen worden opgegeven. In dat geval zou doen we nodig voor het verzamelen van alle onafhankelijke verbruik fasen.
4.  **Temperatuur** – de temperatuur worden meestal verzameld uit een onafhankelijke bron. Dit moet wel compatibel is met het gebruik van gegevens. Het moet een tijdstempel zoals hierboven is beschreven, zodat deze worden gesynchroniseerd met de werkelijke verbruiksgegevens bevatten. De waarde temperatuur kan worden opgegeven in graden Celsius of Fahrenheit, maar consistent over alle metingen moet blijven.
5.  **Plaats, namelijk** Het locatieveld is meestal gekoppeld aan de plaats waar de temperatuurgegevens zijn verzameld. Dit kan worden weergegeven als een getal zip-code of in breedtegraad/lengtegraad (lat/lang)-indeling.

De volgende tabellen ziet u voorbeelden van een goede verbruik en temperatuur gegevensindeling:

|**Datum**|**Tijd**|**Meter-ID**|**Fase 1**|**Fase 2**|**Fase 3**|
|--------|--------|------------|-----------|-----------|-----------|
|1-7-2015|10:00:00|ABC1234     |7.0        |2.1        |5.3        |
|1-7-2015|10:00:01|ABC1234     |7.1        |2.2        |4.3        |
|1-7-2015|10:00:02|ABC1234     |6.0        |2.1        |4.0        |

|**Datum**|**Tijd**|**Locatie**|**Temperatuur**|
|--------|--------|-------------|---------------|
|1-7-2015|10:00:00|11242        |24,4           |
|1-7-2015|10:00:01|11242        |24,4           |
|1-7-2015|10:00:02|11242        |24,5           |

Zoals hierboven is zichtbaar, wordt in dit voorbeeld 3 verschillende waarden voor verbruik die is gekoppeld aan de 3 fasen van power bevat. Houd er ook rekening mee dat de velden voor datum en tijd worden gescheiden, maar ze kunnen ook worden gecombineerd in één kolom. In dit geval wordt de kolom locatie weergegeven in een indeling met 5 cijfers zip-code en de temperatuur in een indeling graden Celsius.

### <a name="data-format"></a>Gegevens opmaken
Cortana Intelligence Suite kunt ondersteuning voor de meest voorkomende gegevensindelingen zoals CSV-, TSV, JSON, *enzovoort*. Het is belangrijk dat de gegevensindeling consistente voor de volledige levenscyclus van het project blijft.

### <a name="data-ingestion"></a>Opname van gegevens
Aangezien energie aanvraagprognose wordt voortdurend en vaak voorspeld, moeten we ervoor zorgen dat de onbewerkte gegevens met behulp van een effen kleur en een betrouwbare gegevens opname proces is die doorloopt. Het proces opname moet waarborgen dat de onbewerkte gegevens beschikbaar voor de prognose proces het vereiste tegelijk is. Dat betekent dat de frequentie van de opname gegevens groter dan de prognose frequentie zijn moet.

Bijvoorbeeld: als de onze aanvraag prognoses oplossing gegenereerd door een nieuwe prognose om 8:00 AM dagelijks en we nodig om ervoor te zorgen dat alles de gegevens die tijdens de afgelopen 24 uur zijn verzameld heeft volledig tot dat moment is ingenomen en heeft tot even het laatste uur van de gegevens bevatten.

Hiervoor biedt Cortana Intelligence Suite verschillende manieren ter ondersteuning van een betrouwbare gegevens opname proces. Hiermee wordt verder worden besproken in de sectie **implementatie** van dit document.

### <a name="data-quality"></a>Kwaliteit van de gegevens
De onbewerkte gegevensbron die is vereist voor de uitvoering van betrouwbare en nauwkeurige aanvraag prognoses moet voldoen aan bepaalde kwaliteitscriteria basisgegevens. Geavanceerde statistische methoden kunnen worden gebruikt voor enkele mogelijke gegevens kwaliteit probleem ter, moeten we nog steeds Zorg ervoor dat we enkele drempelwaarde voor basisgegevens kwaliteit kruist wanneer ingesting nieuwe gegevens. Hier volgen een paar aandachtspunten voor de betreffende onbewerkte gegevenskwaliteit:
-   **Ontbrekende waarde** – dit heeft betrekking op de situatie wanneer bepaalde meting niet is verzameld. De eenvoudige vereiste hier is dat het tarief weer dat ontbrekende waarde niet groter dan 10% voor een bepaalde periode zijn moet. In geval dat op één waarde deze ontbreekt moet worden aangegeven met een vooraf gedefinieerde waarde (bijvoorbeeld: '9999') en niet '0' die een geldige maat kan zijn.
-   **Maateenheden nauwkeurigheid** – de werkelijke waarde van verbruik of temperatuur moet nauwkeurig worden geregistreerd. Onjuiste afmetingen zullen produceren onjuiste prognoses. De fout maateenheden moet meestal kleiner dan 1% ten opzichte van de werkelijke waarde.
-   **Tijdstip van de meting** : dit is vereist dat de werkelijke tijdstempel van de gegevens die worden verzameld wordt niet meer dan 10 seconden ten opzichte van de waar tijd van de werkelijke meting afwijken.
-   **Synchronisatie** – wanneer u meerdere gegevensbronnen die worden gebruikt (*bijvoorbeeld*, verbruik en temperatuur) we moet ervoor zorgen dat er geen tijdsynchronisatie ertussen problemen zijn. Dit betekent dat het tijdverschil tussen de verzamelde tijdstempel van twee onafhankelijke gegevensbronnen niet meer dan meer dan 10 seconden.
-   **Latentie** - zoals hierboven, wordt beschreven in de **Opname van de gegevens**, we zijn afhankelijk van een betrouwbare gegevens-mailstroom en opname proces. Naar het besturingselement dat we ervoor zorgen moet dat we de latentie gegevens bepalen. Hiermee wordt opgegeven als het tijdverschil tussen het moment dat de werkelijke meting heeft plaatsgevonden en het tijdstip waarop deze is geladen in de opslag Cortana Intelligence-Suite en gereed is voor gebruik. Voor de korte termijn laden prognoses van de totale latentie mag niet langer dan 30 minuten. Voor het laden van de lange termijn prognoses van de totale latentie mag geen groter is dan 1 dag.

### <a name="data-preparation-and-feature-engineering"></a>Gegevensvoorbereiding van en functie Engineering
Zodra de onbewerkte gegevens is geconsumeerde (Zie **Gegevens opname**) en veilig is opgeslagen, is klaar om te worden verwerkt. De gegevens voorbereidende fase is in principe duurt de onbewerkte gegevens en converteren (transformeert, vorm hiervan te wijzigen) deze in een formulier voor de fase modellering. Die eenvoudige bewerkingen uitvoeren zoals het gebruik van de kolom onbewerkte gegevens met de werkelijke gemeten waarde, gestandaardiseerde waarden, meer complexe bewerkingen zoals [tijd vertraging](https://en.wikipedia.org/wiki/Lag_operator)en anderen kunnen bevatten. De zojuist gemaakte gegevenskolommen worden genoemd, functies voor gegevens en het proces van het genereren van deze wordt functie engineering genoemd. Aan het einde van dit proces moet we een nieuwe gegevensgroep die heeft zijn afgeleid van de onbewerkte gegevens en kan worden gebruikt voor modellering. Daarnaast moet verrichten ontbrekende waarden (Zie de **Kwaliteit van de gegevens**) en dit voor hen de gegevens voorbereidende fase. In sommige gevallen, zou ook moeten we de gegevens om ervoor te zorgen dat alle waarden worden aangegeven in dezelfde schaal normaliseren.

In deze sectie die wordt een overzicht van de algemene gegevensfuncties die van de energie uitmaken deel prognose modellen van de vraag.

**Tijd basis van hoeveelheid werk functies:** Deze functies zijn afgeleid van de gegevens voor datum/tijdstempel. Deze uitgepakt en dat is geconverteerd naar bestaan functies beschikbaar, zoals:
-   Tijd van de dag: dit is dat het uur van de dag waarmee de waarden van 0 tot 23
-   Dag van week – Hiermee de dag van de week aangeeft en waarden van 1 (zondag) tot en met 7 (zaterdag)
-   Dag van maand – Hiermee de werkelijke datum aangeeft en waarden van 1 tot en met 31 kunt uitvoeren
-   Maand van jaar – Hiermee vertegenwoordigt van de maand en waarden van 1 (januari) tot 12 (December)
-   Weekend: dit is een functie binaire waarde die de waarden van 0 voor weekdagen of 1 voordat weekend duurt
-   Vakantie - dit is een functie binaire waarde die de waarden van 0 voor een normale dag of 1 voordat een feestdag duurt
-   Fourier-termen – de Fourier-voorwaarden zijn lijndikten die zijn afgeleid van de tijdstempel en worden gebruikt voor het vastleggen van de periodieke variaties (maal) in de gegevens. Aangezien mogelijk zijn er meerdere seizoenen in ons hebben we mogelijk meerdere Fourier termen. Waarden van de aanvraag kunnen bijvoorbeeld jaarkalender-, week- en dagpagina seizoenen/maal waardoor 3 Fourier termen.

**Onafhankelijke maateenheden functies:** De onafhankelijke functies bevatten alle gegevenselementen die willen we gebruiken als variabelen in onze model. Hier wordt de afhankelijke functie waarmee zou moeten we voorspellen uitsluiten.
-   Vertragings-functie – hierna ziet u tijd logische verschuiving naar waarden van de werkelijke vraag. De waarde van de aanvraag wordt bijvoorbeeld, houdt u vertragings 1-functies in het vorige uur (ervan uitgaande dat er per uur) ten opzichte van de huidige tijdstempel. We kunt op dezelfde manier toevoegen vertragings 2, 3, vertraging *enzovoort*. De werkelijke combinatie van vertragings-functies die worden gebruikt worden tijdens de fase modellering bepaald door de evaluatie van de resultaten van het model.
-   Lange termijn trending: deze functie de lineaire groei van de vraag tussen jaar vertegenwoordigt.

**Afhankelijke functie:** De afhankelijke functie is de kolom met gegevens die we onze model te voorspellen wilt. Met [gecontroleerde machine learning](https://en.wikipedia.org/wiki/Supervised_learning)moeten we eerst trainen het model met de afhankelijke-functies (die wordt ook wel als labels). Hiermee kunt het model voor meer informatie over de patronen in de gegevens die zijn gekoppeld aan de afhankelijke functie. In energie vraag prognose meestal willen we de werkelijke vraag voorspellen en kunnen daarom we dit wilt gebruiken als de afhankelijke functie.

**Voor de verwerking van ontbrekende waarden:** Tijdens de voorbereidende fase van gegevens moeten we de beste strategie u omgaat met ontbrekende waarden bepalen. Dit gebeurt voornamelijk met behulp van de verschillende statistische [gegevens begrip methoden](https://en.wikipedia.org/wiki/Imputation_(statistics)). Bij het voorspellen van energie de vraag rekenen we meestal ontbrekende waarden met behulp van zwevend gemiddelde van de vorige beschikbare gegevenspunten.

**Gegevens normaliseren:** Gegevens normaliseren is een ander type transformatie die wordt gebruikt om te zorgen dat alle numerieke gegevens zoals vraag prognose brengen naar een soortgelijke schaal. Dit meestal verbetert de nauwkeurigheid van model en precisie van formules. We doorgaans doen door de werkelijke waarde delen door het bereik van de gegevens.
Hiermee wordt de oorspronkelijke waarde naar beneden de schaal in een kleinere bereik, meestal tussen 1 en 1.

## <a name="modeling"></a>Modeling
De fase modellering is waar de conversie van de gegevens in een model plaatsvindt. In een belangrijk onderdeel van dit proces er zijn geavanceerde algoritmen die in de historische gegevens (trainingsgegevens) scannen, patronen ophalen en bouwen van een model. Dat model kan later worden gebruikt om te voorspellen op nieuwe gegevens die niet is gebruikt voor het maken van het model.

Zodra er een betrouwbare werkmodel kunt we deze vervolgens gebruiken voor het verkrijgen van nieuwe gegevens die is gestructureerd de vereiste voorzieningen (X). Het proces score maakt gebruik van de permanente model (object uit de fase training) en de doel-variabele die wordt aangeduid met Ŷ voorspellen.

### <a name="demand-forecasting-modeling-techniques"></a>Aanvraag prognoses Modeling technieken
In het geval van de aanvraag prognoses we geven van historische gegevens die is gesorteerd op tijd gebruiken. We verwijzen doorgaans naar gegevens met de tijddimensie als [tijdreeks](https://en.wikipedia.org/wiki/Time_series). Het doel in tijd reeks modellering is om te zoeken naar gerelateerde trends, periodieke variaties, automatisch-correlatie (correlatie na verloop van tijd), tijd en opstellen die in een model.

In de afgelopen jaren zijn geavanceerde algoritmen ontwikkeld voor tijdreeks en voor het verbeteren van de nauwkeurigheid van de prognose. Wordt deze hier enkele kort besproken.

> [AZURE.NOTE] In dit gedeelte is niet bedoeld moet worden gebruikt als een machine leren en voorspellen overzicht, maar liever als een korte enquête van modeling technieken die vaak worden gebruikt voor het voorspellen van de vraag. Voor meer informatie en educatieve materiaal over tijdreeks, wordt ten zeerste aangeraden het online adresboek [prognose: beginselen en oefening](https://www.otexts.org/book/fpp).

#### <a name="ma-moving-averagehttpswwwotextsorgfpp62"></a>[**MA (zwevend gemiddelde)**](https://www.otexts.org/fpp/6/2)
Zwevend gemiddelde is een van de eerste analytical technieken die is gebruikt voor tijdreeks en nog steeds een van de meest gebruikte technieken vanaf nu. Het is ook de basis voor meer geavanceerde technieken prognoses. Met zwevend gemiddelde zijn we het volgende gegevenspunt prognoses gemiddelde over de meest recente punten K, waarbij K wordt bepaald door de volgorde van het zwevend gemiddelde.

Het zwevend gemiddelde methode heeft het effect van de prognose demping en kunnen daarom niet kunnen ook grote volatiliteit in de gegevens.

#### <a name="ets-exponential-smoothinghttpswwwotextsorgfpp75"></a>[**ETS (exponentiële demping)**](https://www.otexts.org/fpp/7/5)
(Exponential Triple Smoothing) is een reeks verschillende methoden die gewogen gemiddelde recente gegevenspunten gebruiken om te voorspellen van de volgende gegevenspunt. Het idee is hoger lijndikten toewijzen aan meer recente waarden en dit gewicht voor oudere meetwaarden geleidelijk verkleinen. Er zijn een aantal verschillende manieren met deze reeks, een paar hiervan afhandeling van periodieke variaties opnemen in de gegevens zoals [Holt-Winters seizoensgebonden methode](https://www.otexts.org/fpp/7/5).

Enkele van de volgende manieren factor ook in de periodieke variaties van de gegevens.

#### <a name="arima-auto-regression-integrated-moving-averagehttpswwwotextsorgfpp8"></a>[**ARIMA (automatisch regressie geïntegreerd met zwevend gemiddelde)**](https://www.otexts.org/fpp/8)
Automatisch regressie geïntegreerde zwevend gemiddelde (ARIMA) is een andere reeks methoden die wordt gebruikt voor prognoses tijdreeks. Het automatisch-regressie methoden voor het vrijwel combineert met zwevend gemiddelde. Automatisch-regressie methoden voor het gebruik van regressie-modellen door middel van de vorige reeks tijdwaarden om te berekenen van de volgende datum komma. ARIMA methoden differentiërende methoden die het verschil tussen de gegevenspunten berekenen en gebruiken die in plaats van de oorspronkelijke gemeten waarde bevatten ook van toepassing. Tot slot ARIMA ook maakt gebruik van de zwevend gemiddelde technieken die hierboven worden besproken. De combinatie van alle van de volgende manieren op verschillende manieren is wat het gezin van ARIMA methoden wordt gemaakt.

ETS en ARIMA worden worden veel vandaag gebruikt voor energie aanvraag prognoses te stellen en vele andere prognose problemen. In veel gevallen deze worden gecombineerd gecombineerd om zeer nauwkeurige resultaten.

**Algemeen veelvoud regressie** Regressie-modellen mogelijk de belangrijkste modellering benadering binnen het domein van machine learning en statistieken worden aangepast. In de context van tijdreeks we regressie gebruiken om de toekomstige waarden (*bijvoorbeeld*van de aanvraag) te voorspellen. In regressie we uitvoeren van een lineaire combinatie van de variabelen en informatie over het gewicht (ook wel coëfficiënten genoemd) van deze variabelen tijdens het training. Het doel is om te produceren, een regressielijn die onze voorspelde waarde wordt verwacht. Regressie methoden zijn geschikt wanneer de doel-variabele numerieke is en daarom ook tijdreeks past. Er is een breed scala van regressie methoden inclusief zeer eenvoudige regressie-modellen zoals [Lineaire regressie](https://en.wikipedia.org/wiki/Linear_regression) en geavanceerdere bestanden zoals beslissingsbomen, [Willekeurig Forests](https://en.wikipedia.org/wiki/Random_forest) [Neural netwerken](https://en.wikipedia.org/wiki/Artificial_neural_network)en besluit bomen verhoogd.

Het bouwen van energie aanvraag prognoses als een probleem regressie ontstaat een groot aantal flexibiliteit bij het selecteren van de functies van onze gegevens die kunnen worden gecombineerd van de werkelijke vraag tijd reeksgegevens en externe factoren zoals temperatuur. Meer informatie over de geselecteerde onderdelen worden besproken in de functie Engineering (Zie **voorbereiding van gegevens en functie technische**) gedeelte van dit playbook.

Uit ervaring met implementatie en implementatie van energie aanvraag prognoses pilot hebt gevonden dat de geavanceerde regressie-modellen die beschikbaar in Azure ML zijn meestal de beste resultaten wilt produceren en we maken gebruiken deze.

## <a name="model-evaluation"></a>Model evaluatie
Model evaluatie heeft een belangrijke rol binnen de **Ontwikkelingscyclus Model**. In deze stap kijken we naar het model en de prestaties met werkelijke gegevens valideren. Tijdens de stap modellering we een deel van de beschikbare gegevens gebruiken voor het model training. Tijdens de evaluatiefase duren we de rest van de gegevens om te testen van het model. Vrijwel betekent dit dat we de nieuwe modelgegevens dat heeft zijn geherstructureerd en bevat dezelfde functies als de gegevensset training zijn invoeropties. Echter tijdens de validatie gebruiken we het model de doel-variabele voorspellen in plaats van de variabele beschikbaar doel bieden. We raadpleegt vaak u dit proces als het model scoren. Vervolgens zou doen we gebruiken de waar doelwaarden en vergelijken met de voorspelde kleuren die. Het doel hier is om te meten en de fout tekstvoorspelling, wat betekent het verschil tussen de voorspellingen en de werkelijke waarde minimaliseren. De fout meting kwantificeren is heel belangrijk aangezien we willen het model verfijnen en valideren of de fout daadwerkelijk afneemt. Instellen van het model kan worden uitgevoerd door het wijzigen van Modelparameters waarmee u kunt het proces onderwijs bepalen of te toevoegen of verwijderen van functies voor gegevens (genoemd [parameters opschonen](https://channel9.msdn.com/Blogs/Windows-Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Dit betekent vrijwel mogelijk moet tussen de functie engineering, modeling, bladeren en modelleren van evaluatie fasen meerdere keren totdat we kunnen de fout naar de vereiste niveau verlagen.

Het is belangrijk dat de fout tekstvoorspelling nooit zal nul als nadruk is nooit een model dat elke resultaat perfect kunt voorspellen. Er is echter een bepaalde grootte van de fout die acceptabel is voor het bedrijf. Tijdens de validatie, willen we ervoor zorgen dat de fout van onze model tekstvoorspelling op het niveau is of beter dan het niveau van de tolerantie bedrijven. Daarom moet u het niveau van de toegestane fout aan het begin van de cyclus tijdens de fase **Probleem formulering** instellen.

### <a name="typical-evaluation-techniques"></a>Normale evaluatietechnieken
Er zijn verschillende manieren in welke tekstvoorspelling fout kan worden gemeten en bepaalde. In deze sectie gaan we de discussie op evaluatietechnieken relevante tijdreeks en bepaalde voor energie vraag prognose.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE staat voor Absolute Percentage Error bedoelt. Met MAPE we het verschil tussen zijn computing elk prognose punt en de waarde van dat punt. We vervolgens kwantificeer de fout per punt door te berekenen van het deel van het verschil ten opzichte van de werkelijke waarde. In de laatste stap gemiddelde we deze waarden. De wiskundige formule die wordt gebruikt voor MAPE is het volgende:

![MAPE formule](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*waarbij een<sub>t</sub> de actuele waarde is, F,<sub>t</sub> de voorspelde waarde is, en n de prognose horizon.*

## <a name="deployment"></a>Implementatie
Als we hebben, kunt u de fase modellering en de prestaties model gevalideerd zijn we wilt verzenden naar de implementatiefase. In deze context de implementatie betekent het inschakelen van de klant het model door te voeren werkelijke voorspellingen op deze grote schaal in beslag neemt. Het concept van het implementatie is heel belangrijk in Azure ML aangezien onze belangrijkste doel is om aan te roepen voortdurend voorspellingen in plaats van alleen het verkrijgen van het inzicht van de gegevens. De implementatiefase is het gedeelte waar we het model te worden gebruikt bij het op grote schaal inschakelen.

Binnen de context van energie aanvraagprognose is ons doel om aan te roepen continue en periodieke prognoses en ervoor zorgen dat dat nieuwe gegevens beschikbaar zijn voor het model is en dat de voorspelde gegevens wordt verstuurd naar de client in beslag nemen.

### <a name="web-services-deployment"></a>Web Services-implementatie
De belangrijkste bruikbare bouwsteen in Azure ML is de webservice. Dit is het meest efficiënte manier om verbruik van een blog model in de cloud inschakelen. De webservice omvat het model en deze wordt afgerond met een [RESTful](http://www.restapitutorial.com/) API (Application Programming Interface). De API kan worden gebruikt als onderdeel van een clientcode, zoals in het onderstaande diagram.

![We Service-implementatie- en verbruik](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Zoals u kunt zien, wordt de webservice wordt geïmplementeerd in de cloud Cortana Intelligence-Suite en kan worden aangeroepen met het weergegeven REST API-eindpunt. De service via de Web-API kunt tegelijk aanroepen in verschillende soorten clients tussen verschillende domeinen. De webservice kan ook schalen ter ondersteuning van duizenden gelijktijdige gesprekken.

### <a name="a-typical-solution-architecture"></a>Een typische oplossingsarchitectuur
Wanneer u een energie-aanvraag prognoses oplossing implementeert, zijn we geïnteresseerd bij de implementatie van een complete oplossing die gaat verder dan de webservice tekstvoorspelling en de hele gegevensstroom vergemakkelijkt. Op het moment dat we een nieuwe prognose roepen, zou doen we nodig om ervoor te zorgen dat het model met de functies van de actuele gegevens worden ingevoerd. Dat betekent dat de zojuist verzamelde onbewerkte gegevens is voortdurend ingenomen, verwerkte en in de vereiste functieset omgezet op waarin het model is gebouwd. Tegelijkertijd willen we de voorspelde gegevens beschikbaar maken voor het einde die door andere clients. Een voorbeeld van gegevens stroom cyclus (of gegevens verkooppijplijn) wordt geïllustreerd in het onderstaande diagram:

![Energie aanvraag prognose gegevensstroom End-to-End](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Dit zijn de stappen die als onderdeel van de energie aanvraag voorspellen cyclus plaatsvinden:
1.  Miljoenen gedistribueerde gegevens meter genereert voortdurend power verbruik van gegevens in realtime.
2.  Deze gegevens worden verzameld en geüpload naar een cloud-bibliotheek (*bijvoorbeeld*Azure Blob).
3.  Voordat u worden verwerkt, wordt de onbewerkte gegevens aan een onderstation of regionale niveau samengevoegd, zoals gedefinieerd door het bedrijf.
4.  De functie verwerking (Zie **voorbereiding van gegevens en het verwerken van de functie**) vervolgens plaatsvindt en levert de gegevens die is vereist voor model training of scores: de functie setgegevens worden opgeslagen in een database (*bijvoorbeeld*SQL Azure wordt aangegeven).
5.  De opnieuw training service wordt aangeroepen opnieuw trainen de prognose model, dat de nieuwste versie van het model blijft behouden, zodat deze kan worden gebruikt door de score webservice.
6.  De score webservice wordt aangeroepen volgens een schema die voldoet aan de vereiste voorspelde frequentie.
7.  De voorspelde gegevens worden opgeslagen in een database die kan worden geopend door de einde verbruik-client.
8.  De client verbruik haalt de prognoses, past deze weer in het raster en verbruikt deze volgens de vereiste use-case.

Het is belangrijk Houd er rekening mee dat deze hele cyclus volledig is geautomatiseerd en op een planning wordt uitgevoerd. De volledige integratie van de cyclus van deze gegevens kunt met hulpprogramma's zoals [Factory van Azure-gegevens](http://azure.microsoft.com/services/data-factory/)worden aangebracht.

### <a name="end-to-end-deployment-architecture"></a>End-to-End-implementatiearchitectuur
Als u wilt een prognose energie aanvraag-oplossing op Cortana Intelligence vrijwel implementeert, moeten we zorgen dat de vereiste onderdelen zijn gemaakt en geconfigureerd.

In het volgende diagram ziet u een typisch Cortana Intelligence op basis-architectuur waarmee wordt geïmplementeerd en orchestrates van de cyclus voor het stroom van gegevens die hierboven wordt beschreven:

![End-to-End-implementatiearchitectuur](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Raadpleeg de energie oplossing-sjabloon voor meer informatie over elk van de onderdelen en de volledige architectuur.
