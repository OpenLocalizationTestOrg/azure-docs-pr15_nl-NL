<properties
    pageTitle="Openbare gegevens trekken in Azure gebeurtenis Hubs | Microsoft Azure"
    description="Overzicht van de gebeurtenis Hubs importeren uit de web-voorbeeld"
    services="event-hubs"
    documentationCenter="na"
    authors="spyrossak"
    manager="timlt"
    editor=""/>

<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/25/2016"
    ms.author="spyros;sethm" />

# <a name="pulling-public-data-into-azure-event-hubs"></a>Openbare gegevens trekken in Azure gebeurtenis Hubs

In typische Internet van dingen (IoT)-scenario's moet u de apparaten die u push-gegevens naar Azure, hetzij met de Hub van een Azure gebeurtenis of een hub IoT programmeren kunt. Beide deze hubs zijn invoer wordt verwezen in Azure voor het opslaan, analyseren en visualiseren met een groot aantal hulpprogramma's op Microsoft Azure beschikbaar worden gemaakt. Beide vereisen echter dat u gegevens push ze zijn opgemaakt als JSON en beveiligd op specifieke manieren. Hiermee opent u de volgende vraag. Wat moet u doen als u gebruikmaken van gegevens uit openbaar of privé bronnen waar de gegevens als een webservice of een feed van enkele sorteren wordt weergegeven wilt, maar u hebt geen de mogelijkheid om te wijzigen hoe de gegevens wordt gepubliceerd? Houd rekening met de weer of -verkeer is toegestaan of aandelenkoersen - weet u niet NOAA, of WSDOT of NASDAQ voor het configureren van een push op uw Hub gebeurtenis. U lost dit probleem, hebt we geschreven en openen-die afkomstig zijn een kleine cloud-steekproef die u kunt wijzigen en implementeren die de gegevens ophalen uit een dergelijke bron en druk op de Hub van de gebeurtenis. Hierin kunt u doen wat u wilt dat met, onderworpen natuurlijk aan de licentievoorwaarden voor de producent. U vindt de toepassing [hier](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/).

Houd er rekening mee dat de code in dit voorbeeld alleen ziet u hoe u gegevens ophalen uit typische web feeds en hoe u het schrijven op een Hub van de gebeurtenis Azure. Dit is niet bedoeld voor een toepassing productie en geen is geprobeerd zodat u deze geschikt is voor gebruik in een dergelijke omgeving - strictfly een DIY, voorbeeld ontwikkelaars gerichte alleen is. Bovendien is de aanwezigheid van dit voorbeeld niet de Commissie een aanbeveling die u in plaats van **push** Azure **halen** gegevens moet het. U moet controleren beveiliging, prestaties en functionaliteit en factoren voordat op een end-to-end-architectuur kosten.

## <a name="application-structure"></a>Toepassingsstructuur

De toepassing is geschreven in C# en de [Beschrijving van de steekproef](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) bevat alle informatie die u wilt wijzigen, maken en publiceren van de toepassing. De volgende secties bevatten een overzicht van de werking van de toepassing.

We beginnen met ervan uitgegaan dat u toegang tot een gegevensfeed hebt. U wilt bijvoorbeeld halen in het verkeersgegevens uit de afdeling transport van Washington staat of de weergegevens van NOAA, om aangepaste rapporten weer te geven of om die gegevens met andere gegevens in uw toepassing te combineren. U moet ook hebt ingesteld met een Azure gebeurtenis Hub en de verbinding weet tekenreeks nodig toegang toe.

Wanneer u de oplossing GenericWebToEH start, wordt een configuratiebestand (App.config) om een aantal dingen:

1. De URL of een lijst met URL's voor de site die de gegevens te publiceren. In het ideale geval dit is een site waarmee gegevens worden gepubliceerd in JSON, zoals die waarnaar wordt verwezen door WSDOT [hier](http://www.wsdot.wa.gov/Traffic/api/). 
2. Referenties voor de URL, indien nodig. Veel openbare bronnen is geen referenties nodig of kunt u de referenties opnemen in de URL-tekenreeks. Anderen vereisen dat u afzonderlijk opgeven. (Houd er rekening mee dat u slechts één set referenties in deze toepassing, opgeven kunt zodat dit alleen werkt als u slechts één URL, niet een lijst met URL's opgeeft.)
3. De verbindingsreeks en de naam van de Hub gebeurtenis in de naamruimte van die gebeurtenis Hubs, waaraan u de gegevens worden push. U vindt deze informatie in de portal van Azure.
4. Een slaapstand interval, (in milliseconden), voor het interval tussen polling van de site in openbare gegevens. Dit instelt, is voorzichtig vereist. Als u af en toe te controleren, mist u mogelijk gegevens; aan de andere kant, als u te vaak controleren, mogelijk ontvangt u een groot aantal terugkerende gegevens of slechter nog, u wordt mogelijk geblokkeerd als een slechte bots. Houd rekening met hoe vaak de gegevensbron is bijgewerkt - gegevens weer of verkeer kan worden vernieuwd elke 15 minuten, maar aandelenkoersen wellicht om de paar seconden, afhankelijk van waar u deze ophalen. 
5. Een markering om te zien van de toepassing of de gegevens komt als JSON of XML. Aangezien u nodig hebt om de gegevens op een gebeurtenis-Hub, heeft de toepassing een module XML converteren naar JSON voordat u verzendt.

Nadat u hebt het configuratiebestand gelezen, gaat de toepassing in een lus - toegang tot de openbare website, de gegevens zo nodig het schrijven naar uw Hub gebeurtenis en klikt u vervolgens wachten op het interval naar bed gaan voordat u deze helemaal opnieuw te converteren. Name:

  * De openbare website wordt gelezen. Het exemplaar van RawXMLWithHeaderToJsonReader klasse is gebruikt om gegevens te ontvangen gereed om te verzenden vanaf Azure/GenericWebToEH/ApiReaders/RawXMLWithHeaderToJsonReader.cs. Deze bron stream in de methode GetData() leest en vervolgens deze naar kleinere stuks (dat wil zeggen records) via GetXmlFromOriginalText wordt gesplitst. 
  Deze methode leest XML evenals juist opgemaakte JSON of JSON matrix. Klik verwerking wordt gestart met MergeToXML configuratie van App.config (standaard = lege).
  * De gegevens ontvangen en verzenden wordt als een lus vormen in de methode Process() in Program.cs geïmplementeerd. 
  Na de resultaten van de uitvoer van GetData() ontvangt, gescheiden de methode enqueues waarden op de Hub van de gebeurtenis.

## <a name="next-steps"></a>Volgende stappen

Als u wilt de oplossing implementeert, klonen of downloaden van de toepassing [GenericWebToEH](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) , bewerk het bestand App.config bouwen en ten slotte publiceert. Wanneer u de toepassing hebt gepubliceerd, kunt u deze uitgevoerd in de portal van Azure klassieke onder Cloudservices zien en kunt u enkele van de configuratie-instellingen (zoals het doel van de gebeurtenis Hub en het interval naar bed gaan) wijzigen in het tabblad **configureren** .

Zie meer gebeurtenis Hubs voorbeelden in de [voorbeeldengalerie met Azure](https://azure.microsoft.com/documentation/samples/?service=event-hubs) en klik op [MSDN](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5).
