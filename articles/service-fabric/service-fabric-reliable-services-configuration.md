<properties
   pageTitle="Overzicht van de configuratie betrouwbare configuratieservices Azure-Service | Microsoft Azure"
   description="Meer informatie over het configureren van statuscontrole betrouwbare Services in Azure-Service stof."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="sumukhs"/>

# <a name="configure-stateful-reliable-services"></a>Statuscontrole betrouwbare services configureren

Er zijn twee soorten configuratie-instellingen voor betrouwbare services. Één set is globale voor alle betrouwbare services in het cluster terwijl de andere set bij een bepaalde betrouwbare service hoort.

## <a name="global-configuration"></a>Globale configuratie

De configuratie van algemene betrouwbare service is opgegeven in het manifest cluster voor het cluster onder de sectie KtlLogger. Kan de configuratie van de gedeelde locatie en grootte plus de globale geheugenlimieten die worden gebruikt door het logboek. Het cluster manifest is één XML-bestand met instellingen en configuraties die van toepassing zijn op alle knooppunten en services in het cluster. Het bestand is meestal ClusterManifest.xml genoemd. U ziet het cluster manifest voor uw cluster met de powershell-opdracht Get-ServiceFabricClusterManifest.

### <a name="configuration-names"></a>Van configuratienamen

|Naam|Eenheid|Standaardwaarde|Opmerkingen|
|----|----|-------------|-------|
|WriteBufferMemoryPoolMinimumInKB|KB|8388608|Minimum aantal KB toewijzen in de kernelmodus voor het logboek schrijven buffer geheugen van toepassingen. Deze geheugen van toepassingen wordt gebruikt voor caching van gegevens voor de staat voordat schrijven naar schijf.|
|WriteBufferMemoryPoolMaximumInKB|KB|Geen limiet|Maximale grootte waaraan het logboek schrijven buffer geheugen te vergroten.|
|SharedLogId|GUID|""|Geeft een unieke GUID waarmee standaard gedeelde logboekbestand die worden gebruikt door alle betrouwbare services op alle knooppunten in het cluster die de SharedLogId geen in hun specifieke service te configureren opgeeft. Als SharedLogId is opgegeven, moet klikt u vervolgens SharedLogPath ook worden opgegeven.|
|SharedLogPath|De naam van het volledige pad|""|Hiermee geeft u het volledige pad waar het gedeelde logboekbestand gebruikt door alle betrouwbare services op alle knooppunten in het cluster die de SharedLogPath geen in hun specifieke service te configureren opgeeft. Echter als SharedLogPath is opgegeven, moet klikt u vervolgens SharedLogId ook worden opgegeven.|
|SharedLogSizeInMB|Megabytes|8192|Hiermee geeft u het aantal MB schijfruimte statisch toewijzen voor het gedeelde logboek. De waarde moet liggen 2048 of groter.|

### <a name="sample-cluster-manifest-section"></a>Voorbeeld cluster manifest sectie
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Opmerkingen
Het logboek heeft een globale groep geheugen van niet-verwisselbaar kernelgeheugen die beschikbaar is voor alle betrouwbare services op een knooppunt voor caching van gegevens voor de staat voordat naar de speciale log worden geschreven die is gekoppeld aan de replica betrouwbare service toegewezen. De groepsgrootte wordt bepaald door de instellingen voor WriteBufferMemoryPoolMinimumInKB en WriteBufferMemoryPoolMaximumInKB. WriteBufferMemoryPoolMinimumInKB Hiermee geeft u de oorspronkelijke grootte van deze geheugengroep zowel de laagste grootte waaraan het geheugen mogelijk verkleinen. WriteBufferMemoryPoolMaximumInKB is de hoogste grootte waaraan de geheugengroep groter mogelijk gemaakt. Elke betrouwbare service replica die wordt geopend mogelijk de grootte van de geheugengroep vergroten door een systeem bepaald bedrag maximaal WriteBufferMemoryPoolMaximumInKB. Als er meer van de aanvraag voor geheugen uit de geheugengroep dan beschikbaar is, worden verzoeken om geheugen vertraagd totdat geheugen beschikbaar is. Daarom als de toepassingen schrijven buffer geheugen te klein is voor een bepaalde configuratie vervolgens prestaties nadelig beïnvloeden.

