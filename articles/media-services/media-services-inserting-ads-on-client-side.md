<properties 
    pageTitle="Advertenties aan de clientzijde invoegen | Microsoft Azure" 
    description="Dit onderwerp wordt uitgelegd hoe u advertenties aan de clientzijde invoegt." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="inserting-ads-on-the-client-side"></a>Advertenties aan de clientzijde invoegen

In dit onderwerp bevat informatie over het invoegen van verschillende soorten advertenties aan de clientzijde.

Zie [ondersteund gesloten ondertiteling en Ad invoegpositie standaarden](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads)voor informatie over gesloten ondertiteling en ad-ondersteuning in Live streaming van video's.

>[AZURE.NOTE] Azure Media Player ondersteunt momenteel geen advertenties.

##<a id="insert_ads_into_media"></a>Advertenties invoegen in uw Media

Azure Media Services biedt ondersteuning voor advertenties opnemen via het Windows Media-Platform: speler kaders. Speler kaders met ad-ondersteuning zijn beschikbaar voor Windows 8, Silverlight, Windows Phone 8 en iOS-apparaten. Elke speler framework bevat steekproef-code die u hoe ziet u het implementeren van een toepassing voor de speler. Er zijn drie verschillende soorten advertenties die kunt u uw medialijst: invoegen.

- **Lineair** – volledige frame advertenties die de belangrijkste video onderbreken.
- **Nonlinear** – overlay advertenties die worden weergegeven terwijl de belangrijkste video wordt afgespeeld, meestal een logo of andere statische afbeelding in de speler geplaatst.
- **Companion** – advertenties die worden weergegeven buiten de speler.

Advertenties kunnen worden geplaatst op een willekeurige plaats in de tijdlijn van de belangrijkste video. U moet de speler zien wanneer om af te spelen de ad en welke advertenties om af te spelen. Dit is voltooid met een set standaard XML-bestanden: Video Ad-Service-sjabloon (VAST), digitale Video meerdere Ad afspeellijst (VMAP) Media abstracte volgordebepaling sjabloon (MAST) en digitale Video Player Ad Interface definitie (VPAID). GROTE bestanden opgeven welke advertenties om weer te geven. VMAP bestanden opgeven wanneer u verschillende advertenties afspelen en VAST XML bevatten. MAST bestanden zijn een andere manier om sequentie advertenties die ook VAST XML kan bevatten. VPAID bestanden definiëren een interface tussen de videospeler en de ad of Active Directory-server.

Elke speler framework anders werkt en elk in een eigen onderwerp aan bod. In dit onderwerp wordt het eenvoudige regelingen gebruikt voor het invoegen van advertenties beschrijven. Toepassingen van de videospeler aanvraagt advertenties bij een ad-server. De ad-server kan reageren op een aantal manieren:

- Een VAST bestand terug
- Een bestand VMAP (met ingesloten VAST) retourneren
- Een bestand MAST (met ingesloten VAST) retourneren
- Een VAST bestand met VPAID advertenties retourneren

 
###<a name="using-a-video-ad-service-template-vast-file"></a>Gebruik van een Video Ad-Service (VAST) sjabloonbestand

Een VAST bestand bepaalt welke ad of advertenties om weer te geven. De volgende XML is een voorbeeld van een VAST bestand voor een lineaire ad:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>
    
