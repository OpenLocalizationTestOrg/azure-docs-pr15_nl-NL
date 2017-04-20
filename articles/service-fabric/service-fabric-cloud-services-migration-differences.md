<properties
   pageTitle="Verschillen tussen Cloudservices en Service stof | Microsoft Azure"
   description="Algemene informatie over het migreren van toepassingen van Cloudservices Service stof."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Meer informatie over de verschillen tussen Cloudservices en Service stof voordat u migreert toepassingen.
Microsoft Azure-Service stof is het allernieuwste cloud-toepassing-platform voor ten zeerste scalable, uiterst betrouwbaar gedistribueerde toepassingen. Deze maakt u kennis met veel nieuwe functies voor verpakking, implementeert, bijwerken en beheren van verdeelde cloud-toepassingen. 

Dit is een inleidende handleiding voor het migreren van toepassingen van Cloudservices Service stof. Er is gericht hoofdzakelijk op architectuur en ontwerpverschillen tussen Cloudservices en Service stof.
 
## <a name="applications-and-infrastructure"></a>Toepassingen en -infrastructuur

Een fundamentele verschil tussen Cloudservices en Service stof is de relatie tussen VMs, werkbelasting en toepassingen. Een werkbelasting Hier wordt gedefinieerd als de code die u naar een bepaalde taak uitvoeren of dienstverlening schrijven.
 
 - **Cloudservices gaat over het gebruik van toepassingen als VMs.** De code die u schrijft is nauw verbonden met een exemplaar VM, zoals een Web of werknemer rol. Wordt geïmplementeerd exemplaren van een of meer VM waarop de werklast wordt uitgevoerd als u wilt een werkbelasting in Cloud Services implementeren. Er is geen scheiding van toepassingen en VMs en dus er is geen formele definitie van een toepassing. Een toepassing kan van worden beschouwd als een set Internet of een werknemer rol exemplaren binnen een Cloud Services-implementatie of een hele Cloud Services-implementatie. In dit voorbeeld is een toepassing wordt weergegeven als een set exemplaren van de rol.
 
![Cloud Services-toepassingen en topologie][1]

 - **Service stof gaat over het implementeren van toepassingen op bestaande VMs of machines Service stof waarop Windows of Linux.** De services die u schrijft zijn volledig losgekoppelde uit de onderliggende infrastructuur, waardoor beschouwing afwezig door de Service stof toepassingsplatform, zodat een toepassing kan worden toegepast op meerdere omgevingen. Een werkbelasting in Service stof een 'service' genoemd en een of meer services zijn gegroepeerd in een toepassing voor formeel gedefinieerde die wordt uitgevoerd op de Service stof toepassingsplatform. Meerdere toepassingen kunnen worden geïmplementeerd op een Service stof cluster.
 
![Stof service-toepassingen en topologie][2]
 
Service-stof zelf is een platform toepassingslaag die wordt uitgevoerd op Windows of Linux, terwijl Cloudservices een systeem is voor de implementatie van Azure beheerde VMs met werkbelasting die zijn gekoppeld.
Het model van de toepassing Service stof heeft een aantal voordelen:

 - Snelle implementatietijden. VM exemplaren maken kan enige tijd duren. In de Service stof, worden VMs alleen geïmplementeerd zodra als u wilt een cluster vormen die platform voor de Service stof gehost. Vanaf dat punt, kunnen toepassingspakketten worden geïmplementeerd op het cluster zeer snel.
 - HD hosten. In de Cloud Services host een werknemer rol VM één werkbelasting. In de Service stof zijn de toepassingen los van de VMs die worden uitgevoerd, wat betekent dat u een groot aantal een klein aantal VMs, waarin de totale kosten voor grotere implementaties kunt verlagen-toepassingen kunt implementeren.
 - De Service-stof platform kan worden uitgevoerd ergens die is Windows Server- of Linux machines, of u nu Azure of on-premises implementatie. Het platform vindt u een laag abstraction via de onderliggende infrastructuur zodat uw toepassing kan worden uitgevoerd op verschillende omgevingen. 
 - Verdeelde Toepassingsbeheer. Service stof is een platform dat niet alleen hosts toepassingen gedistribueerde, maar ook gemakkelijker kunt levenscyclus van de afzonderlijk van de hostingprovider VM of een beperkte levensduur van de computer beheren.

## <a name="application-architecture"></a>Toepassingsarchitectuur