De instellingen voor SharedLogId en SharedLogPath worden altijd samen gebruikt om te bepalen de GUID en de locatie voor het gedeelde standaardlogboek voor alle knooppunten in het cluster. De standaard gedeelde log wordt gebruikt voor alle betrouwbare services die de instellingen in de settings.xml voor de specifieke service geen opgeeft. Voor de beste prestaties kunnen gedeelde logboekbestanden moeten worden teruggezet op schijven die worden gebruikt voor het gedeelde logboekbestand conflict verkleinen.

SharedLogSizeInMB Hiermee geeft u de hoeveelheid schijfruimte toe te wijzen voor het gedeelde standaardlogboek op alle knooppunten.  SharedLogId en SharedLogPath hoeft niet te worden opgegeven in de volgorde voor SharedLogSizeInMB te worden opgegeven.


## <a name="service-specific-configuration"></a>Specifieke service te configureren
U kunt statuscontrole betrouwbare Services standaardconfiguraties wijzigen met behulp van de configuratiepakket (configuratie) of de service-implementatie (code).

+ **Config** - configuratie via het config-pakket is uitgevoerd door te wijzigen van het Settings.xml-bestand dat in de hoofdmap van het pakket Microsoft Visual Studio onder de map Config voor elke service in de toepassing wordt gegenereerd.
+ **Code** - configuratie via programmacode wordt uitgevoerd door te maken van een ReliableStateManager met behulp van een ReliableStateManagerConfiguration-object met het instellen van de gewenste opties.

Standaard de runtime Azure-Service stof Hiermee wordt gezocht naar de vooraf gedefinieerde sectienamen in het bestand Settings.xml en de configuratiewaarden verbruikt tijdens het maken van de onderliggende runtimeonderdelen.

>[AZURE.NOTE] Verwijder de **niet** de sectienamen van de volgende configuraties in de Settings.xml bestand dat in de Visual Studio-oplossing gegenereerd, tenzij u van plan bent voor het configureren van uw service via programmacode.
De naam van de zoekconfiguratie pakket of nieuwe sectie namen wijzigen vereist een codewijziging tijdens het configureren van de ReliableStateManager.


### <a name="replicator-security-configuration"></a>Configuratie van replicatie-beveiliging
Replicatie beveiligingsconfiguraties worden gebruikt voor het communicatiekanaal die wordt gebruikt tijdens de replicatie secure. Dit betekent dat services niet kan zien elkaars replicatie-verkeer is toegestaan worden, ervoor te zorgen dat de gegevens die bestaat uit ten zeerste beschikbaar ook beveiligd is. Standaard wordt een sectie van de configuratie van lege beveiliging voorkomen dat replicatie beveiliging.

### <a name="default-section-name"></a>De naam van de sectie standaard
ReplicatorSecurityConfig

>[AZURE.NOTE] Deze de sectienaam wilt wijzigen, vervangt u de replicatorSecuritySectionName-parameter voor de constructor ReliableStateManagerConfiguration bij het maken van de ReliableStateManager voor deze service.


### <a name="replicator-configuration"></a>Replicatie-configuratie
Replicatie configuraties configureren replicatie die verantwoordelijk is voor de status van de statuscontrole betrouwbare Service uiterst betrouwbaar door repliceren en de stand lokaal persistent te maken.
De standaardconfiguratie wordt gegenereerd door de Visual Studio-sjabloon en doorgaans voldoende. In dit gedeelte moment spreekt aanvullende configuraties die beschikbaar zijn voor replicatie af te stellen.

### <a name="default-section-name"></a>De naam van de sectie standaard
ReplicatorConfig