De lineaire ad wordt beschreven door de **<Linear>** element. Deze bevat de duur van de ad, gebeurtenissen bijhouden, klikt u op door, klik op het bijhouden en een aantal **<MediaFile>** elementen. Bijhouden gebeurtenissen zijn opgegeven in de **<TrackingEvents>** element en een ad-server voor het bijhouden van verschillende gebeurtenissen die optreden tijdens het bekijken van de ad toestaan. In dit geval de start middelpunt, voltooid, en uitvouwen gebeurtenissen worden bijgehouden. De begingebeurtenis vindt plaats wanneer de advertentie wordt weergegeven. Het middelpunt-gebeurtenis vindt plaats wanneer ten minste 50% van de tijdlijn van de ad bekeken is. De volledige gebeurtenis vindt plaats wanneer de ad heeft uitvoert naar het einde. De gebeurtenis uitvouwen treedt op wanneer de gebruiker wordt uitgebreid met de videospeler naar een volledig scherm. Clickthroughs worden aangeduid met een **<ClickThrough>** element van een **<VideoClicks>** element en een URI aan een resource wilt weergeven wanneer de gebruiker op de ad klikt aangeeft. ClickTracking is opgegeven in een **<ClickTracking>** element, ook binnen de **<VideoClicks>** element en Hiermee geeft u een resource bijhouden voor de speler wanneer de gebruiker op de ad aanvragen. De **<MediaFile>** elementen informatie over een bepaalde codering van een advertentie opgeven. Als er meer dan één **<MediaFile>** element, de videospeler kunt kiezen de beste codering voor het platform. 

Lineaire advertenties kunnen worden weergegeven in een bepaalde volgorde. Klik hiertoe toevoegen extra <Ad> elementen aan de VAST bestand en de volgorde met het kenmerk sequentie opgeven. Het volgende voorbeeld ziet u dit:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>
    
Niet-lineair advertenties zijn opgegeven in een <Creative> ook element. Het volgende voorbeeld ziet een <Creative> element waarmee een niet-lineair ad beschreven.

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>

 
De **<NonLinearAds>** element kan bevatten een of meer **<NonLinear>** elementen, die elk een niet-lineair ad kunt beschrijven. De **<NonLinear>** element Hiermee geeft u de resource voor de niet-lineair ad. De resource kan zijn een **<StaticResouce>**, een **<IFrameResource>**, of een **<HTMLResouce>**.**<StaticResource>** een niet-HTML-bron wordt beschreven en wordt gedefinieerd een creativeType-kenmerk waarmee wordt opgegeven hoe de resource wordt weergegeven:

Afbeelding van de/GIF-bestand, afbeelding/jpeg, afbeelding/png – de resource wordt weergegeven in een HTML- **<img>** tag.

Toepassing/x-javascript – de resource wordt weergegeven in een <**script**> HTML-code.

Toepassing/x-shockwave-flash – de resource wordt weergegeven in een Flash player.

**<IFrameResource>**Beschrijving van een HTML-bron die kan worden weergegeven in een IFrame. **<HTMLResource>**Beschrijving van een deel van een HTML-code die kan worden ingevoegd in een webpagina. **<TrackingEvents>**Geef bijhouden gebeurtenissen en de URI aanvragen als de gebeurtenis. In dit voorbeeld worden de gebeurtenissen acceptInvitation en samenvouwen bijgehouden. Voor meer informatie over de **<NonLinearAds>** element en de onderliggende, raadpleegt u IAB.NET/VAST. Houd er rekening mee dat het **<TrackingEvents>** element zich bevindt in de** <NonLinearAds> ** element plaats van de **<NonLinear>** element.

