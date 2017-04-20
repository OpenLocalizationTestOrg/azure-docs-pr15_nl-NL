<properties
   pageTitle="Statuscontrole in Service stof | Microsoft Azure"
   description="Een inleiding tot het model, waarmee de cmdlets voor controle van het cluster en bijbehorende toepassingen en services controle van de Azure-Service stof gezondheid."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="introduction-to-service-fabric-health-monitoring"></a>Inleiding tot de Service stof statuscontrole
Azure-Service stof maakt u kennis met een systeemstatus-model dat uitgebreide, flexibele en extensible systeemstatus evaluatie en rapportage biedt. Het model kunt dicht bij de realtime cmdlets voor controle van de status van het cluster en de services die erin wordt uitgevoerd. U kunt eenvoudig verkrijgen over gezondheid en corrigeren mogelijke problemen met voordat ze trapsgewijs en enorme bijvoorbeeld veroorzaken. In het normale model, services verzenden rapporten op basis van hun lokale weergaven en dat informatie wordt samengevoegd zodat een algemene cluster niveau weergave.

Service stof onderdelen gebruiken dit statusmodel uitgebreide rapporteren hun huidige status. U kunt de methode rapport status van uw toepassingen. Als u investeren in kwalitatief hoogwaardig status rapporteren die uw aangepaste voorwaarden weergeeft, kunt u detecteren en oplossen van problemen voor uw actieve toepassing veel gemakkelijker.

> [AZURE.NOTE] We begonnen met het subsysteem systeemstatus om een nodig voor gecontroleerde upgrades op te lossen. Service stof beschikt over een gecontroleerde-toepassing en cluster upgrades die volledig beschikbaarheid, geen downtime en zo min mogelijk naar zonder tussenkomst. Als u wilt bereiken van deze doelstellingen, controleert de upgrade gezondheid op basis van de upgrade beleid geconfigureerd en kunt een upgrade naar alleen wanneer systeemstatus houdt rekening gewenste drempelwaarden gaan. Anders wordt de upgrade automatisch ongedaan of onderbroken zodat beheerders kunnen de problemen oplossen. Zie voor meer informatie over de toepassingsupgrades, [in dit artikel](service-fabric-application-upgrade.md).

## <a name="health-store"></a>Status opslaan
De systeemstatus store blijft systeemstatus-gerelateerde gegevens over entiteiten in het cluster voor gemakkelijk kunt ophalen en evaluatie. Dit is geïmplementeerd als een Service stof behouden gebleven statuscontrole service om ervoor te zorgen beschikbaarheid en schaalbaarheid. De status store maakt deel uit van de **stof: / systeem** -toepassing, waarna ze is beschikbaar als het cluster en wordt uitgevoerd.

## <a name="health-entities-and-hierarchy"></a>Servicestatus entiteiten en hiërarchie
De status entiteiten worden ingedeeld in een logische hiërarchie die wordt vastgelegd interacties en afhankelijkheden tussen verschillende eenheden. De entiteiten en hiërarchie worden automatisch gemaakt door de systeemstatus winkel op basis van rapporten hebt ontvangen van Service stof onderdelen.

De status entiteiten gespiegelde de Service stof entiteiten. (Bijvoorbeeld **systeemstatus Toepassingsentiteit** komt overeen met de instantie van een toepassing geïmplementeerd in het cluster, terwijl de **systeemstatus knooppunt entiteit** overeenkomt met het knooppunt van een Service stof cluster.) De hiërarchie status wordt vastgelegd voor de interactie van de systeementiteiten en is de basis voor geavanceerde systeemstatus evaluatie. U kunt meer informatie over belangrijke Service stof concepten in de [Service stof technisch overzicht](service-fabric-technical-overview.md). Zie voor meer informatie over de toepassing, [Service stof toepassingsmodel](service-fabric-application-model.md).

De status entiteiten en de hiërarchie toestaan de cluster en toepassingen effectief gerapporteerd, foutopsporing, en bewaakt. Het model systeemstatus biedt nauwkeurige, *gedetailleerde* weergave van de status van de vele bewegende onderdelen in het cluster.

![Servicestatus diensten.][1]
De status entiteiten, ingedeeld in een hiërarchie op basis van bovenliggende en onderliggende indicatoren weergeven.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

