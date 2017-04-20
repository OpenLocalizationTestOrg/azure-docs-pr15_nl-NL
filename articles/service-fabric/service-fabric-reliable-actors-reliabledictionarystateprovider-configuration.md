<properties
   pageTitle="Overzicht van de configuratie van Azure-Service stof betrouwbare betrokkenen ReliableDictionaryActorStateProvider | Microsoft Azure"
   description="Meer informatie over het configureren van Azure-Service stof statuscontrole betrokkenen van het type ReliableDictionaryActorStateProvider."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/18/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Betrouwbare betrokkenen--ReliableDictionaryActorStateProvider configureren
U kunt de standaardconfiguratie van ReliableDictionaryActorStateProvider wijzigen door te wijzigen van het settings.xml-bestand in de hoofdmap van de Visual Studio-pakket onder de map Config voor de opgegeven acteur gegenereerd.

De runtime Azure-Service stof Hiermee wordt gezocht naar de vooraf gedefinieerde sectienamen in het bestand settings.xml en gebruikt de configuratiewaarden bij het maken van de onderliggende runtimeonderdelen.

>[AZURE.NOTE] Kan **geen** verwijdert of wijzigt u de sectienamen van de volgende configuraties in het settings.xml-bestand dat in de Visual Studio-oplossing wordt gegenereerd.

Er zijn ook globale instellingen die betrekking hebben op de configuratie van ReliableDictionaryActorStateProvider.

## <a name="global-configuration"></a>Globale configuratie

De globale configuratie is opgegeven in het manifest cluster voor het cluster onder de sectie KtlLogger. Kan de configuratie van de gedeelde locatie en grootte plus de globale geheugenlimieten die worden gebruikt door het logboek. Houd er rekening mee dat wijzigingen in het manifest cluster van invloed zijn op alle services met ReliableDictionaryActorStateProvider en betrouwbare statuscontrole services.

Het cluster manifest is één XML-bestand met instellingen en configuraties die van toepassing zijn op alle knooppunten en services in het cluster. Het bestand is meestal ClusterManifest.xml genoemd. U ziet het cluster manifest voor uw cluster met de powershell-opdracht Get-ServiceFabricClusterManifest.

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
Het logboek heeft een globale groep geheugen van niet-verwisselbaar kernelgeheugen die beschikbaar is voor alle betrouwbare services op een knooppunt voor caching van gegevens voor de staat voordat naar de speciale log worden geschreven die is gekoppeld aan de replica betrouwbare service toegewezen. De groepsgrootte wordt bepaald door de instellingen voor WriteBufferMemoryPoolMinimumInKB en WriteBufferMemoryPoolMaximumInKB. WriteBufferMemoryPoolMinimumInKB Hiermee geeft u zowel het oorspronkelijke formaat van deze geheugengroep en de laagste waaraan het geheugen mogelijk verkleinen. WriteBufferMemoryPoolMaximumInKB is de hoogste grootte waaraan de geheugengroep groter mogelijk gemaakt. Elke betrouwbare service replica die wordt geopend mogelijk de grootte van de geheugengroep vergroten door een systeem bepaald bedrag maximaal WriteBufferMemoryPoolMaximumInKB. Als er meer van de aanvraag voor geheugen uit de geheugengroep dan beschikbaar is, worden verzoeken om geheugen vertraagd totdat geheugen beschikbaar is. Daarom als de toepassingen schrijven buffer geheugen te klein is voor een bepaalde configuratie vervolgens prestaties nadelig beïnvloeden.

De instellingen voor SharedLogId en SharedLogPath worden altijd samen gebruikt om te bepalen de GUID en de locatie voor het gedeelde standaardlogboek voor alle knooppunten in het cluster. De standaard gedeelde log wordt gebruikt voor alle betrouwbare services die de instellingen in de settings.xml voor de specifieke service geen opgeeft. Voor de beste prestaties kunnen gedeelde logboekbestanden moeten worden teruggezet op schijven die worden gebruikt voor het gedeelde logboekbestand conflict verkleinen.

SharedLogSizeInMB Hiermee geeft u de hoeveelheid schijfruimte toe te wijzen voor het gedeelde standaardlogboek op alle knooppunten.  SharedLogId en SharedLogPath hoeft niet te worden opgegeven in de volgorde voor SharedLogSizeInMB te worden opgegeven.

## <a name="replicator-security-configuration"></a>Configuratie van replicatie-beveiliging
Replicatie beveiligingsconfiguraties worden gebruikt voor het communicatiekanaal die wordt gebruikt tijdens de replicatie secure. Dit betekent dat services elkaars replicatie-verkeer is toegestaan, zorgen dat de gegevens die bestaat uit ten zeerste beschikbaar is ook secure niet kunnen zien.
Standaard wordt een sectie van de configuratie van lege beveiliging voorkomen dat replicatie beveiliging.

### <a name="section-name"></a>De naam van sectie
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replicatie-configuratie
Replicatie configuraties worden gebruikt voor het configureren van replicatie die verantwoordelijk is voor de status van de acteur staat serviceprovider uiterst betrouwbaar door repliceren en de stand lokaal persistent te maken.
De standaardconfiguratie wordt gegenereerd door de Visual Studio-sjabloon en doorgaans voldoende. In dit gedeelte moment spreekt aanvullende configuraties die beschikbaar zijn voor replicatie af te stellen.

### <a name="section-name"></a>De naam van sectie
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Van configuratienamen

