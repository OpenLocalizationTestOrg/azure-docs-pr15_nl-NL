<properties
   pageTitle="Service stof toepassingsmodel | Microsoft Azure"
   description="Het model en toepassingen en services in Service stof beschrijven."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor="mani-ramaswamy"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"   
   ms.author="seanmck"/>

# <a name="model-an-application-in-service-fabric"></a>Een toepassing in de Service stof model

Dit artikel bevat een overzicht van het model van de toepassing stof van Azure-Service. Hierin wordt ook het definiëren van een toepassing en service via een manifest-bestanden en het ophalen van de toepassing verpakt en klaar voor implementatie.

## <a name="understand-the-application-model"></a>Meer informatie over het toepassingsmodel

Een toepassing is een verzameling onderdeel services die een bepaalde functie of functies uitvoeren. Een service werkt een voltooid en zelfstandige (deze kunt starten en uitvoeren onafhankelijk van andere services) en bestaat uit de code, configuratie en gegevens. Voor elke service, code bestaat uit de uitvoerbare binaire bestanden, configuratie bestaat uit de service-instellingen die kunnen worden geladen tijdens de uitvoering en gegevens bestaat uit willekeurige statische gegevens kunnen worden gebruikt door de service. Elk onderdeel in dit toepassingsmodel hiërarchische kan zijn versienummer en een bijgewerkte onafhankelijk.

![Het model van de toepassing Service stof][appmodel-diagram]


Een toepassingstype is een categorisatie van een toepassing en bestaat uit een bundel servicetype. Een servicetype is een categorisatie van een service. De categorisatie kan hebben verschillende instellingen en configuraties, maar de belangrijkste functies van dezelfde blijft. Exemplaren van een service zijn de andere service configuratie variaties van hetzelfde servicetype.  

Klassen (of "typen") van toepassingen en services in XML-bestanden (Toepassingsmanifesten en service manifesten) die zijn de sjablonen die toepassingen kunnen worden gemaakt in van het cluster afbeelding store zijn beschreven. De schemadefinitie voor het bestand ServiceManifest.xml en ApplicationManifest.xml is geïnstalleerd met de Service stof SDK en functies waarmee u *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

De code naar exemplaren van de andere toepassing wordt uitgevoerd als afzonderlijke processen, zelfs wanneer die worden gehost door hetzelfde Service stof knooppunt. Bovendien kan de levensduur van elk toepassingsexemplaar (dat wil zeggen worden beheerd een upgrade) onafhankelijk. In het volgende diagram ziet u hoe de toepassingstypen servicetype, beurtelings bestaan uit de code, configuratie en pakketten zijn samengesteld. Om te vereenvoudigen het diagram, alleen de code/config/gegevens pakketten voor `ServiceType4` worden weergegeven, hoewel elk servicetype, enkele bevatten zou of alle die als pakket typen inpakken.

![Service stof toepassing bestandstypen en -servicetypen][cluster-imagestore-apptypes]

Twee verschillende manifest-bestanden worden gebruikt om te beschrijven van toepassingen en services: de servicemanifest en toepassingsmanifest. Deze worden beschreven in in de daaropvolgende gedeelten.

Er kan een of meer exemplaren van een servicetype actief zijn in het cluster. Bijvoorbeeld bereiken statuscontrole service-exemplaren of replica's hoge betrouwbaarheid repliceren staat tussen replica's die zich op andere knooppunten in het cluster. Deze replicatie vindt u in feite redundantie voor de service kan worden gecontroleerd, zelfs als een van de knooppunten in een cluster mislukt. Een [gepartitioneerde service](service-fabric-concepts-partitioning.md) verder deelt de staat (en de access-patronen aan deze staat) op knooppunten in het cluster.

In het volgende diagram ziet u de relatie tussen toepassingen en service-exemplaren, partities en replica's.

