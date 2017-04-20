<properties
    pageTitle="Azure gebeurtenis Hubs archief | Microsoft Azure"
    description="Overzicht van de functie Azure gebeurtenis Hubs archief."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="azure-event-hubs-archive"></a>Azure gebeurtenis Hubs archief

Azure gebeurtenis Hubs archief kunt u voor het leveren van de streaming gegevens automatisch in uw Hubs gebeurtenis aan een account Blob opslag van uw keuze met extra flexibiliteit naar een tijd opgeven of het formaat van interval van uw keuze. Het instellen van archief is snelle, zijn er geen administratieve kosten uit te voeren en het automatisch schalen met uw gebeurtenis Hubs [doorvoer eenheden](event-hubs-overview.md#capacity-and-security). Gebeurtenis Hubs archief is de eenvoudigste manier om streaming gegevens laadt in Azure en kunt u zich bezighouden met gegevensverwerking in plaats van gegevens vastleggen.

Azure gebeurtenis Hubs archief kunt u realtime en batch gebaseerde pijpleidingen op dezelfde gegevensstroom verwerken. Hiermee kunt u oplossingen bouwen die met uw behoeften na verloop van tijd te vergroten. Of u batch-systemen vandaag met gaten richting toekomstige realtime verwerking maakt, moet een efficiënte koudwatersystemen pad toevoegen aan een bestaande realtime oplossing, kunt gebeurtenis Hubs archief werken met gegevens gemakkelijker streaming.

## <a name="how-event-hubs-archive-works"></a>De werking van de gebeurtenis Hubs archief

Gebeurtenis Hubs is een tijd-bewaarbeleid duurzame buffer voor telemetrielogboek ingress, vergelijkbaar met een verdeelde logboek. De toets om de schaal in gebeurtenis Hubs is het [gepartitioneerde consumenten model](event-hubs-overview.md#partition-key). Elke partition is een onafhankelijke segment van gegevens en onafhankelijk is verbruikt. Na verloop van tijd leeftijden deze gegevens uit, op basis van de configureerbare bewaarperiode. Hierdoor een opgegeven gebeurtenis Hub nooit ' te vol raakt. "

Gebeurtenis Hubs archief kunt u uw eigen Azure-blobopslag account en de Container die worden gebruikt voor de opslag van de gearchiveerde gegevens op te geven. Dit account kunt werken in hetzelfde gebied, als uw Hub gebeurtenis of in een andere regio, toe te voegen aan de flexibiliteit van de functie gebeurtenis Hubs archief.

Gearchiveerde gegevens is geschreven in [Apache Avro][] -indeling. een compacte, snel, binaire indeling vindt u uitgebreide gegevensstructuren met inlineschema. Deze indeling wordt veel gebruikt in het Hadoop-systeem, en door Stream analyses en Azure gegevens Factory. Verderop in dit artikel vindt u meer informatie over het werken met Avro.

### <a name="archive-windowing"></a>Archief Windowing

Gebeurtenis Hubs archief kunt u een venster instellen om te bepalen archiveren. In dit venster is een minimale grootte en -tijd-configuratie met een "eerste WINS-beleid,' wat betekent dat de eerste trigger heeft voorgedaan een archiveringsbewerking veroorzaakt. Als u een venster vijftien-minuut/100 MB archief en 1 MB/s worden verzonden, wordt het venster grootte voordat het tijdvenster activeren. Elke partition onafhankelijk-archieven en schrijft u een blob voltooide blokkeren op het moment van archiveren, met de naam voor de tijd waarop het interval archief is opgetreden. De naamgevingsconventie is als volgt:

```
<Namespace>/<EventHub>/<Partition>/<YYYY>/<MM>/<DD>/<HH>/<mm>/<ss>
```

### <a name="scaling-to-throughput-units"></a>Schaalbaarheid aan doorvoer duureenheden

Gebeurtenis Hubs verkeer wordt bepaald door [doorvoer eenheden](event-hubs-overview.md#capacity-and-security). Een eenheid één doorvoer kunt 1 MB/s of 1000 gebeurtenissen per seconde van ingress en tweemaal die hoeveelheid egress. Standaard gebeurtenis Hubs kan worden geconfigureerd met 1-20 doorvoer eenheden en meer kan worden aangeschaft via een quotum grotere [ondersteuning aanvragen][]. Gebruik voorbij doorvoer gekochte eenheden is vertraagd. Gebeurtenis Hubs archief kopieert gegevens rechtstreeks vanuit het interne opslagcapaciteit van de gebeurtenis Hubs, overslaan doorvoer eenheid egress quota en uw egress voor andere lezers verwerking zoals Stream Analytics of een op te slaan.

Wanneer geconfigureerd, gebeurtenis Hubs archief wordt automatisch uitgevoerd zodra u uw eerste gebeurtenis verzendt. Wordt dit uitgevoerd allen tijde. Als u deze beter wilt voor uw volgende verwerkt als u wilt weten dat het proces werkt, schrijft gebeurtenis Hubs lege bestanden als er geen gegevens. Dit biedt een overzichtelijk cadence en de markering die u kunt uw processors batch-feed.

## <a name="setting-up-event-hubs-archive"></a>Bij het instellen van gebeurtenis Hubs archief

Gebeurtenis Hubs archief kan worden geconfigureerd op de aanmaaktijd van een gebeurtenis Hub via de portal of resourcemanager Azure. U gewoon inschakelen archief door te klikken op de knop **op** . U configureren een opslag-Account en de container door te klikken op de **Container** sectie van het blad. Omdat gebeurtenis Hubs archief gebruikmaakt van service-naar-service verificatie met opslag, hoeft u niet een verbindingsreeks opslag. De resource kiezer selecteert automatisch u de bron-URI voor uw account opslag. Als u de resourcemanager Azure gebruikt, moet u deze URI expliciet opgeven als een tekenreeks.

Het venster van de tijd standaard is vijf minuten. De minimumwaarde is 1, de maximale 15. Het venster **grootte** heeft een bereik van 10-500 MB.

![][1]

## <a name="adding-archive-to-an-existing-event-hub"></a>Archief toevoegen aan een bestaande gebeurtenis Hub

Archieven kunnen worden geconfigureerd op de bestaande gebeurtenis Hubs die in de naamruimte van een gebeurtenis Hubs. De functie is niet beschikbaar op oudere berichten of gemengde type naamruimten. Archief op een bestaande gebeurtenis Hub inschakelen of uw archief-instellingen wijzigen, klikt u op de naamruimte laden van het blad **Essentials** en klik vervolgens op de gebeurtenis Hub waarvoor u wilt inschakelen of wijzigen van de instelling archief. Klik tot slot op in de sectie **Eigenschappen** van het blad geopend zoals wordt weergegeven in de volgende afbeelding.

![][2]

U kunt ook gebeurtenis Hubs archief via Azure resourcemanager sjablonen. Zie [dit artikel](event-hubs-resource-manager-namespace-event-hub-enable-archive.md)voor meer informatie.

## <a name="exploring-the-archive-and-working-with-avro"></a>Het archief verkennen en hiermee werken Avro

Wanneer geconfigureerd, gebeurtenis Hubs archief bestanden gemaakt in de opslag van Azure-account en de container die zijn opgegeven in het venster geconfigureerde tijd. U kunt deze bestanden weergeven in een hulpmiddel zoals [Azure opslag Explorer][]. U kunt de bestanden lokaal hierop inzetten downloaden.

De bestanden die zijn gemaakt met de gebeurtenis Hubs archief bestaan uit de volgende Avro schema:

![][3]

Er is een eenvoudige manier om te verkennen Avro bestanden met behulp van het oppervlak [Avro hulpmiddelen][] van Apache. Na het downloaden van deze oppervlak, kunt u het schema van een specifieke Avro-bestand bekijken door de volgende opdracht uit te voeren:

```
java -jar avro-tools-1.8.1.jar getschema \<name of archive file\>
```

Deze opdracht het resultaat

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

U kunt ook Avro hulpmiddelen voor het bestand converteren naar JSON-indeling en andere bewerkingen uitvoeren.

Als u wilt meer geavanceerde bewerkingen uitvoeren, downloadt en installeert u Avro voor uw keuze van platform. Op het moment van deze schrijven, er implementaties beschikbaar zijn voor C, C++, C\#, Java, NodeJS Perl, PHP, Python en Ruby.

Apache Avro heeft voltooid aan de slag-handleidingen voor [Java][] en [Python][]. U kunt ook het artikel [Aan de slag met gebeurtenis Hubs archief](event-hubs-archive-python.md) lezen.

## <a name="how-event-hubs-archive-is-charged"></a>Hoe gebeurtenis Hubs archief in rekening wordt gebracht

Gebeurtenis Hubs archief is met datalimiet op dezelfde manier worden doorvoer eenheden als een kostprijs per uur. De kosten is rechtstreeks evenredig aan het aantal doorvoer eenheden voor de naamruimte hebt gekocht. Zoals doorvoer eenheden zijn groter en kleiner, worden de gebeurtenis Hubs archief verhoogt en vermindert om overeenkomende prestaties te bieden. De meter optreden samen. De kosten voor de gebeurtenis Hubs archief is $0,10 per uur per eenheid van doorvoer, met een korting 50% tijdens de proefperiode aangeboden.

Gebeurtenis Hubs archief wordt relatief is de eenvoudigste manier om gegevens in Azure. Azure gegevens Lake, Factory van Azure-gegevens en Azure HDInsight gebruikt, u kunt uitvoeren batchbestand en andere analyses van uw keuze met vertrouwde hulpmiddelen en platforms bij een schaal die u nodig hebt.

## <a name="next-steps"></a>Volgende stappen

U meer informatie over de gebeurtenis Hubs kunt via de volgende koppelingen:

- Een volledige [steekproef-toepassing die gebruikmaakt van gebeurtenis Hubs][].
- De [schaal van het verwerken van de gebeurtenis met gebeurtenis Hubs][] steekproef.
- [Overzicht van de Hubs gebeurtenissen][]

[Apache Avro]: http://avro.apache.org/
[verzoek voor ondersteuning]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[1]: ./media/event-hubs-archive-overview/event-hubs-archive1.png
[2]: media/event-hubs-archive-overview/event-hubs-archive2.png
[Azure opslag Explorer]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-archive-overview/event-hubs-archive3.png
[Hulpmiddelen voor Avro]: http://www-us.apache.org/dist/avro/avro-1.8.1/java/avro-tools-1.8.1.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Overzicht van de Hubs gebeurtenissen]: event-hubs-overview.md
[voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[De schaal van het verwerken van de gebeurtenis met gebeurtenis Hubs aanpassen]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3