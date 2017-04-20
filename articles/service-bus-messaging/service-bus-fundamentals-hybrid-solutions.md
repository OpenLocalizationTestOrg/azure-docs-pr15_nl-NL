<properties 
    pageTitle="Azure-Service Bus | Microsoft Azure" 
    description="Een inleiding tot de Service Bus gebruikt om verbinding maken met andere software van Azure-toepassingen." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/31/2016" 
    ms.author="sethm"/>

# <a name="azure-service-bus"></a>Azure-Service Bus

Of een toepassing of service wordt uitgevoerd in de cloud of on-premises, worden deze vaak nodig om te communiceren met andere toepassingen of services. Als u wilt bieden ruim zijn handig u dit wilt doen, biedt Microsoft Azure Service Bus. In dit artikel gaat bekijkt u deze technologie, waarin wordt beschreven wat is het en waarom u mogelijk wilt gebruiken.

## <a name="service-bus-fundamentals"></a>Service Bus over grondbeginselen

Verschillende situaties belt voor verschillende stijlen van communicatie. Soms is laten toepassingen verzenden en ontvangen via een eenvoudige wachtrij de beste oplossing. In andere situaties een gewone wachtrij niet genoeg; een wachtrij met een publiceren en abonnementen om is betere. In sommige gevallen, alle die is echt noodzakelijk is een verbinding tussen toepassingen. wachtrijen zijn niet verplicht. Service Bus biedt alle drie opties, zodat uw toepassingen om te communiceren op verschillende manieren.

Service Bus is een meerdere tenant-cloudservice, wat betekent dat de service wordt gedeeld door meerdere gebruikers. Elke gebruiker, zoals de ontwerper van een toepassing Hiermee maakt u een *naamruimte*en vervolgens de communicatie regelingen die ze binnen die naamruimte moet definieert. Afbeelding 1 ziet u hoe dit eruitziet.

![][1]
 
**Afbeelding 1: Service Bus biedt een service meerdere tenant om verbinding te maken van toepassingen tot en met de cloud.**

In een naamruimte, kunt u een of meer exemplaren van vier verschillende communicatie regelingen, elk van die toepassingen op een andere manier verbindt. De opties zijn:

- *Wachtrijen*, die één richting communicatie toestaan. Elke wachtrij fungeert als een tussenpersoon (ook wel een *makelaar*genoemd) die worden opgeslagen berichten verzonden totdat ze worden ontvangen. Elk bericht is ontvangen door een bepaalde geadresseerde.
- *Onderwerpen*die één richting communicatie door *abonnementen*- een enkel onderwerp te gebruiken kan meerdere abonnementen hebben. Als een wachtrij, een onderwerp fungeert als een makelaar, maar elk abonnement (optioneel) een filter aan berichten die voldoen aan bepaalde criteria kunt gebruiken.
- *Relais*, die bidirectionele communicatie bieden. In tegenstelling tot wachtrijen en onderwerpen opslaan een relay tijdens een vlucht berichten; niet het is niet een makelaar. In plaats daarvan alleen worden doorgegeven ze kunnen de doeltoepassing.

Wanneer u een wachtrij, onderwerp of relay maakt, geven u deze een naam. Gecombineerd met andere u uw naamruimte genoemd, deze naam Hiermee maakt u een unieke id voor het object. Toepassingen kunnen Service Bus deze naam geven en vervolgens die wachtrij, onderwerp of relay gebruiken om te communiceren met elkaar. 

Als kunnen u een van deze objecten in het scenario relay, Windows-toepassingen gebruiken Windows Communication Foundation (WCF). Windows-toepassingen kunnen SMS API's Service Bus gedefinieerde gebruiken voor wachtrijen en onderwerpen. Microsoft biedt als u deze objecten gemakkelijker maken van niet-Windows-toepassingen, SDK's voor Java, Node.js en andere talen. U kunt ook toegang tot wachtrijen en onderwerpen met REST API's via HTTP (s). 

