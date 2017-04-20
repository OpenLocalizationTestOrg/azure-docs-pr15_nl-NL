<properties
   pageTitle="Een Node.js-toepassing waarin MongoDB implementeren | Microsoft Azure"
   description="Stapsgewijze instructies voor het pakket van meerdere Gast uitvoerbare bestanden om te implementeren naar een cluster Azure-Service stof"
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
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>


# <a name="deploy-multiple-guest-executables"></a>Meerdere Gast uitvoerbare bestanden implementeren

In dit artikel leest hoe u Inpakken en implementeren van meerdere Gast uitvoerbare bestanden naar stof van Azure-Service. Lees voor het maken en implementeren van één Service stof pakket het [implementeren van Gast uitvoerbare naar Service stof](service-fabric-deploy-existing-app.md).

Terwijl deze procedure ziet u hoe u een toepassing met een front-end van Node.js dat MongoDB als de gegevensopslag gebruikt implementeren, kunt u de stappen toepassen op een toepassing die afhankelijk van een andere toepassing is.   

Visual Studio kunt u de toepassingspakket met meerdere Gast uitvoerbare bestanden produceren. Zie [Visual Studio gebruiken om het pakket van een bestaande toepassing](service-fabric-deploy-existing-app.md#using-visual-studio-to-package-an-existing-executable). Nadat u het eerste Gast uitvoerbare bestand hebt toegevoegd, klik met de rechtermuisknop op de toepassingsproject en selecteert u de **nieuwe Service stof service -> toevoegen** aan het tweede Gast uitvoerbare project toevoegen aan de oplossing. Opmerking: Als u de bron in het project Visual Studio koppelt, de oplossing Visual Studio bouwen zorgt ervoor dat uw toepassingspakket bijgewerkt met wijzigingen in de bron is. 

## <a name="manually-package-the-multiple-guest-executable-application"></a>Handmatig inpakken de meerdere Gast uitvoerbare toepassing
U kunt ook kunt u handmatig de uitvoerbare Gast pakket. In dit artikel gebruikt voor de handmatige verpakking, het hulpmiddel Service stof verpakking, die beschikbaar binnen [http://aka.ms/servicefabricpacktool is](http://aka.ms/servicefabricpacktool).

### <a name="packaging-the-nodejs-application"></a>Het verpakken van de toepassing Node.js
In dit artikel wordt ervan uitgegaan dat Node.js niet is geïnstalleerd op de knooppunten in het cluster Service stof. Als gevolg hiervan, moet u Node.exe toevoegen aan de hoofdmap van uw toepassing knooppunt voordat de verpakking. De structuur van de map van de Node.js-toepassing (via Express web framework en Jade sjabloon engine) moet er ongeveer hieronder:

```
|-- NodeApplication
  	|-- bin
        |-- www
  	|-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
  	|-- public
        |-- images
        |-- etc.
  	|-- routes
        |-- index.js
        |-- users.js
  	|-- views
        |-- index.jade
        |-- etc.
  	|-- app.js
  	|-- package.json
  	|-- node.exe
```

Een volgende stap, moet u een toepassingspakket voor de toepassing Node.js maken. De onderstaande code wordt gemaakt van een Service stof toepassing-pakket waarin de toepassing Node.js.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Hieronder volgt een beschrijving van de parameters die worden gebruikt:

- **/Source** verwijst naar de map van de toepassing die moet worden verpakt.
- **/ Target** Hiermee definieert u de map waarin het pakket moet worden gemaakt. Deze map heeft afwijkt van de bronmap.
- **/ AppName** Hiermee definieert u de naam van de toepassing van de bestaande toepassing. Het is belangrijk om te begrijpen dat dit komt neer op de naam van de service in het manifest, en niet op de naam van de Service stof-toepassing.
- **exe** definieert het uitvoerbare bestand dat Service stof moet starten in dit geval `node.exe`.
- **/ma** definieert het argument dat wordt gebruikt voor het starten van het uitvoerbare bestand. Als Node.js niet is geïnstalleerd, Service stof moet de webserver Node.js starten door te voeren `node.exe bin/www`.  `/ma:'bin/www'`Hiermee wordt aan de verpakking hulpmiddel gebruik `bin/ma` als het argument voor node.exe.
- **/AppType** Hiermee definieert u de naam van de Service stof toepassing type.

Als u naar de map die is opgegeven in de parameter/target zoeken, kunt u zien dat het hulpmiddel een volledig functioneel Service stof-pakket heeft gemaakt, zoals hieronder wordt weergegeven:

```
|--[yourtargetdirectory]
  	|-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
De gegenereerde ServiceManifest.xml heeft nu een sectie die wordt beschreven hoe de webserver Node.js moet worden gestart, zoals wordt weergegeven in het onderstaande codefragment:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
In dit voorbeeld wordt luistert de webserver Node.js naar poort 3000, zodat u bijwerken van de weergegeven gegevens in het bestand ServiceManifest.xml moet zoals hieronder wordt weergegeven.   

```xml
<Resources>
      <Endpoints>
        <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>Het verpakken van de toepassing MongoDB
U kunt nu dat u de toepassing Node.js verpakt hebt, verdergaan en MongoDB als pakket inpakken. Zoals eerder zijn de stappen die u doorlopen nu gaat niet specifiek zijn voor Node.js en MongoDB. Ja, ze zijn van toepassing op alle toepassingen die zijn bedoeld samen worden geleverd als één stof Service-toepassing.  

Als u wilt inpakken MongoDB, die u wilt controleren of dat u Mongod.exe en Mongo.exe inpakken. Beide binaire bestanden zich bevinden in de `bin` map van uw MongoDB-installatiemap. De mapstructuur ziet er ongeveer als hieronder.

```
|-- MongoDB
  	|-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Service stof moet beginnen MongoDB met een opdracht die vergelijkbaar is met de onderstaande, zodat u moet gebruiken de `/ma` parameter bij MongoDB verpakking.

```
mongod.exe --dbpath [path to data]
```
> [AZURE.NOTE] Niet wordt de gegevens behouden bij een fout knooppunt als u de gegevensmap van MongoDB op de lokale map van het knooppunt plaatst. U moet duurzame opslagruimte gebruiken of een MongoDB replicaset om te voorkomen dat gegevens verloren gaan implementeren.  

In PowerShell of de opdrachtshell uitvoeren we het productverpakkingen-hulpprogramma met de volgende parameters:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

Wilt u MongoDB hebt toegevoegd aan uw Service stof toepassing-pakket, moet u om ervoor te zorgen dat de/Target parameter wordt verwezen naar dezelfde map die al de toepassing bevat samen met de toepassing Node.js bestandenlijst. Moet u ook om ervoor te zorgen dat u dezelfde naam ApplicationType gebruikt.

Laten we Ga naar de map en bekijk wat het hulpmiddel heeft gemaakt.

```
|--[yourtargetdirectory]
  	|-- MyNodeApplication
  	|-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Zoals u ziet, wordt in het hulpmiddel een nieuwe map, MongoDB, toegevoegd aan de map waarin de MongoDB binaire bestanden. Als u opent het `ApplicationManifest.xml` bestand, kunt u zien dat het pakket nu de Node.js-toepassing en de MongoDB bevat. De volgende code bevat de inhoud van manifest van de toepassing.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-the-application"></a>De toepassing publiceren
De laatste stap is het publiceren van de toepassing op de lokale Service stof cluster met behulp van de onderstaande PowerShell-scripts:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

De toepassing wordt gepubliceerd naar de lokale cluster, kunt u de toepassing Node.js op de poort die we hebben ingevoerd in het servicemanifest van de toepassing Node.js--bijvoorbeeld http://localhost:3000 openen.

In deze zelfstudie kunt u eenvoudig inpakken twee bestaande toepassingen als één stof Service-toepassing hebt gezien. U hebt geleerd hoe u deze implementeert bij Service stof, zodat u deze kunt profiteren van enkele van de Service stof-functies, zoals beschikbaarheid en systeemstatus systeemintegratie.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het implementeren van containers met [Service stof en containers-overzicht](service-fabric-containers-overview.md)
