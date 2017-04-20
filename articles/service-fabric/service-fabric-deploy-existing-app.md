<properties
   pageTitle="Een bestaande uitvoerbaar bestand implementeren naar Azure-Service stof | Microsoft Azure"
   description="Stapsgewijze instructies voor het pakket van een bestaande toepassing als gast uitvoerbare, zodat deze kan worden toegepast op een Service stof cluster"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>

# <a name="deploy-a-guest-executable-to-service-fabric"></a>Uitvoerbaar Gast implementeren naar Service stof

U kunt elk gewenst type-toepassing, zoals node.js, Java of systeemeigen toepassingen uitvoeren in Azure-Service stof. Service stof verwijst naar dit soort toepassingen als gast uitvoerbare bestanden.
Gast uitvoerbare bestanden worden verwerkt door de Service stof zoals stateless services. Hierdoor worden deze geplaatst op knooppunten in een cluster, op basis van beschikbaarheid en andere aan de doelstellingen. In dit artikel wordt beschreven hoe Inpakken en Gast uitvoerbare implementeren naar een Service stof cluster, Visual Studio of een hulpprogramma voor de opdrachtregel gebruikt.

In dit artikel behandeld de stappen voor het inpakken Gast uitvoerbare en het dashboard implementeren naar Service stof.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Voordelen van het uitvoeren van uitvoerbare in Service stof Gast

Er zijn diverse voordelen die worden meegeleverd bij Gast uitvoerbaar in een cluster Service stof uitgevoerd:

- Beschikbaarheid. Toepassingen die worden uitgevoerd in de Service stof worden aangebracht ten zeerste beschikbaar zijn. Service stof zorgt ervoor dat exemplaren van een toepassing worden uitgevoerd.
- Statuscontrole. Service stof statuscontrole, wordt als een toepassing wordt uitgevoerd en diagnostische informatie biedt als er een fout gedetecteerd.   
- Beheer van productlevenscyclus van toepassing. Naast upgrades voorzien van geen downtime, biedt Service stof automatische terug te gaan naar de vorige versie als er een ongeldige status gebeurtenis gerapporteerd tijdens een upgrade.    
- Dichtheid. U kunt meerdere toepassingen uitvoert in een cluster, wat niet nodig voor elke toepassing uitvoeren op een eigen hardware.


## <a name="overview-of-application-and-service-manifest-files"></a>Overzicht van toepassing en service manifest-bestanden

Als onderdeel van het implementeren van uitvoerbare Gast, is het handig voor meer informatie over de Service stof verpakking en implementatiemodel als [toepassingsmodel](service-fabric-application-model.md)beschreven. Het model van de verpakking Service stof is afhankelijk van twee XML-bestanden: de toepassing is en service-manifesten. De schemadefinitie voor de bestanden ApplicationManifest.xml en ServiceManifest.xml is geïnstalleerd met de Service stof SDK in *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **Manifest van de toepassing** Manifest van de toepassing wordt gebruikt om te beschrijven van de toepassing. Dit zijn de services die maakt deze en andere parameters die worden gebruikt om te bepalen hoe de een of meer services moeten worden toegepast, zoals het aantal exemplaren.

  In de structuur van de Service is een toepassing een maateenheid voor de implementatie en upgrade. Een toepassing kan worden bijgewerkt als één eenheid waar mogelijke fouten en mogelijke terugdraaien van acties worden beheerd. Service stof zorgt ervoor dat het upgradeproces is een succesvolle, of, als de upgrade is mislukt, verlaat niet de toepassing in een onbekend/vastloopt staat.

* **Service bestandenlijst** De servicemanifest beschrijving van de onderdelen van een service. Deze bevat gegevens, zoals de naam en type service, en de code, configuratie en gegevens. De servicemanifest bevat ook enkele aanvullende parameters die kunnen worden gebruikt om de service configureren zodra deze is geïmplementeerd.


## <a name="application-package-file-structure"></a>Toepassing pakket bestandsstructuur
Als u wilt een toepassing Service stof implementeren, moet de toepassing een vooraf gedefinieerde mapstructuur volgen. Het volgende voorbeeld van die structuur.

```
|-- ApplicationPackageRoot
  	|-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```