De status entiteiten zijn:

- **Cluster**. Hiermee geeft u de status van een Service stof cluster. Statusrapporten cluster beschrijven voorwaarden die van invloed zijn op het hele cluster en kunnen niet worden beperkt naar een of meer beschadigd kinderen. Voorbeelden hiervan zijn de hersenen van het cluster splitsen vanwege netwerkproblemen partitioneren of communicatie.

- **Knooppunt**. Hiermee geeft u de status van Service stof knooppunt. Statusrapporten knooppunt beschrijven voorwaarden die betrekking hebben op de functionaliteit voor knooppunt. Deze meestal invloed op alle geïmplementeerd entiteiten u erop. Voorbeelden hiervan zijn wanneer een knooppunt buiten schijfruimte (of een andere computer hele eigenschap, zoals geheugen, verbindingen) is, waarbij een knooppunt niet beschikbaar is. Het knooppunt entiteit wordt aangegeven door de naam van het knooppunt (tekenreeks).

- **Toepassing**. Hiermee geeft u de status van de instantie van een toepassing in het cluster wordt uitgevoerd. Statusrapporten van de toepassing te zien welke voorwaarden die betrekking hebben op de algemene status van de toepassing. Ze kunnen niet worden beperkt afzonderlijke kinderen (services of gebruikte toepassingen). Voorbeelden hiervan zijn de end-to-end-interactie tussen de verschillende services in de toepassing. De toepassing entity wordt aangegeven door de naam van de toepassing (URI).

- **Service**. Hiermee geeft u de status van een service die in het cluster wordt uitgevoerd. Service-statusrapporten beschrijven voorwaarden die betrekking hebben op de algemene status van de service en kan niet worden beperkt naar een partition of een replica. Voorbeelden hiervan zijn een service te configureren (zoals poort of externe bestandsshare) dat wordt veroorzaakt door problemen voor alle partities. De service-entiteit wordt aangegeven door de naam van de service (URI).

- **Partition**. Hiermee geeft u de status van een service partition. Partition statusrapporten beschrijven voorwaarden die betrekking hebben op de hele replicaset. Voorbeelden hiervan zijn wanneer het aantal replica's lager dan het aantal doel is en wanneer een partition in quorum verlies. De entiteit partition wordt aangeduid met het partition-ID (GUID).

- **Replica**. Hiermee geeft u de status van een replica statuscontrole service of een exemplaar van stateless service. De kleinste eenheid die watchdogs en de onderdelen kunnen rapporteren voor een toepassing. Voor statuscontrole services, voorbeelden hiervan zijn een primaire replica rapportage wanneer deze bewerkingen naar secundaire servers en wanneer replicatie is niet verder gaat op de verwachte kan repliceren tempo. Een stateless exemplaar kunt ook melden als deze heeft bijna geen resources of verbindingsproblemen heeft. De entiteit replica wordt aangeduid met het partition-ID (GUID) en de replica of exemplaar-ID (lang).

- **DeployedApplication**. Hiermee geeft u de status van een *toepassing wordt uitgevoerd op een knooppunt*. Statusrapporten gedistribueerde toepassing beschrijven voorwaarden die specifiek zijn voor de toepassing op het knooppunt dat niet kan worden beperkt naar servicepakketten geïmplementeerd op hetzelfde knooppunt. Voorbeelden hiervan zijn wanneer het toepassingspakket kan niet worden gedownload op dat knooppunt en wanneer er een probleem bij het instellen van de toepassing beveiliging principes op het knooppunt. De gedistribueerde toepassing wordt aangegeven door de naam van de toepassing (URI) en het knooppunt opgeeft (tekenreeks).

- **DeployedServicePackage**. Hiermee geeft u de status van een servicepakket uitgevoerd op een knooppunt in het cluster. Hierin wordt een servicepakket-specifieke voorwaarden die hebben geen invloed op de andere servicepakketten op hetzelfde knooppunt voor dezelfde toepassing. Voorbeelden hiervan zijn een pakket code in de servicepakket dat niet kan worden gestart en een configuratiepakket dat kan worden gelezen. Het pakket geïmplementeerd service wordt aangeduid met toepassingsnaam (URI), (tekenreeks) de knooppuntnaam van het en manifest servicenaam (tekenreeks).

