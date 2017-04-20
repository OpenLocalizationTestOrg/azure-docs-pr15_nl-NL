<properties 
    pageTitle="Veelgebruikte bewerkingen in de Machine Learning aanbevelingen API | Microsoft Azure" 
    description="Azure ML aanbeveling steekproef-toepassing" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 


# <a name="recommendations-api-sample-application-walkthrough"></a>Stapsgewijze instructies voor toepassing van aanbevelingen-API-voorbeeld

>[AZURE.NOTE]U moet beginnen met de Service van aanbevelingen-API-cognitieve in plaats van deze versie. De aanbevelingen cognitieve Service wordt deze service worden vervangen, en de nieuwe functies worden er worden ontwikkeld. Er worden nieuwe mogelijkheden, zoals batchen ondersteuning, een betere API Explorer, een duidelijkere API oppervlak, consistenter registratie/facturering ervaring, enzovoort.
> Meer informatie over het [migreren naar de nieuwe cognitieve Service](http://aka.ms/recomigrate)

##<a name="purpose"></a>Doel

In dit document ziet u het gebruik van de Azure Machine Learning aanbevelingen API via een [steekproef-toepassing](https://code.msdn.microsoft.com/Recommendations-144df403).

Deze toepassing is niet bedoeld voor de volledige functionaliteit opnemen, en de API's gebruikt. Laat enkele veelgebruikte bewerkingen uit te voeren wanneer u eerst wilt afspelen met de aanbevelingsservice Machine Learning. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="introduction-to-machine-learning-recommendation-service"></a>Inleiding tot Machine Learning-aanbevelingsservice

Aanbevelingen via de aanbevelingsservice Machine Learning zijn ingeschakeld als u een aanbeveling-model op basis van de volgende gegevens samenstelt:

* Een bibliotheek van de items die u wilt aanbevelen, ook bekend als een catalogus
* Gegevens dat staat voor het gebruik van items per gebruiker of sessie (dit kan worden verkregen na verloop van tijd via gegevensverzameling, niet als onderdeel van de steekproef-app)

Nadat een model aanbeveling is gemaakt, kunt u deze gebruiken om te voorspellen items die een gebruiker mogelijk geïnteresseerd in, op basis van een reeks items (of een enkel item) de gebruiker selecteert.

Schakel in het vorige scenario door het volgende in de aanbevelingsservice Machine Learning te doen:

* Een model maken: dit is een logische container met de gegevens (catalogus en het gebruik) en de voorspelling-modellen. Elke container model wordt aangegeven via een unieke ID, dat wordt toegewezen wanneer deze is gemaakt. Deze ID heet het model-ID en deze worden gebruikt door de meeste van de API's. 
* Uploaden naar catalogus: wanneer een container model is gemaakt, kunt u koppelen aan een catalogus.

**Opmerking**: een model maken en uploaden naar een catalogus worden meestal één keer uitgevoerd voor de levenscyclus van het model.

* Gebruik uploaden: Hiermee voegt u gegevens over zoekgebruik aan de container model.
* Maken van een model aanbeveling: nadat u voldoende gegevens hebt, kunt u het model aanbeveling maken. Deze bewerking wordt de bovenste Machine Learning-algoritmen aanbeveling modellen maken. Elke opbouwen is gekoppeld aan een unieke ID. U wilt bewaren van deze-ID omdat deze nodig zijn voor de functionaliteit van sommige API's.
* Het proces building controleren: een aanbeveling model opbouwen is een asynchrone bewerking en deze kunt uitvoeren vanuit enkele minuten tot enkele uren, afhankelijk van de hoeveelheid gegevens (catalogus en het gebruik) en de parameters opbouwen. Daarom moet u controleren van de build. Een model aanbeveling wordt alleen gemaakt als de bijbehorende Opbouwen voltooid is.
* (Optioneel) Kies een actieve aanbeveling model opbouwen: deze stap is alleen nodig als er meer dan één aanbeveling model ingebouwd in uw model container. Een verzoek om aanbevelingen zonder waarin wordt aangegeven dat het model actieve aanbeveling omgeleid automatisch door het systeem naar de actieve build standaard. 

**Opmerking**: een actieve aanbeveling-model is besturingselement en is de aanpassing gebaseerd voor productie werkbelasting. Dit verschilt van een niet-actieve aanbeveling model staat, in een test-achtige-omgeving blijft (ook wel tijdelijke genoemd).

* Aanbevelingen krijgen: wanneer u een model aanbeveling hebt, kunt u aanbevelingen voor één item of een lijst met items die u selecteert activeren. 

U wordt meestal aanbeveling krijgen aanroepen voor een bepaalde periode. Tijdens deze periode, kunt u gegevens over zoekgebruik omleiden naar het systeem van de aanbeveling Machine Learning, waarmee deze gegevens worden toegevoegd aan de container opgegeven model. Als er voldoende gebruiksgegevens, kunt u een nieuw aanbeveling model waarin de aanvullende gebruiksgegevens kunt maken. 

##<a name="prerequisites"></a>Vereisten voor

* Visual Studio 2013
* Internettoegang 
* Abonnement op de aanbevelingen API (https://datamarket.azure.com/dataset/amla/recommendations).

##<a name="azure-machine-learning-sample-app-solution"></a>Azure Machine Learning voorbeeld app-oplossing

Deze oplossing bevat de broncode, gebruiksvoorbeeld, catalogusbestand en richtlijnen om te downloaden van de pakketten die vereist voor compileren zijn.

##<a name="the-apis-used"></a>De API's gebruikt

De toepassing gebruikt Machine Learning aanbeveling functionaliteit via een subset van beschikbare API's. De volgende API's worden getoond in de toepassing:

* Model maken: een logische container waarin gegevens en aanbeveling modellen maken. Een model wordt aangegeven door een naam en u meer dan één model kan maken met dezelfde naam.
* Catalogusbestand uploaden: voor het uploaden van gegevens in de catalogus.
* Gebruik bestand uploaden: voor het uploaden van gegevens over zoekgebruik.
* Opbouwen activeren: gebruiken om een model aanbeveling te maken.
* Opbouwen execution controleren: gebruiken om te controleren van de status van een aanbeveling model opbouwen.
* Kies een en-klare model voor aanbeveling: gebruiken om aan te geven welke model aanbeveling standaard wordt gebruikt voor een bepaalde model container. Deze stap is nodig alleen als u meer dan één aanbeveling model hebt en u wilt een niet-actieve opbouwen als het model actieve aanbeveling activeren.
* Aanbeveling ophalen: gebruik aanbevolen items op basis van een bepaald één item of een reeks items kunt ophalen. 

Voor een gedetailleerde beschrijving van de API's, raadpleegt u de documentatie van Microsoft Azure Marketplace. 

**Opmerking**: een model kan hebben verschillende builds na verloop van tijd (niet tegelijkertijd). Elke opbouwen wordt gemaakt met hetzelfde of een bijgewerkte catalogus en aanvullende gebruiksgegevens.

## <a name="common-pitfalls"></a>Algemene nadelen

* Moet u uw gebruikersnaam en uw primaire accountsleutel van Microsoft Azure Marketplace om uit te voeren van de steekproef-app.
* De app steekproef opeenvolgend uitgevoerd, mislukt. De toepassing stroom bevat maken, uploaden, bouwen van het beeldscherm en aanbevelingen ophalen uit een vooraf gedefinieerde model; Daarom zal mislukken op opeenvolgende worden uitgevoerd als u de naam van het model tussen aanroepen niet wijzigen.
* Aanbevelingen mogelijk retourneren zonder gegevens. De voorbeeld-app wordt gebruikt een zeer klein catalogus en het gebruik-bestand. Sommige items uit de catalogus wordt daarom geen aanbevolen items bevatten.

## <a name="disclaimer"></a>Vrijwaring
De voorbeeld-app is niet bedoeld om te worden uitgevoerd in een productieomgeving. De gegevens die beschikbaar zijn in de catalogus erg klein is en dit wordt niet om er zelf een zinvolle aanbeveling-model. De gegevens worden weergegeven als een demonstratie. 
 