Het is belangrijk om te begrijpen dat hoewel Service Bus zelf wordt uitgevoerd in de cloud (dat wil zeggen in van Microsoft Azure datacenters), waar u ook bent toepassingen die gebruikmaken van deze kunnen worden uitgevoerd. U kunt Service Bus gebruiken om toepassingen op Azure, bijvoorbeeld of toepassingen uitgevoerd binnen uw eigen datacenter te koppelen. U kunt het ook gebruiken om verbinding maken met een toepassing wordt uitgevoerd op Azure of een andere cloud platform met een on-premises implementatie-toepassing of met tablets en -telefoons. Het is zelfs mogelijk om verbinding maken met huishoudelijke apparaten, sensoren en andere apparaten naar een centrale toepassing of naar een andere. Service Bus is een methode communicatie in de cloud die toegankelijk zijn vanaf vrijwel elke locatie is. Hoe u afhankelijk deze van wat uw toepassingen moeten doen.

## <a name="queues"></a>Wachtrijen

Stel dat u besluit om verbinding maken met twee toepassingen van een Service Bus wachtrij gebruiken. Afbeelding 2 ziet u deze situatie.

![][2]
 
**Afbeelding 2: Service Bus wachtrijen vindt u in één richting asynchroon queuing.**

Het proces is heel eenvoudig: een afzender stuurt een bericht naar een Service Bus wachtrij en een ontvanger hervat dat bericht op een later tijdstip. Een wachtrij kunt alleen een enkele ontvanger heeft als afbeelding 2 wordt weergegeven. Of meerdere toepassingen van dezelfde wachtrij kunnen lezen. In deze laatste situatie, wordt elk bericht gelezen door één ontvanger. Voor een meervoudige cast service, moet u een onderwerp in plaats daarvan gebruiken.

Elk bericht bestaat uit twee delen: een set met eigenschappen, elke combinatie van een sleutel/waarde en een binaire berichttekst. Hoe deze worden gebruikt, is afhankelijk van wat een toepassing probeert te doen. Een toepassing die u een bericht over een recente verkoop verzendt kan bijvoorbeeld de eigenschappen *Verkoper = "Ava"* en *bedrag 10000 =*. De berichttekst mogelijk een gescande afbeelding van het ondertekende contract van de verkoop bevatten, of als er niet is, net blijven leeg.

Een ontvanger kan op twee manieren een bericht uit een Service Bus wachtrij lezen. De eerste optie, genaamd *ReceiveAndDelete*, verwijdert een bericht uit de wachtrij en direct verwijderd. Dit is eenvoudige, maar als de ontvanger loopt vast voordat het verwerken van het bericht, het bericht niet verloren. Omdat deze wordt verwijderd uit de wachtrij, geen andere ontvanger toegang tot deze. 

De tweede optie, *PeekLock*, is bedoeld om u te helpen met dit probleem. Een **PeekLock** gelezen Hiermee zoals **ReceiveAndDelete**verwijdert u een bericht uit de wachtrij. Deze verwijderen het bericht echter niet. In plaats daarvan vergrendelen als u het bericht, zodat u deze onzichtbare naar andere ontvangers, en wacht op een van de drie gebeurtenissen:

- Als de ontvanger het bericht is verwerkt, wordt aangeroepen **voltooid**en het bericht in de wachtrij wordt verwijderd. 
- Als de ontvanger besluit dat deze het bericht niet verwerken, wordt aangeroepen **verlaten**. De wachtrij vervolgens de vergrendeling verwijdert uit het bericht en wordt het toegankelijk voor andere ontvangers.
- Als de ontvanger belt geen van deze binnen een configureerbare periode (standaard 60 seconden), wordt in de wachtrij neemt dat de ontvanger is mislukt. In dit geval alsof het de ontvanger had genoemd **verlaten**, het bericht beschikbaar maken voor andere ontvangers.

Zoals u ziet, wat hier kan gebeuren: hetzelfde bericht mogelijk worden bezorgd tweemaal, bijvoorbeeld met twee verschillende ontvangers. Toepassingen van Service Bus wachtrijen gebruiken moeten hiervoor voorbereiden. Het bericht is gelezen om te vereenvoudigen dubbele detectie, dat elk bericht heeft een unieke **MessageID** eigenschap aan die al dan niet standaard hetzelfde, ongeacht hoe vaak blijft uit een wachtrij. 