De granulatie van het model systeemstatus kunt u heel gemakkelijk opsporen en corrigeren van problemen. Bijvoorbeeld als een service niet reageert, is het mogelijk gecombineerd rapporteren dat exemplaar van de toepassing beschadigd is. Rapportage op dat niveau niet ideaal, echter is, omdat het probleem zonder alle services binnen die toepassing dat mogelijk. Het rapport moet worden toegepast op de beschadigde service of naar een specifieke onderliggende partition, als u meer informatie verwijst naar die partition. De gegevens automatisch itemweergavesjabloon geeft door de hiërarchie en een beschadigd partition is zichtbaar gemaakt op diensten- en niveaus. Deze aggregatie kunt identificeren en de oorzaak van het probleem sneller oplossen.

De hiërarchie van de servicestatus is samengestelde van bovenliggende en onderliggende indicatoren weergeven. Een cluster bestaat uit knooppunten en toepassingen. Toepassingen services hebben en toepassingen geïmplementeerd. Gebruikte toepassingen hebt servicepakketten geïmplementeerd. Services hebt partities, en elke partition heeft een of meer replica. Er is een speciale relatie tussen knooppunten en geïmplementeerd entiteiten. Als een knooppunt is beschadigd volgens de systeemonderdeel certificeringsinstantie (Failover Management service), heeft dit gevolgen voor de gebruikte toepassingen, servicepakketten en replica's die zijn geïmplementeerd op is geïnstalleerd.

De hiërarchie systeemstatus voorstelt de meest recente status van het systeem op basis van de meest recente statusrapporten, namelijk bijna realtime gegevens.
Interne en externe watchdogs kunnen melden op dezelfde diensten op basis van toepassingsspecifieke logica of aangepaste gecontroleerde voorwaarden. Gebruikersrapporten naast de systeemrapporten.

Plan te investeren in het rapport en reageren op servicestatus tijdens het ontwerp van een grote cloudservice, om de service gemakkelijker te foutopsporing, bewaken en gebruiken.

## <a name="health-states"></a>Statussen
Service stof drie statussen gebruikt om te beschrijven of een entiteit al dan niet correct is: OK, waarschuwingen en fouten. Elk rapport verzonden naar de store systeemstatus moet een van deze Staten opgeven. Het resultaat van de evaluatie servicestatus is een van deze Staten.

De [status Staten](https://msdn.microsoft.com/library/azure/system.fabric.health.healthstate) kan zijn:

- **OK**. De entiteit in orde is. Er zijn geen bekende problemen voor deze of de onderliggende (indien van toepassing) gerapporteerd.

- **Waarschuwing**. De entiteit ervaringen enkele problemen, maar het is niet nog beschadigd (bijvoorbeeld er vertragingen, maar ze niet opnieuw functionele problemen nog). In sommige gevallen, de waarschuwingsvoorwaarde mogelijk zelf oplossen zonder tussenkomst speciale en is nuttig om te leveren zicht op wat er gebeurt. De waarschuwingsvoorwaarde kan in andere gevallen afnemen een ernstige probleem zonder tussenkomst van gebruikers.

- **Fout**. De entiteit is beschadigd. Actie moet worden ondernomen om op te lossen de status van de entiteit, omdat deze niet naar behoren.

- **Onbekend**. De entiteit bestaat niet in de winkel status. Dit resultaat kan worden opgehaald uit de gedistribueerde query's die worden samengevoegd resultaten van meerdere onderdelen. Bijvoorbeeld opvragen knooppunt lijst Hiermee gaat u naar **FailoverManager** en **HealthManager**, get-toepassing lijstquery Hiermee gaat u naar **ClusterManager** en **HealthManager**. Deze query's samenvoegen resultaten van meerdere onderdelen. Als u een ander systeemonderdeel geeft als resultaat een entiteit die niet heeft gekregen of is opgeruimd uit het archief status, het samengevoegde resultaat onbekend heeft status.

## <a name="health-policies"></a>Statusbeleidsregels voor
De status store geldt systeemstatus beleidsregels om te bepalen of een entiteit in orde gebaseerd op de rapporten en de onderliggende sites is.

> [AZURE.NOTE] Statusbeleidsregels voor kunnen worden opgegeven in het manifest cluster (voor evaluatie van de servicestatus cluster en knooppunt) of in een manifest voor de toepassing (voor evaluatie van de toepassing en een van de onderliggende). Servicestatus evaluatie aanvragen kunnen ook aangepaste status evaluatie beleidsregels, die wordt gebruikt alleen voor deze evaluatie doorgeven.

Standaard geldt Service stof strikte regels (Alles moet zijn in orde) voor de bovenliggende en onderliggende hiërarchische relatie. Als een van de onderliggende items één beschadigd gebeurtenis heeft, de bovenliggende wordt beschouwd als beschadigd.

### <a name="cluster-health-policy"></a>Cluster beleid
Het [beleid cluster](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.aspx) wordt gebruikt om de status van cluster en knooppunt statussen te evalueren. Het beleid kan worden gedefinieerd in het manifest cluster. Als niet aanwezig is, wordt het standaardbeleid (nul mag fouten) wordt gebruikt.
Het beleid van de servicestatus cluster bevat:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.considerwarningaserror.aspx). Hiermee geeft u aan of waarschuwing systeemstatus behandeld als fout tijdens de evaluatie van de servicestatus. Standaard: ONWAAR.

- [MaxPercentUnhealthyApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications.aspx). Hiermee geeft u het maximumpercentage mag van toepassingen die beschadigd worden kunnen voordat het cluster wordt beschouwd als fout.

- [MaxPercentUnhealthyNodes](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes.aspx). Hiermee geeft u het maximumpercentage mag van knooppunten dat beschadigd worden kan voordat het cluster wordt beschouwd als fout. In grote kolomgroepen sommige knooppunten zijn altijd naar beneden- of uitzoomen voor reparaties, zodat dit percentage moet worden geconfigureerd dat zonder.

- [ApplicationTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap.aspx). De toepassing type systeemstatus beleid toewijzing kan worden gebruikt tijdens de evaluatie van de servicestatus cluster om te beschrijven speciale toepassingstypen. Standaard zijn alle toepassingen in een groep plaatsen en die wordt geëvalueerd met MaxPercentUnhealthyApplications. Als u sommige toepassingstypen moeten anders worden behandeld, kunnen worden gemaakt uit de algemene groep. In plaats daarvan worden ze geëvalueerd ten opzichte van de percentages dat is gekoppeld aan de naam van het type toepassing in de toewijzing. Bijvoorbeeld in een cluster er duizenden toepassingen van verschillende typen, en een paar besturingselement toepassingsexemplaren van een speciaal toepassingstype. De control-toepassingen mogen nooit aan fout. Globale MaxPercentUnhealthyApplications 20% voor gemiddeld in sommige gevallen, maar voor het toepassingstype "ControlApplicationType" de MaxPercentUnhealthyApplications ingesteld op 0, kunt u opgeven. Op deze manier als enkele van de vele toepassingen beschadigd zijn, maar onder het globale beschadigd percentage, zou het cluster worden geëvalueerd als waarschuwing. Upgraden van cluster heeft geen gevolgen voor de status van een waarschuwing of andere monitoring geactiveerd door de status van de fout. Maar zelfs één besturingselement-toepassing in een fout maakt cluster beschadigd en waarin de optie Vorig activeert of onderbreekt u de upgrade cluster, afhankelijk van de upgrade configuratie.
Voor de toepassingstypen gedefinieerd in de map, worden alle toepassingsexemplaren uit de algemene groep van toepassingen genomen. Ze worden geëvalueerd op basis van het totale aantal toepassingen van het type van toepassing, met de specifieke MaxPercentUnhealthyApplications van de kaart. De rest van de toepassingen in de algemene groep blijven en met MaxPercentUnhealthyApplications worden geëvalueerd.