![Partities en replica's binnen een service][cluster-application-instances]


>[AZURE.TIP] U kunt de indeling van toepassingen weergeven in een hulpprogramma voor het Service stof Explorer beschikbaar http:// cluster&lt;yourclusteraddress&gt;: 19080/Explorer. Zie [uw cluster met Service stof Explorer visualiseren](service-fabric-visualizing-your-cluster.md)voor meer informatie.

## <a name="describe-a-service"></a>Een service beschrijven

De servicemanifest definieert declaratieve de servicetype en versie. Hiermee geeft u de service-metagegevens zoals servicetype, eigenschappen status, Maatstelsel van taakverdeling, service binaire bestanden en configuratiebestanden.  Een andere manier hebt opgeslagen, hierin de code, configuratie en gegevens-pakketten waaruit een servicepakket ter ondersteuning van een of meer servicetypen. Hier volgt een eenvoudig voorbeeld servicemanifest:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
~~~

**Versie** kenmerken ongestructureerde tekenreeksen zijn en niet worden geparseerd door het systeem. Hierna ziet u een versie kon u elk onderdeel voor upgrades.

**ServiceTypes** gedeclareerd welke servicetypen door **CodePackages** worden ondersteund in deze manifest. Wanneer een service wordt gestart met een van deze servicetypen, worden alle code-pakketten gedeclareerd in dit manifest geactiveerd door hun vermelding punten uit te voeren. De resulterende processen wordt verwacht dat de typen ondersteunde service registreren tijdens runtime. Houd er rekening mee dat servicetypen op het manifest niveau en niet het niveau in de code-pakket zijn gedeclareerd. Dus wanneer er meerdere code-pakketten, worden alle geactiveerd wanneer het systeem Hiermee wordt gezocht naar een van de servicetypen aangegeven.

**SetupEntryPoint** is een bevoegdheden ingangspunt die compatibel is met dezelfde referenties als Service stof *(meestal de systeemaccount)* voordat u andere ingangspunt. Het uitvoerbare bestand opgegeven door **ingangspunt** is meestal de langdurige ServiceHost. De aanwezigheid van een afzonderlijke instellingen ingangspunt voorkomt dat u moet uitvoeren van de ServiceHost met hoge bevoegdheden voor langere tijd. Het uitvoerbare bestand opgegeven door **ingangspunt** wordt uitgevoerd nadat **SetupEntryPoint** is afgesloten. De resulterende proces wordt gecontroleerd en opnieuw gestart (begin opnieuw met **SetupEntryPoint**) als dit ooit wordt beëindigd of loopt vast.

**DataPackage** gedeclareerd een map, met het kenmerk **Name** , willekeurige statische gegevens kunnen worden gebruikt door het proces tijdens runtime met de naam.

**ConfigPackage** gedeclareerd een map, met het kenmerk **Name** , die een *Settings.xml* -bestand bevat de naam. Dit bestand bevat secties van de gebruiker gedefinieerde, sleutelwaarden paar instellingen die tijdens de uitvoering kan worden gelezen door het proces. Tijdens een upgrade als alleen de **ConfigPackage** **versie** is gewijzigd, is klikt u vervolgens het actieve proces niet gestart. In plaats daarvan krijgt een terugbellen het proces dat configuratie-instellingen zijn gewijzigd, zodat ze opnieuw kunnen worden geladen dynamisch. Hier volgt een voorbeeld van een bestand *Settings.xml* :

~~~
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSecion">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
~~~

> [AZURE.NOTE] Een servicemanifest kan bevatten meerdere code, configuratie en gegevenspakketten. Elk van deze kan zijn versienummer onafhankelijk.

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Een toepassing beschrijven


Manifest van de toepassing worden declaratieve de toepassingstype en versie. Hiermee geeft u de service samenstelling metagegevens zoals stabiele namen, partitioneren kleurenschema, exemplaar tellen/herhaling factor beveiliging/moeten worden geïsoleerd beleid, plaatsing beperkingen, configuratie negeren en de servicetypen. De taakverdeling domeinen waarin de toepassing wordt geplaatst, worden ook beschreven.

Een toepassingsmanifest elementen op het toepassingsniveau van de wordt beschreven en dus verwijst naar een of meer service manifesten waarin u een toepassingstype opstelt. Hier volgt een eenvoudig voorbeeld toepassingsmanifest:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
~~~

Zoals service manifesten, **versie** kenmerken ongestructureerde tekenreeksen zijn en niet worden geparseerd door het systeem. Hierna ziet u ook gebruikt om elk onderdeel naar versie voor upgrades.

**ServiceManifestImport** bevat verwijzingen naar service-manifesten waaruit dit toepassingstype. Geïmporteerde service manifesten bepalen met welke servicetypen zijn geldig binnen dit toepassingstype.

**DefaultServices** gedeclareerd service-exemplaren die automatisch worden gemaakt wanneer een toepassing ten opzichte van dit toepassingstype is gemaakt. Services die standaard zijn alleen een gemak en zich gedragen als normale services in alle opzichten nadat ze zijn gemaakt. Ze samen met andere services in het toepassingsexemplaar van zijn bijgewerkt en kunnen ook worden verwijderd.

> [AZURE.NOTE] Een toepassingsmanifest kan bevatten meerdere service manifest invoer en standaardservices. Elke service manifest importeren kan onafhankelijk versienummer zijn.

Als u wilt weten hoe u voor het behoud van andere toepassing en serviceparameters voor afzonderlijke omgevingen, raadpleegt u [beheren toepassingsparameters voor meerdere omgevingen](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->

## <a name="package-an-application"></a>Een toepassing als pakket Inpakken

### <a name="package-layout"></a>Een lay-out

Manifest van de toepassing, service manifest(s) en andere noodzakelijke pakketbestanden moeten worden ingedeeld in een specifieke indeling voor implementatie in een cluster Service stof. De manifesten voorbeeld in dit artikel moet worden ingedeeld in de structuur van de volgende map:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
~~~

De mappen zijn benoemde zodat deze overeenkomt met de kenmerken van de **naam** van elk corresponderende element. Als de servicemanifest twee code-pakketten met de namen **MyCodeA** en **MyCodeB bevatte**, zou vervolgens twee mappen met dezelfde naam bijvoorbeeld de benodigde binaire bestanden voor elk pakket code bevatten.

### <a name="use-setupentrypoint"></a>SetupEntryPoint gebruiken

Gebruikelijke scenario's voor het gebruik van **SetupEntryPoint** zijn wanneer u moet een uitvoerbaar bestand uitvoeren voordat de service wordt gestart, anders moet u een bewerking met meer bevoegdheden uitvoeren. Bijvoorbeeld:

- Instellen en initialisatie van de omgevingsvariabelen die de uitvoerbare bestand van de service nodig heeft. Dit is niet beperkt tot uitvoerbare bestanden via de Service stof programming modellen zijn geschreven. Bijvoorbeeld vereisten npm.exe voor sommige omgevingsvariabelen is geconfigureerd voor het implementeren van een toepassing voor de node.js.

- Bij het instellen van toegangsbeheer door te installeren beveiligingscertificaten.

### <a name="build-a-package-by-using-visual-studio"></a>Een pakket maken met behulp van Visual Studio

Als u Visual Studio-2015 maken van uw toepassing gebruikt, kunt u de opdracht Inpakken automatisch maken van een pakket dat overeenkomt met de indeling hierboven is beschreven.

Als u wilt maken van een pakket, met de rechtermuisknop op het toepassingsproject in Solution Explorer en kies de opdracht inpakken, zoals hieronder wordt weergegeven:

![Het verpakken van een toepassing met Visual Studio][vs-package-command]

Wanneer de verpakking is voltooid, vindt u de locatie van het pakket in **het uitvoervenster** . Houd er rekening mee dat de verpakking stap automatisch optreedt wanneer u implementeren of fouten opsporen in uw toepassing in Visual Studio.

### <a name="test-the-package"></a>Het pakket testen

U kunt de pakketstructuur lokaal via PowerShell verifiëren met de opdracht **Test-ServiceFabricApplicationPackage** . Deze opdracht wordt controleren op problemen parseren manifest en controleer of alle verwijzingen. Houd er rekening mee dat deze opdracht alleen wordt gecontroleerd met de structurele correct voor mappen en bestanden in het pakket. Er wordt een van de inhoud van de code of gegevens het pakket voorbij controleren of alle benodigde bestanden aanwezig zijn niet gecontroleerd.

~~~
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
~~~

Deze fout wordt weergegeven dat het bestand dat *MySetup.bat* waarnaar wordt verwezen in de servicemanifest **SetupEntryPoint** uit het pakket code ontbreekt. Nadat het ontbrekende bestand is toegevoegd, wordt de controle van toepassingen doorgegeven:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
~~~

Zodra de toepassing juist is gecomprimeerd en verificatie doorstuurt, klikt u vervolgens werkmap is gereed voor implementatie.

## <a name="next-steps"></a>Volgende stappen

[Implementeren en -toepassingen verwijderen][10]

[Parameters voor toepassingen voor meerdere omgevingen beheren][11]

[RunAs: Een toepassing voor de Service stof uitgevoerd met verschillende machtigingen][12]

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png
[vs-package-command]: ./media/service-fabric-application-model/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