Wachtrijen zijn handig in helemaal enkele situaties. Ze mogelijk dat toepassingen om te communiceren zelfs wanneer beide waarop niet wordt uitgevoerd op hetzelfde moment, iets dat is met name handig met batch en mobiele toepassingen. Een wachtrij met meerdere ontvangers bevat ook automatische taakverdeling, aangezien verzonden berichten worden verdeeld over deze ontvangers.

## <a name="topics"></a>Onderwerpen

Handig als ze worden wachtrijen zijn niet altijd de juiste oplossing. Soms vaker Service Bus onderwerpen. Afbeelding 3 wordt dit idee.

![][3]
 
**Afbeelding 3:, Is afhankelijk van het filter dat Hiermee geeft u een abonnement neemt-toepassing, kan deze sommige of alle berichten verzonden naar een onderwerp Service Bus ontvangen.**

Een *onderwerp* lijkt op tal van manieren naar een wachtrij. Berichten van afzenders verzenden naar een onderwerp op dezelfde manier dat ze berichten naar een wachtrij en deze berichten verzenden uitzien als u met de wachtrijen. Het grote verschil is dat onderwerpen elke ontvangst toepassing een eigen *abonnement* maken door te definiëren van een *filter*inschakelen. Vervolgens wordt abonnee alleen de berichten die overeenkomen met het filter. Afbeelding 3 ziet u bijvoorbeeld een afzender en een onderwerp met drie abonnees, elk voorzien van een eigen filter:

- 1-abonnee ontvangt alleen berichten met de eigenschap *Verkoper = "Ava"*.
- 2-abonnee ontvangt berichten met de eigenschap *Verkoper = "Ruby"* en/of de eigenschap van een *bedrag* waarvan de waarde groter dan 100.000 is bevatten. Ruby is misschien de Verkoopmanager, zodat ze wil haar eigen verkoop en alle groot verkopen ongeacht die wordt omgezet in.
- Abonnee 3 heeft het filter ingesteld op *waar*, wat betekent dat deze alle berichten ontvangt. Bijvoorbeeld het is mogelijk dat deze toepassing verantwoordelijk voor het onderhouden van een audittrail en kunnen daarom deze nodig om alle berichten weer te geven.

Als u met de wachtrijen, kunnen abonnees van een onderwerp met **ReceiveAndDelete** of **PeekLock**berichten lezen. In tegenstelling tot wachtrijen, kan echter één bericht verzonden naar een onderwerp worden ontvangen door meerdere abonnementen. Deze benadering genoemd *publiceren en abonneren* (of *Publisher/sub*), is handig wanneer meerdere toepassingen bent geïnteresseerd in dezelfde berichten. Als u het juiste filter, toegang elke abonnee hebben tot het deel van de stream bericht dat u deze wilt zien.

## <a name="relays"></a>Relais

Zowel wachtrijen en onderwerpen bieden één richting asynchrone communicatie via een makelaar. Verkeersstromen in één richting en er is geen directe verbinding tussen verzenders en ontvangers. Maar wat gebeurt er als u niet wilt dat dit? Stel dat uw toepassingen moeten zowel berichten verzenden en ontvangen, of misschien wilt u een directe koppeling ertussen en hoeft u een makelaar berichten opslaan. Naar de adres-scenario's zoals dit, Service Bus biedt *doorstuurt*, zoals figuur 4 ziet.

![][4]
 
**Afbeelding 4: De Service Bus relay biedt synchroon, twee richtingen communicatie tussen toepassingen.**

De hand liggende vraag om te vragen over relais is dit: Waarom zou ik een gebruiken? Zelfs als ik niet nodig wachtrijen hebt, waarom toepassingen communiceren via een cloudservice in plaats van alleen rechtstreeks interactie maken? Het antwoord is dat het woord rechtstreeks kan moeilijker dan u denkt.