De ApplicationPackageRoot bevat de ApplicationManifest.xml-bestand met de toepassing definieert. Een submap voor elke service die is opgenomen in de toepassing wordt gebruikt voor alle de onderdelen die de service is vereist; de ServiceManifest.xml en meestal de volgende drie mappen:

- *Code*. Deze map bevat de code.
- *Config*. Deze map bevat een Settings.xml-bestand (en andere bestanden indien nodig) dat de service toegang hebben tot tijdens runtime om op te halen specifieke configuratie-instellingen.
- *Gegevens*. Dit is een extra directory om op te slaan aanvullende lokale gegevens die u de service moet mogelijk. Opmerking: Gegevens moet worden gebruikt voor de opslag alleen kortstondige gegevens. Service stof bevat niet kopiëren/repliceren wijzigingen in de gegevensmap als de service worden verplaatst moet, bijvoorbeeld tijdens een overname.

Opmerking: Die u niet hoeft te maken de `config` en `data` mappen als u deze niet nodig.

## <a name="packaging-an-existing-executable"></a>Het verpakken van een bestaande uitvoerbaar bestand

Als een gast uitvoerbaar bestand het verpakken, kunt u hetzij gebruik van een project-sjabloon voor Visual Studio of [handmatig de toepassingspakket maken](#manually). Gebruik van Visual Studio, worden de toepassing pakketstructuur en manifest-bestanden gemaakt door de wizard Nieuw project voor u.

>[AZURE.NOTE] De eenvoudigste manier om een bestaande Windows uitvoerbaar in een service als pakket inpakken is Visual Studio gebruiken.

## <a name="using-visual-studio-to-package-an-existing-executable"></a>Gebruik van Visual Studio inpakken van een bestaande uitvoerbaar bestand

Visual Studio biedt een sjabloon van de service-Service stof waarmee u Gast uitvoerbare implementeren naar een Service stof cluster.

Ga via de volgende stappen voor het voltooien van de publicatie:

1. Kies Bestand -> Nieuw Project en een stof-servicetoepassing maken.
2. Kies Gast uitvoerbare bestand als de servicesjabloon.
3. Klik op Bladeren en selecteer de map met uw uitvoerbare bestand en vul de rest van de parameters voor het maken van de service.
    - *Code pakketgedrag* kan worden ingesteld kopiëren van alle inhoud van de map naar de Visual Studio-Project, welke is handig als het uitvoerbare bestand wordt niet gewijzigd. Als u verwacht het uitvoerbare bestand dat wilt wijzigen en de mogelijkheid te werk om nieuwe builds dynamisch wilt, kunt u kiezen om de koppeling naar de map in plaats daarvan. Houd er rekening mee dat u gekoppelde mappen gebruiken kunt bij het maken van het project application in Visual Studio. Dit is gekoppeld aan de bronlocatie van binnen het project, waardoor u de uitvoerbare Gast in de bestemming bron bijwerken te laten uitvoeren die deel uitmaken van het toepassingspakket op opbouwen.
    - *Programma* - Kies het uitvoerbare bestand dat moet worden uitgevoerd om de service te starten.
    - *Argumenten* - opgeven van de argumenten die moeten worden doorgegeven naar het uitvoerbare bestand. Kan zijn om een lijst met parameters die met argumenten.
    - *WorkingFolder* - Hiermee geeft u de werkmap voor het proces dat dient te worden gestart. U kunt drie waarden opgeven:
        - `CodeBase`Hiermee wordt opgegeven dat de werkmap is opgegeven om te worden naar de codemap in de toepassingspakket (`Code` in de bestandsstructuur weergegeven voorafgaand aan de map).
        - `CodePackage`Hiermee wordt opgegeven dat de werkmap wilt worden ingesteld in de hoofdmap van de toepassingspakket (`GuestService1Pkg` in de bestandsstructuur voorafgaand aan de weergegeven).
        - `Work`Geeft aan dat de bestanden in een submap werk worden geplaatst
4. Geef een naam op voor uw service en klik op OK.
5. Als uw service een eindpunt voor communicatie moet, kunt u nu het Protocol, poorten en Type toevoegen aan het bestand ServiceManifest.xml. e.g. `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. U kunt nu de Inpakken en publiceren van actie ten opzichte van uw lokale cluster door de oplossing in Visual Studio foutopsporing. Als u klaar bent kunt u de toepassing aan een externe cluster publiceren of de oplossing aan bronbeheer inchecken.
7. Ga naar het einde van dit artikel om te zien hoe u de weergave die u uitvoerbare Gast-service in Service stof Explorer wordt uitgevoerd.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-an-existing-executable"></a>Handmatig het verpakken en implementeren van een bestaande uitvoerbaar bestand
Het proces van het handmatig verpakking een gast uitvoerbaar bestand is gebaseerd op de volgende stappen uit:

1. Maak de mapstructuur pakket.
2. De bestanden code en configuratie van de toepassing toevoegen.
3. Het manifest servicebestand bewerken.
4. De toepassing manifest bestand bewerken.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>De mapstructuur pakket maken
U kunt beginnen met het maken van de structuur van de adreslijst, zoals eerder is beschreven.

### <a name="add-the-applications-code-and-configuration-files"></a>De bestanden code en configuratie van de toepassing toevoegen
Nadat u de mapstructuur hebt gemaakt, kunt u de code van de toepassing en configuratiebestanden onder de code en config mappen kunt toevoegen. U kunt ook extra mappen of submappen onder de code of config mappen maken.

Service stof biedt een xcopy van de inhoud van de hoofdmap van de toepassing, dus is er geen vooraf gedefinieerde structuur dan het maken van de twee belangrijkste mappen, code en instellingen. (U kunt kiezen verschillende namen als u wilt. Meer details zijn in het volgende gedeelte.)

>[AZURE.NOTE] Zorg ervoor dat u alle bestanden/afhankelijkheden die de toepassing moet bevatten. De inhoud van het toepassingspakket op alle knooppunten kopiëren service stof in het cluster waar van de toepassing services gaat worden geïmplementeerd. Het pakket mogen bevatten de code die de toepassing moet worden uitgevoerd. We raden niet ervan uitgaande dat de afhankelijkheden al zijn geïnstalleerd.

### <a name="edit-the-service-manifest-file"></a>De service manifest bestand bewerken
De volgende stap is het manifest servicebestand als u wilt opnemen van de volgende informatie bewerken:

- De naam van het servicetype. Dit is een ID die Service stof wordt gebruikt om een service te identificeren.
- De opdracht gebruiken om de toepassing (ExeHost) te starten.
- Een script dat moet worden uitgevoerd om in te stellen omhoog/configureren de toepassing (SetupEntrypoint).

De volgende is een voorbeeld van een `ServiceManifest.xml` bestand:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

Laten we eens de verschillende onderdelen van het bestand dat u wilt bijwerken:

#### <a name="updating-the-servicetypes"></a>De ServiceTypes bijwerken

```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

- U kunt een naam die u wilt gebruiken voor kiezen `ServiceTypeName`. De waarde wordt gebruikt in de `ApplicationManifest.xml` bestand om de service te identificeren.
- U moet opgeven `UseImplicitHost="true"`. Dit kenmerk worden vermeld Service stof dat de service is gebaseerd op een zelfstandig-app, zodat alle Service stof moet gebeuren is dit als een proces te starten en controleren van de servicestatus.

#### <a name="updating-the-codepackage"></a>De CodePackage bijwerken
Het element CodePackage geeft de locatie (en versie) van de code van de service.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

De `Name` element wordt gebruikt voor het opgeven van de naam van de map in het toepassingspakket van de service-code bevat. `CodePackage`heeft ook de `version` kenmerk. Hiermee kan worden gebruikt om op te geven van de versie van de code-- en mogelijk ook van de service-code bijwerkt met behulp van de Service stof van toepassing levenscyclus management infrastructuur kan worden gebruikt.
#### <a name="optional-updating-the-setupentrypoint"></a>Optioneel: De SetupEntrypoint bijwerken

```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
De SetupEntrypoint-element wordt gebruikt om op te geven van een bestand uitvoerbare bestand of de batch die moet worden uitgevoerd voordat de code van de service wordt gestart. Deze is stap optioneel, zodat deze niet hoeft te worden opgenomen als er geen initialisatie/configuratie vereist. De SetupEntryPoint wordt uitgevoerd telkens wanneer de service opnieuw is opgestart.

Is er slechts één SetupEntrypoint, dus setup/config scripts worden gegroepeerd in een enkel batchbestand moeten als van de toepassing setup/config vereist is voor meerdere scripts. De SetupEntrypoint kunt elk type bestand--uitvoerbare bestanden, batch-bestanden en PowerShell-cmdlets uitvoeren. Zie [SetupEntryPoint configureren](service-fabric-application-runas-security.md)voor meer informatie.

Klik in het voorgaande voorbeeld, de SetupEntrypoint wordt uitgevoerd een batchbestand genoemd `LaunchConfig.cmd` die zich bevindt in de `scripts` submap van de codemap (ervan uitgaande dat het element WorkingFolder is ingesteld op CodeBase).

#### <a name="updating-the-entrypoint"></a>De ingangspunt bijwerken

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

De `Entrypoint` element in de bestandenlijst servicebestand kunt u opgeven hoe u het starten van de service wordt gebruikt. De `ExeHost` element geeft het uitvoerbare bestand (en de argumenten) die moet worden gebruikt voor het starten van de service.

- `Program`Hiermee geeft u de naam van het uitvoerbare bestand dat moet worden uitgevoerd om de service te starten.
- `Arguments`Hiermee geeft u de argumenten die moeten worden doorgegeven naar het uitvoerbare bestand. Kan zijn om een lijst met parameters die met argumenten.
- `WorkingFolder`Hiermee geeft u de werkmap voor het proces dat dient te worden gestart. U kunt drie waarden opgeven:
    - `CodeBase`Hiermee wordt opgegeven dat de werkmap is opgegeven om te worden naar de codemap in de toepassingspakket (`Code` directory in de voorgaande bestandsstructuur).
    - `CodePackage`Hiermee wordt opgegeven dat de werkmap wilt worden ingesteld in de hoofdmap van de toepassingspakket (`GuestService1Pkg` in de voorgaande bestandsstructuur).
  - `Work`Geeft aan dat de bestanden in een submap werk worden geplaatst

De WorkingFolder is handig voor het instellen van de juiste werkmap zodat relatieve paden kunnen worden gebruikt door de toepassing of een initialisatie scripts.

#### <a name="updating-the-endpoints-and-registering-with-naming-service-for-communication"></a>Bijwerken van de eindpunten en registreren met Naming Service voor communicatie

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
Klik in het voorgaande voorbeeld de `Endpoint` element geeft de eindpunten die de toepassing kunt luisteren op. In dit voorbeeld wordt de toepassing Node.js luistert op http op poort 3000.

Bovendien kunt u Service stof dit eindpunt publiceren naar de Naming Service, zodat andere services kunnen het eindpuntadres naar deze service detecteren vragen. Hiermee kunt u kunnen communiceren tussen services bekijken die gast uitvoerbare bestanden zijn.
Het gepubliceerde eindpuntadres is aan het formulier `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme`en `PathSuffix` zijn optioneel kenmerken. `IPAddressOrFQDN`is het IP-adres of de volledig gekwalificeerde domeinnaam van het knooppunt dit uitvoerbare bestand wordt geplaatst op en voor u wordt berekend.

In het volgende voorbeeld nadat de service wordt geïmplementeerd, in de Service stof Explorer ziet u een eindpunt die vergelijkbaar is met `http://10.1.4.92:3000/myapp/` gepubliceerd voor het exemplaar van de service of als dit is een lokale computer die u ziet `http://localhost:3000/myapp/`. 

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
U kunt deze adressen gebruiken met de [reverse-proxy](service-fabric-reverseproxy.md) voor de communicatie tussen services.

### <a name="edit-the-application-manifest-file"></a>De toepassing manifest bestand bewerken

Nadat u hebt geconfigureerd de `Servicemanifest.xml` bestand, die u wilt aanbrengen enkele wijzigingen in de `ApplicationManifest.xml` bestand om ervoor te zorgen dat de juiste servicetype en de naam worden gebruikt.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport

In de `ServiceManifestImport` element, kunt u een of meer services die u wilt opnemen in de app. Services wordt verwezen naar met `ServiceManifestName`, die aangeeft dat de naam van de map waar het `ServiceManifest.xml` bestand zich bevindt.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Logboekregistratie instellen
Voor gast uitvoerbare bestanden is het handig om te kunnen zien console Logboeken om vast te stellen als de toepassing en configuratie scripts eventuele fouten weergegeven.
Console-omleiding kan worden geconfigureerd de `ServiceManifest.xml` bestand met de `ConsoleRedirection` element.

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

* `ConsoleRedirection`kan worden gebruikt om te leiden console-uitvoer (stdout en stderr) een werkmap zodat ze kunnen worden gebruikt om te bevestigen dat er geen fouten tijdens de installatie of het uitvoeren van de toepassing in het cluster Service stof.

    * `FileRetentionCount`bepaalt het aantal bestanden worden opgeslagen in de werkmap. Een waarde van 5, bijvoorbeeld betekent dat de logboekbestanden voor de vorige vijf executions worden opgeslagen in de werkmap.
    * `FileMaxSizeInKb`Hiermee geeft u de maximale grootte van de logboekbestanden.

Logboekbestanden worden opgeslagen in een van de service werken mappen. Om te bepalen waar de bestanden die zich bevindt, moet u Service stof Explorer gebruiken om te bepalen welke knooppunt dat op de service wordt uitgevoerd en welke werkmap wordt gebruikt. Dit proces valt verderop in dit artikel.

## <a name="deployment"></a>Implementatie
De laatste stap is uw toepassing implementeren. Het volgende PowerShell-script leert hoe u uw toepassing naar het plaatselijke ontwikkeling cluster implementeren en een nieuwe Service stof-service starten.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```
Een Service stof-service kan worden geïmplementeerd in verschillende "configuraties." Bijvoorbeeld kan worden geïnstalleerd als één of meerdere exemplaren of deze zo dat er een exemplaar van de service voor elk knooppunt van de Service stof cluster wordt kan worden geïmplementeerd.

De `InstanceCount` -parameter van de `New-ServiceFabricService` cmdlet wordt gebruikt om op te geven hoeveel exemplaren van de service wordt gestart in het cluster Service stof. U kunt instellen dat de `InstanceCount` waarde, afhankelijk van het type van toepassing die u wilt implementeren. Dit zijn de twee meest voorkomende scenario's:

* `InstanceCount = "1"`. In dit geval wordt slechts één exemplaar van de service geïmplementeerd in het cluster. De service stof scheduler bepaalt welke knooppunt dat de service dient te worden geïmplementeerd op.

* `InstanceCount ="-1"`. In dit geval wordt één exemplaar van de service geïmplementeerd op elk knooppunt in het cluster Service stof. Het resultaat is één (en slechts één) exemplaar van de service voor elk knooppunt dat in het cluster.

Dit is een handige configuratie voor front-toepassingen (bijvoorbeeld een REST-eindpunt) omdat clienttoepassingen moeten "verbinden met" de knooppunten in het cluster gebruik van het eindpunt. Deze configuratie kan ook worden gebruikt wanneer u bijvoorbeeld alle knooppunten van de Service stof cluster zijn verbonden met een taakverdeling zodat clientverkeer kan worden verdeeld over de service die wordt uitgevoerd op alle knooppunten in het cluster.

## <a name="check-your-running-application"></a>Controleer uw actieve toepassing

Geef aan dat het knooppunt waarop de service wordt uitgevoerd in Service stof Explorer. In dit voorbeeld wordt uitgevoerd op knooppunt1:

![Knooppunt waar de service wordt uitgevoerd](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Als u naar het knooppunt navigeren en blader naar de toepassing, ziet u de knooppuntgegevens van essentiële, inclusief de locatie op schijf.

![Locatie op schijf](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Als u naar de map zoeken met behulp van de Server Explorer, vindt u de werkmap en een van de service logboekmap als de volgende afbeelding ziet.

![Locatie van logboekbestanden](./media/service-fabric-deploy-existing-app/loglocation.png)


## <a name="next-steps"></a>Volgende stappen
In dit artikel, hebt u geleerd hoe inpakken een gast uitvoerbaar bestand en het dashboard implementeren naar Service stof. U kunt een volgende stap, extra inhoud van dit onderwerp raadplegen.

- [Voorbeeld voor verpakking en implementeren van uitvoerbaar op GitHub Gast](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/GuestExe/SimpleApplication), inclusief een koppeling naar de voorlopige versie van het hulpmiddel verpakking
- [Meerdere Gast uitvoerbare bestanden implementeren](service-fabric-deploy-multiple-apps.md)
- [Uw eerste Service stof toepassing maken met Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