Het volgende voorbeeld is een uittreksel uit een manifest cluster. Als u wilt posten in de toepassing type kaart definieert, aanduiding voor de parameternaam van de met 'ApplicationTypeMaxPercentUnhealthyApplications-", gevolgd door de naam van het type toepassing.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Toepassingenbeleid
Het [beleid van toepassing-status](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.aspx) wordt beschreven hoe de evaluatie van evenementen en onderliggende Staten aggregatie klaar is voor toepassingen en onderliggende items. Dit kan worden gedefinieerd in de toepassingsmanifest, **ApplicationManifest.xml**, in de toepassingspakket. Als er geen beleidsregels zijn opgegeven, Service stof wordt ervan uitgegaan dat de entiteit beschadigd is als er een statusrapport of een onderliggend bij de status waarschuwing of fout.
De configureerbare beleidsregels zijn:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.considerwarningaserror.aspx). Hiermee geeft u aan of waarschuwing systeemstatus behandeld als fout tijdens de evaluatie van de servicestatus. Standaard: ONWAAR.

- [MaxPercentUnhealthyDeployedApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications.aspx). Hiermee geeft u het maximumpercentage mag van gebruikte toepassingen die beschadigd worden kunnen voordat de toepassing wordt beschouwd als fout. Dit percentage wordt berekend door het aantal beschadigde geïmplementeerd toepassingen delen via het aantal knooppunten die de toepassingen worden momenteel geïmplementeerd op in het cluster. De berekening Rondt naar boven voor één storing op kleine hoeveelheden knooppunten gemiddeld. Standaard percentage: nul.

- [DefaultServiceTypeHealthPolicy](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy.aspx). Hiermee geeft u de service type systeemstatus standaardbeleid, die het standaardbeleid uit servicestatus voor alle servicetypen in de toepassing vervangt.

- [ServiceTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap.aspx). Geeft een overzicht van de service systeemstatus beleidsregels per servicetype. Dit beleid vervangt de service type systeemstatus standaardbeleidsregels voor elk type opgegeven service. Bijvoorbeeld als een toepassing een servicetype stateless gateway en een servicetype statuscontrole engine heeft, kunt u het beleid servicestatus voor hun evaluatie anders. Wanneer u beleid per servicetype opgeeft, kunt u meer gedetailleerde controle over de status van de service toegang.

### <a name="service-type-health-policy"></a>Service type beleid
Het [beleid type service](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.aspx) geeft aan hoe evalueren en aggregeren van de services en de onderliggende services. Het beleid bevat:

- [MaxPercentUnhealthyPartitionsPerService](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice.aspx). Hiermee geeft u het maximumpercentage mag van beschadigde partities voordat een service wordt beschouwd als beschadigd. Standaard percentage: nul.

- [MaxPercentUnhealthyReplicasPerPartition](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition.aspx). Hiermee geeft u het maximumpercentage mag beschadigd replica voordat een partition als beschadigd wordt beschouwd. Standaard percentage: nul.

- [MaxPercentUnhealthyServices](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices.aspx). Hiermee geeft u het maximumpercentage mag van beschadigde services voordat de toepassing wordt beschouwd als beschadigd. Standaard percentage: nul.

Het volgende voorbeeld is een uittreksel uit een toepassingsmanifest:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Servicestatus evaluatie
Gebruikers en geautomatiseerde services kunnen servicestatus voor een entiteit op elk gewenst moment evalueren. Als u wilt evalueren van een entiteit status, de systeemstatus store aggregaties alle gezondheid-rapporten op de entiteit en evalueert alle onderliggende items (indien van toepassing). De algoritme van de aggregatie systeemstatus gebruikt systeemstatus beleidsregels waarmee opgeven hoe statusrapporten wordt berekend en hoe u aggregeren onderliggende statussen (indien van toepassing).

### <a name="health-report-aggregation"></a>Status rapport aggregatie
Één entiteit kan meerdere statusrapporten worden verzonden door verschillende verslaggevers (onderdelen of watchdogs) op verschillende eigenschappen hebben. De aggregatie maakt gebruik van de bijbehorende statusbeleid, met name de ConsiderWarningAsError lid zijn van toepassing of cluster beleid. ConsiderWarningAsError geeft aan hoe waarschuwingen wordt berekend.

De samengevoegde status wordt door de *Slechtste* statusrapporten op de entiteit geactiveerd. Als er ten minste één fout statusrapport, wordt de samengevoegde status een fout.

![Status rapport aggregatie met foutrapport.][2]

Een systeemstatus entiteit met een of meer fout statusrapporten wordt geëvalueerd als fout. Dit geldt voor een statusrapport verlopen, ongeacht de status.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Als er geen foutenrapporten en een of meer waarschuwingen, is de geaggregeerde status waarschuwing of fout, afhankelijk van de vlag ConsiderWarningAsError-beleid.