Companion advertenties zijn gedefinieerd binnen een <CompanionAds> element. De <CompanionAds> element kan bevatten een of meer <Companion> elementen. Elke <Companion> element een ad companion beschreven en kan bevatten een <StaticResource>, <IFrameResource>, of <HTMLResource> die zijn opgegeven op dezelfde manier als in een niet-lineair ad. Een VAST bestand kan bevatten meerdere companion advertenties en de toepassing speler kunt kiezen de meest geschikte ad om weer te geven. Zie voor meer informatie over VAST, [VAST 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

###<a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Met behulp van een digitale Video meerdere Ad afspeellijst (VMAP)-bestand

Een bestand VMAP kunt u opgeven wanneer ad-einden voorkomen, hoe lang elke einde is hoeveel advertenties kunnen worden weergegeven binnen een einde en welke soorten advertenties kunnen worden weergegeven tijdens een einde. In een voorbeeld van een VMAP bestand waarmee wordt gedefinieerd een enkele ad-einde volgt:
    
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
     
Een bestand VMAP begint met een <VMAP> element met een of meer <AdBreak> elementen, elke definiëren van een ad-einde. Elke ad-einde Hiermee geeft u een type einde, einde-ID en tijdverschil. Geeft het type ad die kan worden afgespeeld tijdens het einde van het kenmerk breakType: lineaire, niet-lineair, of weer te geven. Advertenties kaart voor VAST companion advertenties weergeven. Meer dan één ad-type kan worden opgegeven in een lijst gescheiden door komma's (geen spaties). De breakID is een optionele aanduiding voor de ad. De timeOffset aangeeft wanneer de ad moet worden weergegeven. Dit kan worden opgegeven in een van de volgende manieren:

1. Tijd in de: mm: ss of hh:mm:ss.mmm waar .mmm milliseconden is. De waarde van dit kenmerk bevat de tijd vanaf het begin van de video tijdlijn naar het begin van het ad-einde.
1. Percentage – in n % indeling is waarbij n het percentage van de video tijdlijn om af te spelen voordat u de ad wordt afgespeeld
1. Begin/einde – Hiermee wordt opgegeven dat een advertentie moet worden weergegeven voor of na de video is weergegeven
1. Plaats – Hiermee geeft u de volgorde van ad-einden wanneer de timing van de ad-einden onbekend, bijvoorbeeld in live streaming is. De volgorde van elk ad-einde is opgegeven in de #n indeling waarbij n een geheel getal 1 of groter is. 1 duidt op de ad moet worden afgespeeld op de eerste verkoopkans, 2 duidt op de ad moet worden afgespeeld op de tweede verkoopkans enzovoort.

Binnen het element <**AdBreak**> kunnen er één <**AdSource**>-element. Het element <**AdSource**> bevat de volgende kenmerken:

1. -ID: Hiermee geeft u een id voor de ad-bron
1. allowMultipleAds – een Booleaanse waarde die aangeeft of meerdere advertenties kunnen worden weergegeven tijdens het ad-einde
1. followRedirects – een optionele Booleaanse waarde waarmee wordt opgegeven als de videospeler moet voldoen aan wordt omgeleid binnen een ad-antwoord

Het element <**AdSource**> biedt de speler een inline ad antwoord wordt verzonden of een verwijzing naar een ad-antwoord wordt verzonden. Dit kan een van de volgende elementen bevatten:

- <VASTAdData>Geeft aan dat een VAST ad-antwoord is ingesloten in het bestand VMAP
- <AdTagURI>een URI die verwijst naar een ad-antwoord wordt verzonden vanuit een ander systeem
- <CustomAdData>-een willekeurige tekenreeks die respresents een niet-VAST antwoord

In dit voorbeeld een antwoord in line ad is opgegeven met een <VASTAdData> element met een VAST ad-reactie. Zie voor meer informatie over de andere elementen, [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

Het element <**AdBreak**> kan ook één <**TrackingEvents**>-element bevatten. Het element <**TrackingEvents**> kunt u voor het bijhouden van het begin of einde van een ad-einde of of een fout opgetreden tijdens de ad-einde. Het element <**TrackingEvents**> bevat een of meer <**bijhouden**> elementen, die elk Hiermee geeft u een gebeurtenis bijhouden en een bijhouden URI. De mogelijke bijhouden gebeurtenissen zijn:

1. breakStart – wordt bijgehouden voor het begin van een ad-einde
1. breakEnd – de voltooiing van een ad-einde bijhouden
1. Fout: een fout is opgetreden tijdens de ad-einde wordt bijgehouden

Het volgende voorbeeld ziet u een bestand VMAP waarmee wordt opgegeven bijhouden gebeurtenissen

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

Zie voor meer informatie over het element <**TrackingEvents**> en de onderliggende sites, http://iab.org/VMAP.pdf

###<a name="using-a-media-abstract-sequencing-template-mast-file"></a>Gebruik van een sjabloonbestand (MAST) voor volgordebepaling Media-samenvatting

Een bestand MAST kunt u opgeven triggers die definiëren wanneer een advertentie wordt weergegeven. Hier volgt een voorbeeld van een MAST bestand met triggers voor een pre album ad, een ad midden album en een post album ad.

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' the trigger for the next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>

 

Een bestand MAST begint met een **<MAST>** element met een **<triggers>** element. De <triggers> element bevat een of meer **<trigger>** elementen die bepalen wanneer een advertentie moet worden afgespeeld. 

De **<trigger>** element bevat een **<startConditions>** element dat opgeven waarop een advertentie moet beginnen om af te spelen. De **<startConditions>** element bevat een of meer <condition> elementen. Wanneer elke <condition> resulteert in waar een trigger wordt gestart of ingetrokken afhankelijk van of u de <condition> is opgenomen in een **< startConditions**> of **<endConditions>** element respectievelijk. Wanneer meerdere <condition> elementen aanwezig zijn, maar worden behandeld als een impliciete OR, een willekeurige voorwaarde evalueren in true, wordt de trigger om te starten. <condition>elementen kunnen worden genest. Wanneer onderliggende <condition> elementen zijn vooraf ingesteld, maar worden behandeld als een impliciete AND, alle voorwaarden moeten als resultaat waar voor de trigger om te starten. De <condition> element bevat de volgende kenmerken die de voorwaarde definiëren: 

1. **type** , bevat het soort voorwaarde, gebeurtenis of eigenschap
1. **naam** : de naam van de eigenschap of de gebeurtenis moet worden gebruikt tijdens de evaluatie
1. **waarde** – de waarde die een eigenschap wordt geëvalueerd tegen
1. **operator** – de bewerking moet worden gebruikt tijdens de evaluatie: EQ (equal), NEQ (niet gelijk aan), GTR (groter), GEQ (groter of gelijk), LT (kleiner dan), LEQ (kleiner dan of gelijk aan), rest (modulo)

**<endConditions>**ook bevatten <condition> elementen. Als de voorwaarde waar de trigger wordt opnieuw ingesteld. De <trigger> element bevat ook een <sources> element met een of meer <source> elementen. De <source> elementen definiëren de URI met het antwoord ad en het type ad-antwoord. In dit voorbeeld wordt een URI aan een VAST antwoord gegeven. 


    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>
 

###<a name="using-video-player-ad-interface-definition-vpaid"></a>Gebruik van Video-speler-Ad interfacedefinitie (VPAID)

VPAID is een API voor het inschakelen van uitvoerbare ad-eenheden om te communiceren met een videospeler. Hiermee kunt interactieve ad ervaringen. De gebruiker kunt communiceren met de ad en de ad kunt reageren op acties die u hebt gemaakt door de viewer. Een advertentie kan bijvoorbeeld knoppen waarmee de gebruiker om weer te geven voor meer informatie of een langere versie van de ad weergeven. De videospeler ondersteunt de API VPAID en de uitvoerbare ad de API moet implementeren. Een speler vraagt wanneer dat een advertentie van een ad-server de server kan antwoorden met een VAST antwoord dat een ad VPAID bevat.

Een uitvoerbare ad is gemaakt in de code die moet worden uitgevoerd in een runtimeomgeving zoals Adobe Flash™ of JavaScript die kan worden uitgevoerd in een webbrowser. Wanneer een ad-server geeft als resultaat een VAST antwoord met een ad VPAID, wordt de waarde van de apiFramework kenmerk de <MediaFile> element moet "VPAID". Dit kenmerk geeft aan dat de container ad een uitvoerbare ad VPAID. Het typekenmerk moet zijn ingesteld op het type MIME het uitvoerbare bestand, zoals "application/x-shockwave-flash" of "toepassing/x-javascript". De volgende XML-fragment ziet u de <MediaFile> een VAST antwoord met een VPAID uitvoerbare ad-element. 

    
    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>
 

Een uitvoerbare ad kunt geïnitialiseerd met de <AdParameters> element van de <Linear> of <NonLinear> elementen in een VAST antwoord. Voor meer informatie over de <AdParameters> element, Zie [VAST 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). Zie voor meer informatie over de API VPAID, [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

##<a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Uitvoering van een Windows- of Windows Phone 8 speler met Ad-ondersteuning

Het Microsoft-Media-Platform: Speler Framework voor Windows 8 en Windows Phone 8 bevat een verzameling steekproef-toepassingen die u hoe u een videospeler-toepassing met het framework implementeert weergeven. U kunt het kader Player en in de voorbeelden downloaden van [speler Framework voor Windows 8 en Windows Phone 8](https://playerframework.codeplex.com).

Bij het openen van de oplossing Microsoft.PlayerFramework.Xaml.Samples ziet u een aantal mappen binnen het project. De map reclame bevat de voorbeeldcode die relevant zijn voor het maken van een videospeler met ad-ondersteuning. In de reclame is map een aantal XAML/cs bestanden weergeven waarvan het invoegen van advertenties op een andere manier. De volgende lijst worden alle beschreven:

- AdPodPage.xaml ziet u hoe u een ad-pod weergeeft.
- AdSchedulingPage.xaml ziet hoe u advertenties plannen.
- FreeWheelPage.xaml ziet u hoe u met de invoegtoepassing voor FreeWheel advertenties plannen.
- MastPage.xaml ziet hoe u advertenties plannen met een MAST-bestand.
- ProgrammaticAdPage.xaml ziet hoe u via programmacode advertenties plannen in een video.
- ScheduleClipPage.xaml ziet hoe u het plannen van een advertentie zonder een VAST bestand.
- VastLinearCompanionPage.xaml ziet u het invoegen van een lineaire en companion ad.
- VastNonLinearPage.xaml ziet hoe u een niet-lineair ad invoegen.
- VmapPage.xaml ziet hoe u advertenties opgeven met een VMAP-bestand.

Elk van deze voorbeelden wordt gebruikt voor de klas MediaPlayer is gedefinieerd door het kader van de speler. De meeste voorbeelden gebruik Plug-ins die ondersteuning voor verschillende indelingen voor ad-antwoord toevoegen. De steekproef ProgrammaticAdPage communiceert via programmacode met een exemplaar MediaPlayer.

###<a name="adpodpage-sample"></a>Voorbeeld van AdPodPage

Dit voorbeeld wordt de AdSchedulerPlugin gebruikt bij het weergeven van een advertentie definiëren. In dit voorbeeld is een advertentie midden album worden gepland wordt afgespeeld na 5 seconden. De ad-pod (een groep advertenties in volgorde) is opgegeven in een VAST bestand geretourneerd door een ad-server. De URI naar het bestand dat VAST is opgegeven in de <RemoteAdSource> element.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
    
        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>
    
                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>
    
                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

Zie voor meer informatie over de AdSchedulerPlugin, [reclame in het kader Player op Windows 8 en Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

###<a name="adschedulingpage"></a>AdSchedulingPage

In dit voorbeeld wordt ook gebruikt voor de AdSchedulerPlugin. Deze drie advertenties, een ad oude album, een ad midden album en een ad-album, post worden gepland. De URI naar de VAST voor elke ad is opgegeven in een <RemoteAdSource> element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>
    
                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


###<a name="freewheelpage"></a>FreeWheelPage

In dit voorbeeld wordt de FreeWheelPlugin waarmee een bronkenmerk waarmee een URI die verwijst naar een bestand SmartXML waarmee wordt opgegeven ad inhoud, evenals ad plannen, informatie gebruikt.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="mastpage"></a>MastPage

In dit voorbeeld wordt de MastSchedulerPlugin waarmee u een bestand MAST gebruikt. Het kenmerk Source geeft de locatie van het bestand MAST.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="programmaticadpage"></a>ProgrammaticAdPage

In dit voorbeeld is via een programma werkt samen met de MediaPlayer. Het bestand ProgrammaticAdPage.xaml wordt de MediaPlayer:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

Het bestand ProgrammaticAdPage.xaml.cs Hiermee maakt u een AdHandlerPlugin, voegt u een TimelineMarker om op te geven wanneer een advertentie moet worden weergegeven, en telt vervolgens een handler voor de MarkerReached-gebeurtenis die een RemoteAdSource geven een URI naar een bestand VAST geladen en vervolgens het afspelen van de ad.
    
    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;
    
            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }
    
            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

###<a name="scheduleclippage"></a>ScheduleClipPage

In dit voorbeeld wordt de AdSchedulerPlugin plannen van een ad-album, midden worden door het opgeven van een WMV-bestand met de ad gebruikt.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearcompanionpage"></a>VastLinearCompanionPage

In dit voorbeeld ziet u hoe de AdSchedulerPlugin gebruiken om te plannen van een lineaire ad midden album met een advertentie companion. De <RemoteAdSource> element Hiermee geeft u de locatie van het bestand VAST.
    
    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage

In dit voorbeeld worden de AdSchedulerPlugin een lineaire plannen en een niet-lineair ad. De locatie van het VAST is opgegeven met de <RemoteAdSource> element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
                            
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vmappage"></a>VMAPPage

Deze voorbeelden wordt de VmapSchedulerPlugin gebruikt om te plannen met behulp van een bestand VMAP advertenties. De URI naar het bestand VMAP is opgegeven in het bron-kenmerk van de <VmapSchedulerPlugin> element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

##<a name="implementing-an-ios-video-player-with-ad-support"></a>Een iOS-Video-speler met Ad-ondersteuning implementeren


Het Microsoft-Media-Platform: Speler Framework voor iOS bevat een verzameling steekproef-toepassingen die u hoe u een videospeler-toepassing met het framework implementeert weergeven. U kunt het kader Player en in de voorbeelden downloaden van [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework). De pagina github heeft een koppeling naar een Wiki met aanvullende informatie over het framework speler en een introductie over de steekproef speler: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).


###<a name="scheduling-ads-with-vmap"></a>Advertenties met VMAP plannen

Het volgende voorbeeld ziet u hoe u met behulp van een bestand VMAP advertenties plant.

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest
    
    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

###<a name="scheduling-ads-with-vast"></a>Advertenties met VAST plannen

In het onderstaande voorbeeld ziet hoe u een late binding VAST ad plannen.
    
    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
         
   In het onderstaande voorbeeld ziet hoe u een vroege binding VAST ad plannen.
Planning van voorbeeld 4: een vroege binding VAST ad //Download de VAST bestand als (! [ framework.adResolver downloadManifest: & manifest withURL: [NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[self logFrameworkError];} anders {adLinearTime.startTime = 7; adLinearTime.duration = 0;
        
        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

In het onderstaande voorbeeld ziet hoe u het invoegen van een advertentie ruw knippen bewerken (Dwingen) gebruiken

    //Example:1 How to use RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

Het volgende voorbeeld ziet u hoe u een ad-pod plant.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;
    
    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Het volgende voorbeeld ziet u hoe u een niet-Plaktoetsen midden album ad plant. Een niet-Plaktoetsen ad is alleen afgespeeld wanneer ongeacht eventuele zoeken de viewer voert uit.
    
    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Het volgende voorbeeld ziet u hoe u een Plaktoetsen midden album ad plant. Een Plaktoetsen ad worden weergegeven zodra die het opgegeven punt in de video tijdlijn is bereikt.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


In het onderstaande voorbeeld ziet hoe u een post album ad plannen.

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

In het onderstaande voorbeeld ziet hoe u een oude album ad plannen.
    
    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

In het onderstaande voorbeeld ziet hoe u een ad midden album overlay plannen.
    
    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
##<a name="see-also"></a>Zie ook

[Toepassingen van de videospeler ontwikkelen](media-services-develop-video-players.md)
