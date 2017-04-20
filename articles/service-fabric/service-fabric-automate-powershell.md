<properties
    pageTitle="Service stof Toepassingsbeheer automatiseren via PowerShell | Microsoft Azure"
    description="Implementeren, upgrade, testen en stof Service-toepassingen verwijderen via PowerShell."
    services="service-fabric"
    documentationCenter=".net"
    authors="rwike77"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-fabric"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="ryanwi"/>

# <a name="automate-the-application-lifecycle-using-powershell"></a>De levenscyclus van de toepassing via PowerShell automatiseren

Veel aspecten van de [levenscyclus van de Service stof-toepassing](service-fabric-application-lifecycle.md) kunnen worden geautomatiseerd.  In dit artikel leest hoe u PowerShell gebruiken om algemene taken voor implementeert, bijwerken, verwijderen en testen van Azure-Service stof-toepassingen te automatiseren.  Beheerd en HTTP APIs voor de app management zijn ook beschikbaar. Zie [de levenscyclus van de app](service-fabric-application-lifecycle.md) voor meer informatie.  

## <a name="prerequisites"></a>Vereisten voor
Voordat u op naar de taken in het artikel verplaatsen, moet u naar:

+ Vertrouwd raken met de Service stof concepten die worden beschreven in [technisch overzicht van de Service stof](service-fabric-technical-overview.md).
+ [Installeren van de runtime, SDK, en hulpprogramma's](service-fabric-get-started.md), waarin installeert ook de **ServiceFabric** PowerShell-module.
+ [Uitvoering van inschakelen PowerShell-script](service-fabric-get-started.md#enable-powershell-script-execution).
+ Een lokale cluster starten.  Een nieuw venster PowerShell als beheerder starten en vervolgens het cluster setup-script uitvoeren uit de map SDK:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
+ Voordat u een PowerShell-opdrachten in dit artikel uitvoert, eerst verbinding maken met de lokale Service stof cluster met behulp van [**Connect-ServiceFabricCluster**](https://msdn.microsoft.com/library/azure/mt125938.aspx):`Connect-ServiceFabricCluster localhost:19000`
+ De volgende taken vereisen het pakket van een v1-toepassing te implementeren en een v2 toepassing voor de upgrade. Download de [ **WordCount** steekproef-toepassing](http://aka.ms/servicefabricsamples) (zich in de voorbeelden aan de slag). Bouw en inpakken van de toepassing in Visual Studio (met de rechtermuisknop op **WordCount** in Solution Explorer en selecteer **pakket**). Kopieer het pakket v1 in `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` naar `C:\Temp\WordCount`. Kopie `C:\Temp\WordCount` naar `C:\Temp\WordCountV2`, het pakket v2 toepassing voor de upgrade maken. Open `C:\Temp\WordCountV2\ApplicationManifest.xml` in een teksteditor. Wijzig in het element **ApplicationManifest** het kenmerk **ApplicationTypeVersion** van '1.0.0' naar '2.0.0' bij het app-versienummer. Sla het gewijzigde ApplicationManifest.xml-bestand.

## <a name="task-deploy-a-service-fabric-application"></a>Taak: Een Service stof-toepassing implementeren

Nadat u hebt gemaakt en de toepassing verpakt (of de toepassingspakket hebt gedownload), kunt u de toepassing in een lokale Service stof cluster implementeren. Implementatie heeft betrekking op het toepassingspakket van de uploaden, het toepassingstype registreren en het exemplaar van de toepassing maken. Volg de instructies in deze sectie naar een nieuwe toepassing aan een cluster implementeren.

### <a name="step-1-upload-the-application-package"></a>Stap 1: De toepassingspakket uploaden
Het toepassingspakket van de uploaden naar de afbeelding van de store verplaatst het onderdeel op een locatie toegankelijk zijn voor interne Service stof onderdelen.  De toepassingspakket bevat de benodigde toepassingsmanifest, service manifesten en code, configuratie, en gegevens pakket(ten) om de toepassing en exemplaren van de service te maken.  De opdracht [**Kopiëren-ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125905.aspx) uploadt het pakket. Bijvoorbeeld:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>Stap 2: Het toepassingstype registreren
Registreren van de toepassingspakket, maakt de toepassingstype en de versie voor gebruik in een manifest van de toepassing beschikbaar zijn gedeclareerd. Het systeem leest het pakket in de stap 1 hebt geüpload, Controleer of het pakket (gelijk aan [**Test-ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125950.aspx) lokaal uitgevoerd) en de inhoud van het pakket van proces het verwerkte pakket kopiëren naar de systeemlocatie van een interne.  De [**Register-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) -cmdlet uitvoeren:

```powershell
Register-ServiceFabricApplicationType WordCount
```
Als u wilt zien van alle de toepassingstypen geregistreerd in het cluster, voert u de cmdlet [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/azure/mt125871.aspx) :

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>Stap 3: Het toepassingsexemplaar van de maken
Een toepassing kan worden gemaakt met behulp van een versie van de toepassing type dat is geregistreerd met de opdracht [**Nieuw ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125913.aspx) . De naam van elke toepassing wordt gedeclareerd op implementeren van tijd en moet beginnen met het **stof:** kleurenschema en voor elk toepassingsexemplaar uniek zijn. De naam van de toepassing type en de toepassing type versie zijn gedefinieerd in het bestand **ApplicationManifest.xml** van de toepassingspakket. Als een standaardservices zijn gedefinieerd in de manifest van de toepassing van het type doeltoepassing, zijn klikt u vervolgens die gemaakt op dit moment.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

De opdracht [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx) worden alle instanties van toepassing die zijn gemaakt, samen met de algemene status. De opdracht [**Get-ServiceFabricService**](https://msdn.microsoft.com/library/azure/mt125889.aspx) vindt u alle van de service-exemplaren die zijn gemaakt in een bepaalde toepassingsexemplaar. Standaardservices (indien aanwezig) worden weergegeven.

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Taak: Een toepassing voor de Service stof upgraden
U kunt een upgrade uitvoert voor een eerder gedistribueerde stof Service-toepassing met een bijgewerkte toepassing-pakket. Deze taak de WordCount-toepassing die is geïmplementeerd in upgrades "taak: een Service stof-toepassing implementeren." Lees door de [Service stof toepassing upgraden](service-fabric-application-upgrade.md) voor meer informatie.

Als u wilt houden dingen eenvoudige voor dit voorbeeld, is in het WordCountV2 toepassing-pakket hebt gemaakt in de vereisten voor alleen het versienummer van toepassing bijgewerkt. Een meer natuurlijke scenario waarbij bijwerken van uw service-code, configuratie of gegevens-bestanden en klik vervolgens opnieuw te bouwen en verpakking van de toepassing met de bijgewerkte versie getallen.  

### <a name="step-1-upload-the-updated-application-package"></a>Stap 1: De bijgewerkte toepassing-pakket uploaden
De WordCount v1-toepassing kan worden bijgewerkt. Als u een PowerShell-venster als beheerder en type [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx)opent, ziet u dat die versie 1.0.0 van het type van de toepassing WordCount wordt geïmplementeerd.  

Het pakket bijgewerkte toepassing nu kopiëren naar de Service stof afbeelding store (waar de toepassingspakketten worden opgeslagen door de Service stof). De parameter **ApplicationPackagePathInImageStore** wordt geïnformeerd Service stof waar deze het toepassingspakket kunt vinden. De volgende opdracht kopieert de toepassingspakket naar **WordCountV2** in de winkel afbeelding:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>Stap 2: De bijgewerkte toepassingstype registreren
De volgende stap is de nieuwe versie van de toepassing registreren bij Service stof, die kan worden uitgevoerd met behulp van de [**Register-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) -cmdlet:

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>Stap 3: De upgrade starten
Verschillende parameters voor het bijwerken, -outs en systeemstatus criteria kunnen worden toegepast op toepassingsupgrades. Lees de [toepassing een upgrade uitvoert parameters](service-fabric-application-upgrade-parameters.md) en [proces](service-fabric-application-upgrade.md) documenten voor meer informatie. Alle services en processen moeten _in orde_ na de upgrade.  Stel de **HealthCheckStableDuration** tot 60 seconden (zodat de services zijn in orde voor ten minste 20 seconden voordat de upgrade naar de volgende upgrade domein verloopt).  Stel ook de **UpgradeDomainTimeout** 1200 seconden en de **UpgradeTimeout** op 3000 seconden. Ten slotte, stelt u de **UpgradeFailureAction** op **ongedaan maken**, die vraagt dat Service stof ongedaan gemaakt de toepassing naar de vorige versie als er fouten zijn opgetreden tijdens de upgrade.

U kunt de upgrade van de toepassing nu starten met behulp van de [**Begin-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125975.aspx) -cmdlet:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Houd er rekening mee dat de naam van toepassing hetzelfde als de naam van de toepassing eerder geïmplementeerd v1.0.0 is (stof: / WordCount). Deze naam van de service stof wordt gebruikt om aan te geven welke toepassing wordt ophalen bijgewerkt. Als u de time-outs moeten te kort instelt, kunnen er een time-out mislukt-bericht dat het probleem. Raadpleeg [problemen oplossen met de toepassingsupgrades](service-fabric-application-upgrade-troubleshooting.md)of de time-outs vergroten.

### <a name="step-4-check-upgrade-progress"></a>Stap 4: Selectievakje upgrade voortgang
U kunt de toepassing upgrade voortgang controleren met behulp van de [Service stof Explorer](service-fabric-visualizing-your-cluster.md)of met behulp van de cmdlet [**Get-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125988.aspx) :

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

In een paar minuten de cmdlet [Get-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/azure/mt125988.aspx) aangegeven dat alle upgrade domeinen zijn bijgewerkt (voltooid).

## <a name="task-test-a-service-fabric-application"></a>Taak: Een toepassing voor de Service stof testen

Als u wilt schrijven kwalitatief hoogwaardig services, moeten ontwikkelaars kunnen veroorzaken onbetrouwbare infrastructuur fouten om te testen van de stabiliteit van hun services. Service stof kunt ontwikkelaars veroorzaken foutenstructuuranalyse acties en testen van services in de aanwezigheid van fouten met behulp van de chaos en overname bij storing test-scenario's.  [Overzicht van de testbaarheid](service-fabric-testability-overview.md) Lees voor meer informatie.

### <a name="step-1-run-the-chaos-test-scenario"></a>Stap 1: Het chaos Testscenario uitvoeren
Het chaos Testscenario genereert fouten in het hele Service stof cluster. Het scenario wordt gecomprimeerd fouten in het algemeen gezien via maanden of jaren enkele uren. De combinatie van afwisselende fouten met een hoge foutenstructuuranalyse tarief vindt hoek zaken die anders zou worden overgeslagen. Het volgende voorbeeld wordt de chaos Testscenario uitgevoerd voor 60 minuten.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>Stap 2: Het failover-Testscenario uitvoeren
De overname testen scenario doelen een specifieke services partition in plaats van het hele cluster en deze andere services ongewijzigd blijven. Een reeks gesimuleerd fouten in service validatie doorlopen het scenario terwijl uw bedrijfslogica wordt uitgevoerd. Een fout in de service validatie duidt op een probleem die nog moet worden verder onderzoek. De test failover induceert slechts één foutenstructuuranalyse per keer, in plaats van het scenario voor het testen van chaos, waarin meerdere fouten kan veroorzaken. Het volgende voorbeeld wordt de test failover voor 60 minuten uitgevoerd ten opzichte van de stof: / WordCount/WordCountService-service.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Taak: Een toepassing Service stof verwijderen
U kunt een exemplaar van een gedistribueerde toepassing verwijderen, de ingerichte toepassingstype verwijderen uit het cluster en het toepassingspakket verwijderen uit de ImageStore.

### <a name="step-1-remove-an-application-instance"></a>Stap 1: Een toepassingsexemplaar verwijderen
Wanneer een toepassingsexemplaar niet meer nodig hebt wordt, kunt u deze permanent verwijderen met behulp van de cmdlet [**Verwijderen-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125914.aspx) . Alle services die deel uitmaken van de toepassing, permanent verwijderen alle servicestatus wordt ook automatisch verwijderd. Deze bewerking kan niet ongedaan worden gemaakt en de toepassingsstatus kan niet worden hersteld.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>Stap 2: Het toepassingstype Unregister
Wanneer u een bepaalde versie van het type van een toepassing niet meer nodig hebt, kunt u deze met behulp van de [**Registratie-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125885.aspx) -cmdlet unregister. Afmelden van niet-gebruikte typen loslaat opslagruimte die wordt gebruikt door het toepassingspakket in de winkel afbeelding. Een toepassingstype kan worden gemaakt zo lang maken als er geen toepassingen tegen of in behandeling toepassingsupgrades ernaar wordt gestart.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>Stap 3: Het toepassingspakket verwijderen
Nadat het toepassingstype niet geregistreerd is, kan het toepassingspakket worden verwijderd uit de store afbeelding met behulp van de cmdlet [**Verwijderen-ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt163532.aspx) .

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Volgende stappen
[De levenscyclus van de service stof-toepassing](service-fabric-application-lifecycle.md)

[Een toepassing implementeren](service-fabric-deploy-remove-applications.md)

[Upgrade van toepassing](service-fabric-application-upgrade.md)

[Azure-Service stof-cmdlets](https://msdn.microsoft.com/library/azure/mt125965.aspx)

[Azure-Service stof testbaarheid cmdlets](https://msdn.microsoft.com/library/azure/mt125844.aspx)