>[AZURE.NOTE] Deze de sectienaam wilt wijzigen, vervangt u de replicatorSettingsSectionName-parameter voor de constructor ReliableStateManagerConfiguration bij het maken van de ReliableStateManager voor deze service.


### <a name="configuration-names"></a>Van configuratienamen
|Naam|Eenheid|Standaardwaarde|Opmerkingen|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Seconden|0.015|De periode waarvoor de distributeur op de secundaire wacht nadat u hebt ontvangen van een bewerking alvorens een bevestiging terug naar de primaire. Een andere bevestigingen wilt sturen voor bewerkingen verwerkt binnen dit interval worden verzonden als één antwoord.|
|ReplicatorEndpoint|N/B|Geen standaard--vereiste parameter|IP-adres en poort dat primaire/secundaire replicatie wordt gebruikt om te communiceren met andere replicators in de replica ingesteld. Dit moet verwijzen naar een resource TCP-eindpunt in het servicemanifest. Raadpleeg [Service manifest bronnen](service-fabric-service-manifest-resources.md) voor meer informatie over het definiëren van eindpunt resources in een servicemanifest. |
|MaxPrimaryReplicationQueueSize|Aantal bewerkingen|8192|Maximum aantal bewerkingen in de primaire wachtrij. Een bewerking is vrijgemaakt nadat primaire replicatie een bevestiging van de secundaire replicators ontvangt. Deze waarde moet groter zijn dan 64 en een macht van 2.|
|MaxSecondaryReplicationQueueSize|Aantal bewerkingen|16384|Maximum aantal bewerkingen in de secundaire wachtrij. Een bewerking is vrijgemaakt na de staat ten zeerste beschikbaar tot en met permanente maken. Deze waarde moet groter zijn dan 64 en een macht van 2.|
|CheckpointThresholdInMB|MB|50|De hoeveelheid ruimte in logboekbestand waarna de status gecontroleerd wordt.|
|MaxRecordSizeInKB|KB|1024|Record maximumgrootte replicatie in het logboek schrijven mogelijk. Deze waarde moet een veelvoud is van 4 en groter is dan 16.|
|MinLogSizeInMB|MB|0 (systeem bepaald)|Minimale grootte van het logboek transacties. Het logboek worden niet kunnen afkappen maken onder deze instelling. 0, geeft aan dat replicatie de minimale grootte bepaalt. Deze waarde verhoogt, wordt de mogelijkheid van gedeeltelijke kopieën en incrementele back-ups sinds kans groter dat relevante logboekrecords worden afgebroken wordt verlaagd doen.|
|TruncationThresholdFactor|Factor|2|Bepaalt op welke grootte van het logboek afgekapt wordt geactiveerd. Drempelwaarde voor PST-bestanden wordt bepaald door MinLogSizeInMB TruncationThresholdFactor vermenigvuldigd. TruncationThresholdFactor moet groter zijn dan 1. MinLogSizeInMB * TruncationThresholdFactor moet kleiner zijn dan MaxStreamSizeInMB.|
|ThrottlingThresholdFactor|Factor|4|Bepaalt welke grootte van het logboek, de replica wordt gestart wordt vertraagd. Bandbreedteregeling drempel (in MB) wordt bepaald door Max ((MinLogSizeInMB *ThrottlingThresholdFactor),(CheckpointThresholdInMB* ThrottlingThresholdFactor)). Bandbreedteregeling drempel (in MB) moet groter zijn dan de drempelwaarde voor PST-bestanden (in MB). Drempelwaarde voor PST-bestanden (in MB) moet kleiner dan MaxStreamSizeInMB.|
|MaxAccumulatedBackupLogSizeInMB|MB|800|Max cumulatief in de grootte van de back-Logboeken in een bepaalde back-logboekketen (in MB). Een incrementele back-up aanvragen loopt vast als de incrementele back-up gegenereerd door een back-logboek waardoor de samengevoegde back-logboeken sinds de relevante volledige back-up groter zijn dan deze grootte. In dat geval is gebruiker vereist voor het uitvoeren van een volledige back-up.|
|SharedLogId|GUID|""|Geeft een unieke GUID waarmee het gedeelde logboekbestand gebruikt met deze replica. Meestal moeten services niet gebruiken voor deze instelling. Echter als SharedLogId is opgegeven, moet klikt u vervolgens SharedLogPath ook worden opgegeven.|
|SharedLogPath|De naam van het volledige pad|""|Hiermee geeft u het volledige pad waar het gedeelde logboekbestand voor deze replica worden gemaakt. Meestal moeten services niet gebruiken voor deze instelling. Echter als SharedLogPath is opgegeven, moet klikt u vervolgens SharedLogId ook worden opgegeven.|
|SlowApiMonitoringDuration|Seconden|300|Hiermee stelt u de controle interval voor beheerde API-oproepen. Voorbeeld: de gebruiker opgegeven back-up terugbellen functie. Nadat het interval is verstreken, een waarschuwing systeemstatus rapport wordt verzonden naar de status Manager.|

