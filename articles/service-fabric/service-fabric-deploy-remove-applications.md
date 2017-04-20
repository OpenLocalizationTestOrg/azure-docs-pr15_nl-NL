<properties
   pageTitle="Installatie van service stof toepassingen | Microsoft Azure"
   description="Hoe kunt implementeren en toepassingen in de Service stof verwijderen"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="deploy-and-remove-applications-using-powershell"></a>Implementeren en via PowerShell-toepassingen verwijderen

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Één keer een die [toepassingstype is gecomprimeerd][10], de werkmap is gereed voor implementatie in een cluster stof van Azure-Service. Implementatie omvat de volgende drie stappen:

1. De toepassingspakket uploaden
2. Het toepassingstype registreren
3. Exemplaar van de toepassing maken

>[AZURE.NOTE] Als u Visual Studio gebruikt voor de implementatie en voor foutopsporing in toepassingen op uw cluster plaatselijke ontwikkeling, zijn de volgende stappen automatisch via een PowerShell-script zijn gevonden in de map van het toepassingsproject verwerkt. In dit artikel biedt achtergrond aan deze scripts presteren zodat u bewerkingen buiten Visual Studio kunt uitvoeren.

## <a name="upload-the-application-package"></a>De toepassingspakket uploaden

Het toepassingspakket van de uploaden, wordt deze plaatst in een locatie die toegankelijk is voor interne Service stof onderdelen. U kunt PowerShell gebruiken om uit te voeren de upload. Voordat u een PowerShell-opdrachten in dit artikel, altijd starten met [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) verbinding maken met de Service stof cluster.

Stel dat u hebt een map genaamd *MyApplicationType* bevat die de benodigde toepassingsmanifest, service manifesten en code/config/gegevens-pakketten. De opdracht [Kopiëren-ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125905.aspx) uploadt het pakket naar het cluster afbeelding Store. De cmdlet **Get-ImageStoreConnectionStringFromClusterManifest** , die deel van de Service stof SDK PowerShell-module uitmaakt, wordt gebruikt om de verbindingsreeks van de afbeelding van de store.  Als u wilt importeren de module SDK, voert u het:

