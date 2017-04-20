<properties
   pageTitle="Betrouwbare service architectuur | Microsoft Azure"
   description="Overzicht van de architectuur betrouwbare Service voor statuscontrole en stateless services"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/30/2016"
   ms.author="alanwar"/>

# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Architectuur voor statuscontrole en stateless betrouwbare Services

Een Service stof betrouwbare van Azure-Service mogelijk statuscontrole of stateless. Elk type service wordt uitgevoerd binnen een bepaalde architectuur. Deze architecturen worden beschreven in dit artikel.
Zie [overzicht van de betrouwbare Service](service-fabric-reliable-services-introduction.md) voor meer informatie over de verschillen tussen statuscontrole en stateless services.

## <a name="stateful-reliable-services"></a>Statuscontrole betrouwbare Services

### <a name="architecture-of-a-stateful-service"></a>Architectuur van een statuscontrole service
![Architectuurdiagram van een statuscontrole service](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Statuscontrole betrouwbare Service

Een statuscontrole betrouwbare Service kunt afgeleid van de StatefulService of StatefulServiceBase klasse. Deze grondtal klassen worden verstrekt door de Service stof. Ze bieden verschillende niveaus van ondersteuning en abstractie voor de statuscontrole service aan Service stof-- en deel te nemen als een service die u binnen het cluster Service stof.

StatefulService afgeleid van StatefulServiceBase. StatefulServiceBase services biedt meer flexibiliteit, maar meer begrip van de interne van Service stof vereist.
Zie [overzicht van de betrouwbare Service](service-fabric-reliable-services-introduction.md) en [betrouwbare Service Geavanceerd gebruik](service-fabric-reliable-services-advanced-usage.md) voor meer informatie over de details van de services met behulp van de klassen StatefulService en StatefulServiceBase schrijven.

Beide grondtal klassen beheren de levensduur en de rol van de service-implementatie. De service-implementatie mogelijk virtuele methoden van beide klasse base overschrijven als de service-implementatie moet doen op die punten in de levenscyclus van de service-implementatie--heeft of als deze wil maken van een communicatieobject-luisteraar ervan af. Houd er rekening mee dat hoewel de implementatie van een service mogelijk een eigen communicatie luisteraar ervan af object ICommunicationListener, weergeeft in het bovenstaande diagram implementeren de communicatie luisteraar ervan af wordt geïmplementeerd door de Service stof--tijdens de uitvoering van de service wordt gebruikt voor een communicatie luisteraar ervan af die wordt geïmplementeerd door de Service stof.

De manager betrouwbare staat een statuscontrole betrouwbare Service gebruikt om te profiteren van betrouwbare siteverzamelingen. Betrouwbare verzamelingen zijn lokale gegevensstructuren die ten zeerste beschikbaar zijn voor de service--, altijd beschikbaar, ongeacht de service failovers. Elk type betrouwbare siteverzameling wordt geïmplementeerd door de provider van een betrouwbare staat.
Zie voor meer informatie over betrouwbare verzamelingen, [betrouwbare verzamelingen overzicht](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Betrouwbare staat manager en staat providers

De manager betrouwbare staat is het object dat betrouwbare staat providers beheert. Dit is de functionaliteit wilt maken, verwijderen, opsommen en zorg ervoor dat de providers betrouwbare staat permanente en ten zeerste beschikbaar zijn. Een exemplaar van de provider betrouwbare staat vertegenwoordigt een exemplaar van een permanente en ten zeerste beschikbaar gegevensstructuur, zoals een woordenlijst of een wachtrij.

Elke provider betrouwbare staat stelt een interface die door een statuscontrole service wordt gebruikt om te communiceren met de provider betrouwbare staat. Bijvoorbeeld IReliableDictionary wordt gebruikt om een koppeling met de betrouwbare woordenlijst, terwijl IReliableQueue wordt gebruikt om een koppeling met de betrouwbare wachtrij. Alle betrouwbare staat providers implementeren de interface IReliableState.

De manager betrouwbare staat heeft een interface benoemde IReliableStateManager, waarmee u toegang krijgt tot het met een statuscontrole-service. Interfaces naar betrouwbaar staat providers worden geretourneerd door IReliableStateManager.

De manager betrouwbare staat gebruikt een invoegtoepassing architectuur zodat nieuwe soorten betrouwbare verzamelingen kunnen worden aangesloten dynamisch.

Betrouwbare woordenlijst en betrouwbare wachtrij zijn gebaseerd op de uitvoering van een krachtige, versienummer differentiële store.

### <a name="transactional-replicator"></a>Transacties distributeur

Het onderdeel transacties replicatie is verantwoordelijk voor dat de status van een service (dat wil zeggen de status binnen de manager betrouwbare provincie en de betrouwbare verzamelingen) consistent is op alle replica's waarop de service wordt uitgevoerd. Ook zorgt u ervoor dat de staat in het logboek behouden is gebleven. De manager-interfaces voor betrouwbare staat met transacties replicatie via een privé om.

Transacties replicatie gebruikt een netwerkprotocol om te communiceren staat met andere replica's van de service-exemplaar zodat alle replica's bijgewerkt statusgegevens hebt.

Transacties replicatie wordt een logboek gebruikt om te behouden statusinformatie zodat de statusinformatie proces blijft of knooppunt loopt vast. De interface naar het logboek is via een privé om.

### <a name="log"></a>Log

De component log biedt een high-performance permanente winkel die kan worden geoptimaliseerd voor schrijven naar draaiende of solid-state schijven.  Het ontwerp van het logboek is bedoeld voor de permanente opslag (dat wil zeggen vaste schijven) op de lokale naar de knooppunten die de statuscontrole service wordt uitgevoerd. Hiermee lage vertragingstijden en hoge doorvoersnelheden ten opzichte van externe permanente opslag, dat wil niet lokale naar het knooppunt zeggen.

De component log gebruikt meerdere logbestanden. Er is een knooppunt hele gedeelde logboekbestand dat alle replica's gebruikt u deze kunt voorzien in de latentie laagste en hoogste doorvoer staat gegevens opslaat. Al dan niet standaard de gedeelde log in de Service stof knooppunt werkmap is geplaatst, maar dit kan ook worden geconfigureerd op een andere locatie, in het ideale geval op een schijf gereserveerd voor alleen de gedeelde log worden geplaatst. Elke replica voor de service, heeft ook een speciale logboekbestand en de speciale log in de werkmap van de service is geplaatst. Er is geen methode voor het configureren van de speciale log moet worden geplaatst op een andere locatie.

Het gedeelde logboek is een overgangs gebied voor informatie over de status van de replica, terwijl het speciale logboekbestand is de uiteindelijke bestemming waar persistent wordt gemaakt. In dit ontwerp is de statusinformatie eerst naar het gedeelde logboekbestand geschreven en vervolgens lazily worden verplaatst naar de speciale logboekbestand op de achtergrond. Op deze manier zou het schrijven naar het gedeelde logboek de latentie laagste en hoogste doorvoer waarmee de service om voortgang sneller te hebben.

Gelezen en geschreven naar het gedeelde logboek worden uitgevoerd via directe IO naar vooraf toegewezen ruimte op de schijf voor het gedeelde logboekbestand. Als u optimaal gebruik van schijfruimte op het station met speciale Logboeken, wordt het speciale logboekbestand gemaakt als een verspreid NTFS-bestand. Houd er rekening mee dat hierdoor kunnen overprovisioning schijfruimte en de speciale logboekbestanden met veel meer schijfruimte dan werkelijk wordt gebruikt door het besturingssysteem wordt weergegeven.

Afgezien van een interface minimale gebruiker-modus in het logboek, is het logboek geschreven als een kernelmodusstuurprogramma. Het logboek kunt uitvoeren als een kernelmodusstuurprogramma, de beste prestaties tot alle services dat deze gebruikt bieden.

Zie voor meer informatie over het configureren van het logboek [Configureren statuscontrole betrouwbare Services](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Stateless betrouwbare Service

### <a name="architecture-of-a-stateless-service"></a>Architectuur van een stateless service
![Architectuurdiagram van een stateless service](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Stateless betrouwbare Service

Stateless service implementaties afgeleid van de klasse StatelessService of StatelessServiceBase. De klas StatelessServiceBase hebt u meer mogelijkheden dan de klas StatelessService.
Beide grondtal klassen beheren de levensduur en de rol van een service.

De service-implementatie mogelijk virtuele methoden van beide klasse base overschrijven als de service moet doen op die punten in de levenscyclus van de service--heeft of als deze wil maken van een communicatieobject-luisteraar ervan af. Houd er rekening mee dat hoewel de service mogelijk een eigen communicatie luisteraar ervan af object ICommunicationListener, weergeeft in het bovenstaande diagram implementeren de communicatie luisteraar ervan af wordt geïmplementeerd door de Service stof, zoals de implementatie van die service wordt gebruikt voor een communicatie luisteraar ervan af die wordt geïmplementeerd door de Service stof.

Zie [overzicht van de betrouwbare Service](service-fabric-reliable-services-introduction.md) en [betrouwbare Service Geavanceerd gebruik](service-fabric-reliable-services-advanced-usage.md) voor meer informatie over de details van de services met de klassen StatelessService en StatelessServiceBase schrijven.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Service stof:

[Overzicht van de betrouwbare service](service-fabric-reliable-services-introduction.md)

[Snel aan de slag](service-fabric-reliable-services-quick-start.md)

[Overzicht van een betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md)

[Betrouwbare service Geavanceerd gebruik](service-fabric-reliable-services-advanced-usage.md)

[Betrouwbare service te configureren](service-fabric-reliable-services-configuration.md)  