|Naam|Eenheid|Standaardwaarde|Opmerkingen|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Seconden|0.015|De periode waarvoor de distributeur op de secundaire wacht nadat u hebt ontvangen van een bewerking alvorens een bevestiging terug naar de primaire. Een andere bevestigingen wilt sturen voor bewerkingen verwerkt binnen dit interval worden verzonden als één antwoord.||
|ReplicatorEndpoint|N/B|Geen standaard--vereiste parameter|IP-adres en poort dat primaire/secundaire replicatie wordt gebruikt om te communiceren met andere replicators in de replica ingesteld. Dit moet verwijzen naar een resource TCP-eindpunt in het servicemanifest. Raadpleeg [Service manifest bronnen](service-fabric-service-manifest-resources.md) voor meer informatie over het definiëren van eindpunt resources in de servicemanifest. |
|MaxReplicationMessageSize|Bytes|50 MB|Maximale grootte van replicatiegegevens die kan worden verzonden in één bericht.|
|MaxPrimaryReplicationQueueSize|Aantal bewerkingen|8192|Maximum aantal bewerkingen in de primaire wachtrij. Een bewerking is vrijgemaakt nadat de primaire distributeur een bevestiging van de secundaire replicators ontvangt. Deze waarde moet groter zijn dan 64 en een macht van 2.|
|MaxSecondaryReplicationQueueSize|Aantal bewerkingen|16384|Maximum aantal bewerkingen in de secundaire wachtrij. Een bewerking is vrijgemaakt na de staat ten zeerste beschikbaar tot en met permanente maken. Deze waarde moet groter zijn dan 64 en een macht van 2.|
|CheckpointThresholdInMB|MB|200|De hoeveelheid ruimte in logboekbestand waarna de status gecontroleerd wordt.|
|MaxRecordSizeInKB|KB|1024|Record maximumgrootte replicatie in het logboek schrijven mogelijk. Deze waarde moet een veelvoud is van 4 en groter is dan 16.|
|OptimizeLogForLowerDiskUsage|Booleaanse waarde|waar|Waar is, is het logboek zodat speciale logboekbestand van de replica wordt gemaakt met behulp van een NTFS verspreid bestand geconfigureerd. Hiermee wordt het gebruik van werkelijke schijfruimte voor het bestand minder. Indien false, wordt het bestand wordt gemaakt met vaste toewijzingen, die het beste prestaties schrijven bieden.|
|SharedLogId|GUID|""|Geeft een unieke guid waarmee het gedeelde logboekbestand gebruikt met deze replica. Meestal moeten services niet gebruiken voor deze instelling. Echter als SharedLogId is opgegeven, moet klikt u vervolgens SharedLogPath ook worden opgegeven.|
|SharedLogPath|De naam van het volledige pad|""|Hiermee geeft u het volledige pad waar het gedeelde logboekbestand voor deze replica worden gemaakt. Meestal moeten services niet gebruiken voor deze instelling. Echter als SharedLogPath is opgegeven, moet klikt u vervolgens SharedLogId ook worden opgegeven.|


## <a name="sample-configuration-file"></a>Voorbeeld van configuratiebestand

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
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

## <a name="remarks"></a>Opmerkingen
Replicatielatentie Hiermee stelt u de parameter BatchAcknowledgementInterval. Een waarde van '0' levert de laagste mogelijke latentie, maar oplevert doorvoer (zoals meer bevestigingsberichten moeten worden verzonden en verwerkt, elk met minder bevestigingen).
Des te groter de waarde voor BatchAcknowledgementInterval, het sneller de algehele herhaling worden verwerkt, maar een hogere bewerking latentie oplevert. Dit equivalent rechtstreeks naar de latentie van transactiebevestigingen.

De hoeveelheid schijfruimte die replicatie gebruiken kunt voor de opslag van informatie over de status in van de replica speciale logboekbestand Hiermee stelt u de parameter CheckpointThresholdInMB. Met groter wordende dit op een hogere waarde dan het standaardgeluid kan leiden tot nieuwe configuratie te versnellen wanneer een nieuwe replica wordt toegevoegd aan de set. Dit is vanwege de overdracht van gedeeltelijke staat die uitgevoerd vanwege de beschikbaarheid van meer geschiedenis van bewerkingen in het logboek wordt. Dit meer hersteltijd van een replicaset potentieel na vastlopen.

Als u OptimizeForLowerDiskUsage ingesteld op waar, ruimte in logboekbestand niet te veel ingerichte zodat actieve replica's kunnen meer informatie over de status opslaan in hun logboekbestanden, terwijl inactief replica's minder schijfruimte wordt gebruikt. Hierdoor kunnen voor het hosten van meer replica's op een knooppunt. Als u OptimizeForLowerDiskUsage ingesteld op ONWAAR, wordt de statusinformatie sneller naar de logboekbestanden geschreven.

De instelling MaxRecordSizeInKB Hiermee definieert u de maximale grootte van een record die in het logboekbestand door replicatie kan worden opgeslagen. De standaardgrootte voor 1024 KB record is in de meeste gevallen optimaal. Echter als de service groter gegevensitems veroorzaakt wilt toevoegen aan de statusinformatie, deze waarde moet mogelijk worden verhoogd. Er is weinig voordeel bij het maken van MaxRecordSizeInKB kleiner is dan 1024, omdat voor kleinere records alleen de benodigde ruimte voor de kleinere record worden gebruikt. We verwachten dat deze waarde zou moeten zelden worden gewijzigd.

De instellingen voor SharedLogId en SharedLogPath worden altijd samen gebruikt om een service een apart gedeelde logboek uit het gedeelde standaardlogboek gebruiken voor het knooppunt. Zo veel services mogelijk moeten hetzelfde gedeelde logboek opgeven voor een optimale efficiency. Gedeelde logboekbestanden moeten worden teruggezet op schijven die worden gebruikt voor het gedeelde logboekbestand bewegingen conflict verkleinen. We verwachten dat deze waarden zou moeten zelden worden gewijzigd.