![Status rapport aggregatie met waarschuwing rapport en ConsiderWarningAsError ONWAAR.][3]

Status rapport aggregatie met waarschuwing rapport en ConsiderWarningAsError ingesteld op ONWAAR (standaard).

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Onderliggende systeemstatus aggregatie
De samengevoegde status van een entiteit weerspiegelt de onderliggende statussen (indien van toepassing). De algoritme voor het verzamelen van onderliggende statussen maakt gebruik van de servicestatus beleidsregels van toepassing op basis van het entiteitstype.

![Onderliggende entiteiten systeemstatus aggregatie.][4]

Onderliggende aggregatie op basis van beleidsregels voor de servicestatus.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Nadat de systeemstatus store alle onderliggende items te evalueren heeft, verzamelt deze hun statussen op basis van de geconfigureerde maximumpercentage webonderdeelmenu voor een beschadigd. Dit percentage wordt opgehaald uit het beleid op basis van het type entiteit en onderliggende.

- Als alle onderliggende OK Staten, is de status van de onderliggende samengevoegd OK.

- Als kinderen OK en waarschuwing Staten hebt, wordt de status van de onderliggende samengevoegd waarschuwing.

- Als er kinderen met fout Staten die bieden geen ondersteuning voor het percentage van de beschadigde kinderen toegestane maximum, wordt in de samengevoegde status een fout is.

- Als de onderliggende items met fout Staten houden aan de de maximaal toegestane percentage van beschadigde kinderen, is de geaggregeerde status waarschuwing.

## <a name="health-reporting"></a>Status rapporteren
Onderdelen van het systeem, systeem stof toepassingen en intern/extern watchdogs kunnen Service stof entiteiten rapporteren. De verslaggevers zorg *lokale* bepalingen van de status van de gecontroleerde entiteiten, op basis van de voorwaarden die ze wilt controleren. Ze niet nodig hebt om een globale staat of statistische gegevens te bekijken. Het gewenste gedrag is eenvoudige verslaggevers te houden en geen complexe organismen die moeten kijkt u naar veel zaken af te leiden welke gegevens om te verzenden.

Systeemstatus om gegevens te verzenden naar de store status, moet een reporter bepalen van de desbetreffende entiteit en een statusrapport maken. Het rapport kan vervolgens worden verzonden via de API met behulp van [FabricClient.HealthClient.ReportHealth](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient_members.aspx), via PowerShell, of REST.