```
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

U kunt het toepassingspakket van een van *C:\users\ryanwi\Documents\Visual Studio 2015\Projects\MyApplication\myapplication\pkg\debug* kopiëren naar *c:\temp\MyApplicationType* (naam de map 'foutopsporing' naar 'MyApplicationType'). Het volgende voorbeeld uploads het pakket:

~~~
PS C:\temp> dir

    Directory: c:\temp


Mode                LastWriteTime         Length Name                                                                                   
----                -------------         ------ ----                                                                                   
d-----        8/12/2016  10:23 AM                MyApplicationType                                                                          

PS C:\temp> tree /f .\Stateless1Pkg

C:\TEMP\MyApplicationType
│   ApplicationManifest.xml
|
└───Stateless1Pkg
  	|   ServiceManifest.xml
    │
    └───Code
    │   │  Microsoft.ServiceFabric.Data.dll
    │   │  Microsoft.ServiceFabric.Data.Interfaces.dll
    │   │  Microsoft.ServiceFabric.Internal.dll
    │   │  Microsoft.ServiceFabric.Internal.Strings.dll
    │   │  Microsoft.ServiceFabric.Services.dll
    │   │  ServiceFabricServiceModel.dll
    │   │  MyService.exe
    │   │  MyService.exe.config
    │   │  MyService.pdb
    │   │  System.Fabric.dll
    │   │  System.Fabric.Strings.dll
    │   │
    │   └───en-us
  	|         Microsoft.ServiceFabric.Internal.Strings.resources.dll
  	|         System.Fabric.Strings.resources.dll
  	|
    ├───Config
    │     Settings.xml
    │
    └───Data
          init.dat

PS C:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath MyApplicationType -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
Copy application package succeeded

PS D:\temp>
~~~

## <a name="register-the-application-package"></a>Het toepassingspakket registreren

Registreren van de toepassingspakket, maakt de toepassingstype en de versie voor gebruik in een manifest van de toepassing beschikbaar zijn gedeclareerd. Het systeem leest het pakket in de vorige stap hebt geüpload, Controleer of het pakket (gelijk aan [Test-ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125950.aspx) lokaal uitgevoerd) en de inhoud van het pakket van proces het verwerkte pakket kopiëren naar de systeemlocatie van een interne.

~~~
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
~~~

De opdracht [Register-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125958.aspx) retourneert alleen nadat het systeem heeft het toepassingspakket gekopieerd. Hoe lang deze duurt, is afhankelijk van de inhoud van de toepassingspakket. Indien nodig, kan de parameter **- TimeoutSec** worden gebruikt voor een langere time-out opgeven. (De standaard-time-out is 60 seconden.)

De opdracht [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125871.aspx) bevat alle geregistreerd toepassing type versies.

## <a name="create-the-application"></a>De toepassing maken

U kunt een exemplaar van een toepassing maken met behulp van een versie van de toepassing type dat is geregistreerd via de opdracht [Nieuw ServiceFabricApplication](https://msdn.microsoft.com/library/mt125913.aspx) . De naam van elke toepassing moet beginnen met het *stof:* kleurenschema en voor elk toepassingsexemplaar uniek zijn. Een standaardservices gedefinieerd in de manifest van de toepassing van het type doeltoepassing worden op dit moment gemaakt.

~~~
PS D:\temp> New-ServiceFabricApplication fabric:/MyApp MyApplicationType AppManifestVersion1

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationStatus      : Ready
HealthState            : OK
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/MyService
ServiceKind            : Stateless
ServiceTypeName        : MyServiceType
IsServiceGroup         : False
ServiceManifestVersion : SvcManifestVersion1
ServiceStatus          : Active
HealthState            : Ok

PS D:\temp>
~~~

De opdracht [Get-ServiceFabricApplication](https://msdn.microsoft.com/library/mt163515.aspx) worden alle instanties van toepassing die zijn gemaakt, samen met de algemene status.

De opdracht [Get-ServiceFabricService](https://msdn.microsoft.com/library/mt125889.aspx) bevat alle service-exemplaren die zijn gemaakt in een bepaalde toepassingsexemplaar. Standaardservices (indien aanwezig), worden hier weergegeven.

Meerdere toepassingsexemplaren kunnen worden gemaakt voor een bepaald-versie van een geregistreerde toepassingstype. Elk toepassingsexemplaar wordt uitgevoerd in moeten worden geïsoleerd, met eigen werk directory en proces.

## <a name="remove-an-application"></a>Verwijderen van een toepassing

Wanneer een toepassingsexemplaar niet meer nodig hebt wordt, kunt u deze permanent verwijderen met de opdracht [Verwijderen-ServiceFabricApplication](https://msdn.microsoft.com/library/mt125914.aspx) . Deze opdracht automatisch Hiermee verwijdert u alle services die deel uitmaakt van de toepassing, ook alle servicestatus permanent verwijderen. Deze bewerking kan niet ongedaan worden gemaakt en de toepassingsstatus kan niet worden hersteld.

~~~
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
~~~

Wanneer een bepaalde versie van een toepassingstype niet meer nodig hebt is, moet u deze met de opdracht [Unregister-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125885.aspx) unregister. Afmelden van niet-gebruikte typen loslaat opslagruimte die wordt gebruikt door de inhoud van de toepassing-pakket van dat type op de afbeelding van de store. Een toepassingstype kan worden gemaakt zo lang maken als geen toepassingen zijn geactiveerd tegen en niet in afwachting van toepassing upgrades hiernaar wordt verwezen.

~~~
PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp> Unregister-ServiceFabricApplicationType MyApplicationType AppManifestVersion1
Unregister application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

PS D:\temp>
~~~

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopie-ServiceFabricApplicationPackage wordt gevraagd om een ImageStoreConnectionString

De Service stof SDK-omgeving moet al de juiste standaardwaarden instellen. Maar indien nodig de ImageStoreConnectionString voor alle opdrachten moet overeenkomen met de waarde die het cluster Service stof wordt gebruikt. U kunt dit vinden in het cluster manifest opgehaald via de opdracht [Get-ServiceFabricClusterManifest](https://msdn.microsoft.com/library/mt126024.aspx) :

~~~
PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType

cmdlet Copy-ServiceFabricApplicationPackage at command pipeline position 1
Supply values for the following parameters:
ImageStoreConnectionString:

PS D:\temp> Get-ServiceFabricClusterManifest
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]

PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType -ImageStoreConnectionString file:D:\ServiceFabric\Data\ImageStore
Copy application package succeeded

PS D:\temp>
~~~

## <a name="next-steps"></a>Volgende stappen

[Service stof toepassing upgrade](service-fabric-application-upgrade.md)

[Service stof systeemstatus Inleiding](service-fabric-health-introduction.md)

[Vaststellen en oplossen van een Service stof-service](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Een toepassing in de Service stof model](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