Stel dat u wilt verbinden met twee on-premises implementatie-toepassingen, worden beide uitgevoerd binnen bedrijfs datacenters. Elk van deze toepassingen bevindt zich achter een firewall en elke datacenter waarschijnlijk NAT (NAT) gebruikt. De firewall geblokkeerd binnenkomende gegevens op alle maar een paar poorten en NAT betekent dat de computer die elke toepassing wordt uitgevoerd op een vaste IP-adres zijn dat u rechtstreeks van buiten het datacenter bereiken kunt geen. Zonder extra hulp bij de is het verbinden van deze toepassingen via de openbare internet problematisch.

Een Service Bus relay kunt. Als u wilt communiceren bi andersom via een relay, elke toepassing tot stand brengt een uitgaande TCP-verbinding met Service Bus en vervolgens behoudt openen. Alle communicatie tussen de twee toepassingen legt via deze verbindingen. Omdat elke verbinding is gemaakt van binnen het datacenter, staat de firewall binnenkomende verkeer naar elke toepassing zonder het openen van nieuwe poorten. Deze methode wordt ook het probleem oplossen door NAT, omdat elke toepassing een consistente eindpunt in de cloud overal in de communicatie heeft. De toepassingen kunnen voorkomen dat de problemen die anders worden doorgevoerd communicatie moeilijk door uitwisseling van gegevens via de relay. 

Als u wilt gebruiken Service Bus relais, vertrouwen toepassingen op de Windows Communication Foundation (WCF). Service Bus biedt WCF-bindingen waardoor deze ingewikkelde voor Windows-toepassingen om te communiceren via relais. Toepassingen die al WCF gebruiken kunnen meestal net een van deze bindingen opgeven en vervolgens met elkaar communiceren via een relay. In tegenstelling tot wachtrijen en onderwerpen, echter gebruikt relais uit niet-Windows-toepassingen, terwijl mogelijk is vereist programming moeite; geen standaard bibliotheken worden gegeven.

In tegenstelling tot wachtrijen en onderwerpen maken niet toepassingen relais expliciet. In plaats daarvan wanneer een toepassing die wil ontvangen tot stand een TCP-verbinding met Service Bus brengt, wordt een relay automatisch gemaakt. Wanneer de verbinding wordt verbroken, wordt de relay verwijderd. Als u wilt dat een toepassing om te vinden van de relay gemaakt door een specifieke luisteraar ervan af, biedt Service Bus een register waarmee toepassingen om te zoeken van een specifieke relay met hun naam weergeven.

Relais zijn de juiste oplossing als u rechtstreeks contact tussen toepassingen nodig hebt. Stel een luchtvaartmaatschappijen reserveringssysteem uitgevoerd in een on-premises implementatie-datacenter dat moet worden geopend vanuit inchecken kiosk, mobiele apparaten en andere computers. Toepassingen op al deze systemen kunnen zijn afhankelijk van Service Bus relais in de cloud om te communiceren, waar ze kunnen worden uitgevoerd.

## <a name="summary"></a>Overzicht

Toepassingen verbinding is deel van het ontwikkelen van oplossingen voorstelt voltooid, en het bereik van scenario's waarvoor toepassingen en services met elkaar communiceren is ingesteld om uit te breiden als meer toepassingen en apparaten zijn verbonden met Internet. Doordat cloudgebaseerde technologieën voor het bereiken van dit tot en met wachtrijen, onderwerpen en relais ernaar Service Bus deze essentiële functie gemakkelijker te implementeren en breder beschikbaar te maken.

## <a name="next-steps"></a>Volgende stappen

U kunt de grondbeginselen van Azure Service Bus hebt geleerd, volgt u deze koppelingen voor meer informatie.

- Het gebruik van de [Service Bus wachtrijen](service-bus-dotnet-get-started-with-queues.md)
- Het gebruik van de [Service Bus onderwerpen](service-bus-dotnet-how-to-use-topics-subscriptions.md)
- Het gebruik van de [Service Bus relay](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
- [Voorbeelden van de service Bus](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