### <a name="health-reports"></a>Statusrapporten
De [statusrapporten](https://msdn.microsoft.com/library/azure/system.fabric.health.healthreport.aspx) voor elk van de entiteiten in het cluster bevatten de volgende informatie:

- **Bron-id**. Een tekenreeks die de reporter van de gebeurtenis servicestatus deze identificeert.

- **Entiteit-id**. Geeft de entity wanneer het rapport wordt toegepast. Deze verschilt op basis van de [entiteit Typ](service-fabric-health-introduction.md#health-entities-and-hierarchy):

  - Cluster. Geen.

  - Knooppunt. Knooppuntnaam (tekenreeks).

  - Toepassing. Naam van de toepassing (URI). De naam van het exemplaar van de toepassing geïmplementeerd in het cluster vertegenwoordigt.

  - Service. Antwoordgroepservice naam (URI). De naam van de service-exemplaar geïmplementeerd in het cluster vertegenwoordigt.

  - Partition. Partition-ID (GUID). De unieke id van partition vertegenwoordigt.

  - Replica. De statuscontrole replica-ID of de stateless exemplaar-ID (INT64).

  - DeployedApplication. Naam van de toepassing (URI) en de knooppuntnaam (tekenreeks).

  - DeployedServicePackage. Naam van de toepassing (URI), (tekenreeks) de knooppuntnaam van het en service bestandenlijst naam (tekenreeks).

- **Eigenschap**. Een *tekenreeks* (niet een vaste opsomming) waarmee de reporter voor het categoriseren van de gebeurtenis servicestatus voor een specifieke eigenschap van de entiteit. Bijvoorbeeld kunt reporter A melden dat de status van de eigenschap Node01 "opslag" en reporter B de status van de eigenschap Node01 "connectivity" kunt melden. In het archief status, worden deze rapporten behandeld als afzonderlijke systeemstatus-gebeurtenissen voor de entiteit Node01.

- **Beschrijving**. Een tekenreeks waarmee een reporter naar bevatten gedetailleerde informatie over de status gebeurtenis. **Bron-id**, **eigenschap**en **HealthState** moeten u het rapport volledig beschrijven. De beschrijving van de leesbare informatie over het rapport toegevoegd. De tekst gemakkelijker voor beheerders en gebruikers voor meer informatie over het statusrapport.

- **HealthState**. Een [inventarisatie](service-fabric-health-introduction.md#health-states) waarmee de status van het rapport wordt beschreven. De geaccepteerde waarden zijn OK, waarschuwingen en fouten.

- **TimeToLive**. Een tijdspanne waarmee wordt aangegeven hoe lang het statusrapport geldig is. In combinatie met **RemoveWhenExpired**, kunt de servicestatus store verlopen gebeurtenissen evalueren. Standaard is de waarde oneindig en de lijst eeuwen geldig is.

- **RemoveWhenExpired**. Een Booleaanse waarde. Als ingesteld op waar, het statusrapport verlopen is automatisch verwijderd uit het archief gezondheid en het rapport niet van invloed zijn op entiteit systeemstatus evaluatie. Wanneer de lijst geldig gedurende een bepaalde alleen tijd is en de reporter niet moet expliciet ruimen gebruikt. Dit wordt ook gebruikt voor rapporten verwijderen uit het archief status (bijvoorbeeld een watchdog is gewijzigd en stopt met het verzenden van rapporten met vorige bron en eigenschappen). Een rapport met een kort TimeToLive samen met RemoveWhenExpired naar een vorige staat in de store systeemstatus ruimen kan verzenden. Als de waarde is ingesteld op ONWAAR, wordt het verlopen rapport behandeld als een fout op de evaluatie van de servicestatus. De waarde false signalen naar de status store waaraan de bron regelmatig op deze eigenschap moet rapporteren. Als dit niet het geval is, moet klikt u vervolgens er iets mis met het watchdog. De status van de watchdog wordt vastgelegd door te overwegen de gebeurtenis als een fout.

- **SequenceNumber**. Een positief geheel getal dat moet worden groeiende, staat dit voor de volgorde van de rapporten. Wordt gebruikt door de winkel systeemstatus verouderde rapporten die zijn ontvangen, laat vanwege vertragingen in het netwerk of andere problemen detecteren. Een rapport is geweigerd als het volgnummer is dat kleiner is dan of gelijk is aan de meest recent getal voor de dezelfde entiteit, de bron en de eigenschap toegepast. Als dit niet is opgegeven, wordt het volgnummer automatisch gegenereerd. Het is nodig om te zetten in het volgnummer alleen tijdens het rapporteren voor de staat overgangen. In dit geval moet de bron onthouden welke rapporten het verzonden en de gegevens voor herstel op failover behouden.

Deze vier onderdelen van gegevens--bron-id, entiteit-id, eigenschap en HealthState--zijn vereist voor elke statusrapport. De bron-id tekenreeks mag niet beginnen met het voorvoegsel "**systeem.**", dat is gereserveerd voor systeem rapporten. Voor dezelfde entiteit, moet u er slechts één rapport voor de dezelfde bron en de eigenschap is. Meerdere rapporten voor de bron en de eigenschap overschrijven elkaar, aan de clientzijde servicestatus (als ze de batch zijn verwerkt) of op de status kant opslaan. De vervangende is gebaseerd op de volgnummers; nieuwere rapporten (met hogere volgnummers) vervangen oudere rapporten.

### <a name="health-events"></a>Servicestatus gebeurtenissen
Intern, blijft de systeemstatus store [systeemstatus gebeurtenissen](https://msdn.microsoft.com/library/azure/system.fabric.health.healthevent.aspx), die de gegevens uit de rapporten en aanvullende metagegevens bevatten. De metagegevens bevat de tijd waarop die het rapport werd besteed aan de systeemstatus-client en de tijd die op de server is gewijzigd. De status gebeurtenissen worden geretourneerd door [systeemstatus query's](service-fabric-view-entities-aggregated-health.md#health-queries).

De toegevoegde metagegevens bevat:

- **SourceUtcTimestamp**. De tijd het rapport is besteed aan de systeemstatus-client (Coordinated Universal Time).

- **LastModifiedUtcTimestamp**. De tijd die het rapport het laatst is gewijzigd op de server (Coordinated Universal Time).

- **IsExpired**. Een vlag om aan te geven of het rapport wanneer de query is uitgevoerd door de winkel servicestatus is verlopen. Een gebeurtenis kan worden verlopen alleen als RemoveWhenExpired ONWAAR is. Anders de gebeurtenis niet wordt geretourneerd door de query en wordt verwijderd uit het archief.

- **LastOkTransitionAt**, **LastWarningTransitionAt**, **LastErrorTransitionAt**. De laatste keer voor OK/waarschuwing/fout overgangen. Deze velden geven de geschiedenis van de status staat overgangen voor de gebeurtenis.

De velden van de overgang staat kunnen worden gebruikt voor slimmer waarschuwingen of "historische" systeemstatus gebeurtenisinformatie. Ze mogelijk scenario's zoals:

- Melding wanneer een eigenschap bij waarschuwing/fout voor meer dan X minuten is. De voorwaarde voor een periode controleren vermijdt waarschuwingen van tijdelijke voorwaarden. Bijvoorbeeld een waarschuwing als de status heeft zijn waarschuwing gedurende meer dan vijf minuten in kan worden omgezet (HealthState == waarschuwing en nu - LastWarningTransitionTime > 5 minuten).

- Waarschuwing is alleen van voorwaarden die zijn gewijzigd in de afgelopen X minuten. Als u een rapport is al op Fout vóór de opgegeven tijd, kan worden genegeerd omdat dit al eerder is gesignaleerd.

- Als u een eigenschap is schakelen tussen waarschuwingen en fouten, moet u bepalen hoe lang deze is beschadigd (dat wil zeggen niet OK). Bijvoorbeeld een waarschuwing als de eigenschap niet correct gedurende meer dan vijf minuten is in kan worden omgezet (HealthState! = Ok en nu - LastOkTransitionTime > 5 minuten).

## <a name="example-report-and-evaluate-application-health"></a>Voorbeeld: Report en evalueren van de toepassingsstatus
Het volgende voorbeeld stuurt een statusrapport via PowerShell over de toepassing **stof: / WordCount** uit de bron **MyWatchdog**. Het statusrapport bevat informatie over de status eigenschap "beschikbaarheid" in systeemstatus foutstatus, met oneindig TimeToLive. Vervolgens hiervoor wordt de toepassingsstatus, welke resulteert samengevoegd systeemstatus staat fouten en de gebeurtenissen gerapporteerde servicestatus in de lijst met gebeurtenissen systeemstatus.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Gebruik van systeemstatus-modellen
Het model systeemstatus kunt cloudservices en de onderliggende Service stof-platform om wilt verkleinen, omdat voor controle en systeemstatus bepalingen zijn verdeeld over de verschillende monitoren binnen het cluster.
Andere systemen hebben een centrale service op het clusterniveau van waarop alle *mogelijk* nuttige informatie verstrekt door services parseert. Deze methode storend hun schaalbaarheid. Dit ook kan geen voor het verzamelen van zeer specifieke informatie waarmee problemen en mogelijke problemen als dicht bij de onderliggende oorzaak mogelijk identificeren.

Het model systeemstatus wordt intensief gebruikt voor controle en diagnose, voor het evalueren van cluster en toepassing gezondheid en voor gecontroleerde upgrades. Andere systeemstatus gegevens gebruikt om automatische repareren, cluster medische geschiedenis bouwen en actie-waarschuwingen op bepaalde voorwaarden voldoen.

## <a name="next-steps"></a>Volgende stappen
[Statusrapporten weergeven-Service stof](service-fabric-view-entities-aggregated-health.md)

[Statusrapporten systeem gebruiken voor probleemoplossing](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Hoe kunt melden en servicestatus controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Aangepaste Service stof statusrapporten toevoegen](service-fabric-report-health.md)

[Bewaken en diagnose stellen bij lokaal services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service stof toepassing upgrade](service-fabric-application-upgrade.md)
