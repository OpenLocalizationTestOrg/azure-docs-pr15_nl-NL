<properties 
    pageTitle="Een MPEG-streepje geavanceerde Streaming van Video insluiten in een toepassing HTML5 met DASH.js | Microsoft Azure" 
    description="Dit onderwerp wordt beschreven hoe u een MPEG-streepje geavanceerde Streaming-Video insluiten in een toepassing HTML5 met DASH.js." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>Een MPEG-streepje geavanceerde Streaming van Video insluiten in een toepassing HTML5 met DASH.js

##<a name="overview"></a>Overzicht

MPEG-streepje is een ISO-standaard voor de geavanceerde streaming van video-inhoud, dat grote voordelen voor degenen die wilt geven van hoge kwaliteit, geavanceerde video streaming uitvoer bevat. Met MPEG-streepje, wordt de video-stream automatisch neerzetten aan de definitie van een lagere wanneer wordt druk met het netwerk. Hierdoor wordt de kans van de viewer een 'onderbroken' video zien terwijl de speler downloads van de volgende enkele seconden om af te spelen (of buffer). Als overbezet netwerk wordt beperkt, retourneert de videospeler op zijn beurt naar een hoger kwaliteit stroom. Deze mogelijkheid om aan te passen bandbreedte vereist resulteert ook in een snellere begintijd voor video. Dat betekent dat de eerste paar seconden kunnen worden afgespeeld in een fast-naar-download lagere kwaliteit segment en klikt u vervolgens stap naar een hoger kwaliteit eenmaal voldoende inhoud is in buffer geplaatst.

Dash.js is een bron openen MPEG-streepje videospeler geschreven in JavaScript. Het doel is om aan te bieden een robuuste, platforms-speler die in de toepassingen die opgevolgd video's afspelen moeten vrij kan worden gebruikt. MPEG-streepje afspelen in een browser die ondersteuning biedt voor de W3C Media bron extensies (MSE) vandaag die is Chrome, Microsoft Edge en IE11 (andere browsers hebben aangegeven opzet ter ondersteuning van MSE) krijgen. Js Zie voor meer informatie over DASH.js, de GitHub dash.js opslagplaats.


##<a name="creating-a-browser-based-streaming-video-player"></a>Een browser gebaseerde streaming videospeler maken

Als u wilt maken van een eenvoudige pagina waarin u kunt een videospeler met de verwachte-besturingselementen, zoals een afspelen, pauze, terugspoelen enzovoort, moet u naar:

1. Maken van een HTML-pagina
1. De video tag toevoegen
1. De speler dash.js toevoegen
1. Initialisatie van de speler
1. Een CSS-stijl toevoegen
1. De resultaten bekijken in een browser die wordt geïmplementeerd MSE

Initialisatie van de speler kan worden uitgevoerd in slechts een klein aantal regels JavaScript-code. Met dash.js, heel is eenvoudig naar MPEG-streepje video insluiten in uw op basis-browsertoepassingen.

##<a name="creating-the-html-page"></a>De HTML-pagina maken

De eerste stap is het opzetten van een standaard HTML-pagina met de **video** -element, in dit bestand opslaan als basicPlayer.html, zoals u ziet het volgende voorbeeld:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

##<a name="adding-the-dashjs-player"></a>De speler DASH.js toevoegen

Als u wilt de uitvoering van de verwijzing dash.js hebt toegevoegd aan de toepassing, moet u het bestand dash.all.js van de 1,0 versie van project dash.js pak. Dit moet worden opgeslagen in de map JavaScript van uw toepassing. Dit bestand is een gemak-bestand dat alle benodigde dash.js code in één bestand verzamelt. Als u een uiterlijk rond de opslagplaats dash.js hebt, wordt u de afzonderlijke bestanden hebt gevonden, test-code en nog veel meer, maar als u wilt doen gebruik dash.js en dan het bestand dash.all.js is wat u nodig hebt.

Als u wilt de speler dash.js hebt toegevoegd aan uw toepassingen, door een script-tag te toevoegen aan de hoofdsectie van basicPlayer.html:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Maak vervolgens een functie als u wilt de speler geïnitialiseerd wanneer de pagina wordt geladen. Hiermee voegt u het volgende script na de regel waarin u dash.all.js hebt geladen:

    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

Deze functie maakt eerst een DashContext. Dit wordt gebruikt voor het configureren van de toepassing voor een specifieke runtimeomgeving. Vanuit een oogpunt van de technische deze de klassen worden gedefinieerd die het kader van de webweergave afhankelijkheid gebruiken moet wanneer het bouwen van de toepassing. In de meeste gevallen gebruikt u Dash.di.DashContext.

Vervolgens exemplaar te maken van de primaire klasse van het kader dash.js, MediaPlayer. Deze klasse bevat de core methoden nodig, zoals afspelen en onderbreken, beheer van de relatie met het video-element en beheert ook de interpretatie van de Media presentatie beschrijving (MPD)-bestand geïmporteerd beschrijving van de video moet worden afgespeeld.

De functie startup() van de klasse MediaPlayer heet om ervoor te zorgen dat de speler kan worden afgespeeld video. Onder andere deze functie zorgt ervoor dat alle benodigde klassen (zoals gedefinieerd door de context) zijn geladen. Zodra de speler klaar is, kunt u het video-element aan met de functie attachView() te koppelen. Hiermee worden de MediaPlayer naar de video-stream invoeren in het element en ook het afspelen indien nodig.

De URL van de MPD-bestand aan de MediaPlayer doorgeven zodat deze op de hoogte van de video dat wordt naar verwachting om af te spelen. De zojuist gemaakte setupVideo()-functie moet worden uitgevoerd nadat de pagina volledig is geladen. Deze stap herhalen met behulp van de gebeurtenis onload van het hoofdtekstelement. Wijzigen uw <body> element dat moet worden:

    <body onload="setupVideo()">

Ten slotte, stelt u de grootte van het video-element met CSS. In een omgeving met geavanceerde streaming is dat vooral belangrijk omdat de grootte van de video wordt afgespeeld wijzigen kan, zoals afspelen past netwerkcondities wijzigen. In deze eenvoudige demo moet u gewoon het video-element 80% van de beschikbare browservenster worden door de volgende CSS toe te voegen aan de hoofdsectie van de pagina afdwingen:
    
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

##<a name="playing-a-video"></a>Een Video afspelen

Een video afspelen, wijst u de browser op het bestand basicPlayback.html en klikt u op afspelen op de videospeler weergegeven.


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zie ook

[Toepassingen van de videospeler ontwikkelen](media-services-develop-video-players.md)

[GitHub dash.js opslagplaats](https://github.com/Dash-Industry-Forum/dash.js) 