De architectuur van een Cloud Services-toepassing bevat meestal veel externe serviceafhankelijkheden zoals Service Bus, Azure tabel en Blob Storage, SQL, bestand Vgx., en anderen voor het beheren van de status en gegevens van een toepassing en de communicatie tussen Web en werknemer rollen in een Cloud Services-implementatie. Een voorbeeld van een volledige Cloud Services-toepassing uitzien als volgt:  

![Cloud Services-architectuur][9]

Stof service-toepassingen kunt ook de dezelfde externe services gebruiken in een volledige toepassing. Het volgende voorbeeld Cloud Services-architectuur is het eenvoudigste migratiepad van Cloudservices naar Service stof alleen de Cloud Services-implementatie vervangen door een Service stof-toepassing, de algehele architectuur hetzelfde blijft. Het Web en werknemer rollen worden overgezet naar Service stateless configuratieservices met minimale codewijzigingen.

![Service stof architectuur na eenvoudige migratie][10]

In dit stadium blijven het systeem werken op dezelfde manier als voordat. Profiteert van de Service stof statuscontrole functies, externe staat winkels kan worden internalized zoals stateful services indien van toepassing. Dit is meer betrokken dan een eenvoudige migratie van Web en werknemer rollen naar Service stateless configuratieservices, zoals deze is vereist voor het schrijven van aangepaste services die dezelfde functionaliteit in uw toepassing bieden, zoals de externe services gedaan. De voordelen hiervan zijn onder andere: 

 - Externe afhankelijkheden verwijderen 
 - De implementatie, beheer en de upgrade modellen verenigen. 
 
Een voorbeeld van de resulterende architectuur van deze services internalizing kan er als volgt uit:

![Service stof architectuur na de volledige migratie][11]

## <a name="communication-and-workflow"></a>Communicatie en de werkstroom

De meeste Cloud servicetoepassingen bestaan uit meer dan één laag. Daarnaast bestaat een toepassing voor de Service stof uit meer dan één service (meestal veel services). Twee veelvoorkomende communicatie-modellen zijn directe communicatie en via een externe duurzame opslag.

### <a name="direct-communication"></a>Rechtstreekse communicatie

Met directe communicatie communiceren lagen rechtstreeks via eindpunt die worden aangeboden door elke laag. In stateless omgevingen zoals Cloudservices, dit betekent dat een exemplaar van een rol VM een willekeurig selecteren of round-robin balance '-laden en rechtstreeks naar het eindpunt verbinding.

![Cloudservices rechtstreekse communicatie][5]

 Rechtstreekse communicatie is een algemene communicatiemodel in Service stof. Het belangrijkste verschil tussen Service stof en Cloud Services is die in Cloud Services u verbinding met een VM maakt, terwijl in Service stof u verbinding met een service maken. Dit is een belangrijk onderscheid om enkele redenen:

 - Services in Service stof zijn niet gekoppeld aan de VMs die als host Services mogelijk navigeren in het cluster en zelfs naar verwachting navigeren om verschillende redenen: Resource verdelen, failover-toepassing en de infrastructuur van upgrades en plaatsing of laden beperkingen. Dit betekent dat een exemplaar van de service-adres op elk gewenst moment kunt wijzigen. 
 - Een VM in Service stof kunt meerdere services, elk voorzien van een unieke eindpunten hosten.

Service stof biedt een service discovery om, de Naming Service, die kan worden gebruikt voor het oplossen van de adressen van de eindpunten van services genoemd. 

![Service stof directe communicatie][6]

### <a name="queues"></a>Wachtrijen

Een algemene om van de communicatie tussen lagen in stateless omgevingen zoals Cloud Services is een externe opslag-wachtrij gebruiken om op te slaan blijvend Werktaken van één niveau naar een andere. Een gebruikelijk is een weblaag die taken verzendt naar een Azure wachtrij of Service Bus waar werknemer rol exemplaren kunnen in wachtrij en de taken van proces.

![Cloud Services wachtrij communicatie][7]

Hetzelfde communicatiemodel kan worden gebruikt in de Service stof. Dit is handig bij het migreren van een bestaande Cloud Services-toepassing naar Service stof. 

![Service stof directe communicatie][8]
 
## <a name="next-steps"></a>Volgende stappen

De eenvoudigste migratiepad van Cloudservices naar Service stof is alleen de Cloud Services-implementatie vervangen door een Service stof-toepassing, blijft de algehele architectuur van uw toepassing ongeveer hetzelfde. Het volgende artikel bevat richtlijnen waarmee een Web of werknemer rol converteren naar een Service stof stateless service.

 - [Eenvoudige migratie: een Web of werknemer rol converteren naar een Service stof stateless-service](./service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