### <a name="sample-configuration-via-code"></a>Configuratie van de steekproef via programmacode
```csharp
class Program
{
    /// <summary>
    /// This is the entry point of the service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>Voorbeeld van configuratiebestand
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```


### <a name="remarks"></a>Opmerkingen
BatchAcknowledgementInterval besturingselementen herhaling latentie. Een waarde van '0' levert de laagste mogelijke latentie, maar oplevert doorvoer (zoals meer bevestigingsberichten moeten worden verzonden en verwerkt, elk met minder bevestigingen).
Des te groter de waarde voor BatchAcknowledgementInterval, het sneller de algehele herhaling worden verwerkt, maar een hogere bewerking latentie oplevert. Dit equivalent rechtstreeks naar de latentie van transactiebevestigingen.

De waarde voor CheckpointThresholdInMB Hiermee stelt u de hoeveelheid schijfruimte die replicatie gebruiken kunt voor de opslag van informatie over de status in van de replica speciale logboekbestand. Met groter wordende dit op een hogere waarde dan het standaardgeluid kan leiden tot nieuwe configuratie te versnellen wanneer een nieuwe replica wordt toegevoegd aan de set. Dit is vanwege de overdracht van gedeeltelijke staat die uitgevoerd vanwege de beschikbaarheid van meer geschiedenis van bewerkingen in het logboek wordt. Dit meer hersteltijd van een replicaset potentieel na vastlopen.

De instelling MaxRecordSizeInKB Hiermee definieert u de maximale grootte van een record die in het logboekbestand door replicatie kan worden opgeslagen. De standaardgrootte voor 1024 KB record is in de meeste gevallen optimaal. Echter als de service groter gegevensitems veroorzaakt wilt toevoegen aan de statusinformatie, deze waarde moet mogelijk worden verhoogd. Er is weinig voordeel bij het maken van MaxRecordSizeInKB kleiner is dan 1024, omdat voor kleinere records alleen de benodigde ruimte voor de kleinere record worden gebruikt. We verwachten dat deze waarde moet worden gewijzigd in alleen uitzonderlijke gevallen.

De instellingen voor SharedLogId en SharedLogPath worden altijd samen gebruikt om een service een apart gedeelde logboek uit het gedeelde standaardlogboek gebruiken voor het knooppunt. Zo veel services mogelijk moeten hetzelfde gedeelde logboek opgeven voor een optimale efficiency. Gedeelde logboekbestanden moeten worden teruggezet op schijven die worden gebruikt voor het gedeelde logboekbestand bewegingen conflict verkleinen. We verwachten dat deze waarde moet worden gewijzigd in alleen uitzonderlijke gevallen.

## <a name="next-steps"></a>Volgende stappen
 - [Fouten opsporen in uw toepassing Service stof in Visual Studio](service-fabric-debugging-your-application.md)
 - [Naslaginformatie voor ontwikkelaars voor betrouwbare Services](https://msdn.microsoft.com/library/azure/dn706529.aspx)
