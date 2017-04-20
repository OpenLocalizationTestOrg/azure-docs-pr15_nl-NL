<properties
   pageTitle="Meerdere omgevingen in Service stof beheren | Microsoft Azure"
   description="Stof service-toepassingen kunnen worden uitgevoerd op clusters die in grootte van één computer tot duizenden machines variëren. In sommige gevallen wilt u wordt uw toepassing voor deze gevarieerde omgevingen anders configureren. In dit artikel wordt uitgelegd hoe u andere toepassingsparameters per omgeving definiëren."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/19/2016"
   ms.author="seanmck"/>

# <a name="manage-application-parameters-for-multiple-environments"></a>Parameters voor toepassingen voor meerdere omgevingen beheren

U kunt Azure-Service stof clusters maken met behulp van een willekeurige plaats één tot duizenden machines. Terwijl de binaire bestanden van toepassing kunnen worden uitgevoerd zonder wijzigingen in deze groot aantal omgevingen, zult u vaak voor het configureren van de toepassing anders, afhankelijk van het aantal machines die u naar implementeren bent.

Een eenvoudig voorbeeld, kunt u beter `InstanceCount` voor een stateless service. Wanneer u toepassingen in Azure wordt aangegeven uitvoert, wilt u in het algemeen deze parameter instelt op de speciale waarde '-1'. Dit zorgt ervoor dat uw service wordt uitgevoerd op elk knooppunt in het cluster. Deze configuratie is echter niet geschikt voor een cluster één computer aangezien er geen meerdere processen luisteren op hetzelfde eindpunt op één computer. In plaats daarvan u meestal wordt ingesteld `InstanceCount` op '1'.

## <a name="specifying-environment-specific-parameters"></a>Omgeving / regiospecifieke parameters geven

De oplossing voor dit configuratieprobleem is een set met parameters standaardservices en parameter toepassingsbestanden die in deze parameterwaarden voor een gegeven-omgeving doorvoeren. Standaardservices en parameters voor toepassingen zijn in de toepassing is en service-manifesten geconfigureerd. De schemadefinitie voor de bestanden ServiceManifest.xml en ApplicationManifest.xml is geïnstalleerd met de Service stof SDK en functies waarmee u *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

### <a name="default-services"></a>Standaardservices

Service stof toepassingen bestaan uit een verzameling exemplaren van de service. Het is mogelijk te maken van een lege toepassing en maakt alle exemplaren van de service dynamisch, hebben de meeste toepassingen een set van core services die altijd moet worden gemaakt wanneer de toepassing wordt gestart. Deze worden 'standaardservices' genoemd. Ze zijn opgegeven in een manifest van de toepassing, met tijdelijke aanduidingen voor de configuratie van de per-omgeving is opgenomen in vierkante haken:

    <DefaultServices>
        <Service Name="Stateful1">
            <StatefulService
                ServiceTypeName="Stateful1Type"
                TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
                MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

                <UniformInt64Partition
                    PartitionCount="[Stateful1_PartitionCount]"
                    LowKey="-9223372036854775808"
                    HighKey="9223372036854775807"
                />
        </StatefulService>
    </Service>
  </DefaultServices>

Elk van de benoemde parameters moet worden gedefinieerd binnen het element Parameters van manifest van de toepassing:

    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>

Het kenmerk DefaultValue geeft de waarde moet worden gebruikt bij afwezigheid van een specifiekere-parameter voor een gegeven-omgeving.

>[AZURE.NOTE] Niet alle parameters voor het exemplaar van service zijn geschikt voor per-omgeving configuratie. In het bovenstaande voorbeeld wordt zijn de LowKey en HighKey waarden voor de partities kleurenschema van de service expliciet gedefinieerd naar alle exemplaren van de service aangezien het bereik partition een functie van het domein van de gegevens, niet de omgeving is.


### <a name="per-environment-service-configuration-settings"></a>Configuratie-instellingen voor per-omgeving

De [Service stof toepassingsmodel](service-fabric-application-model.md) kunnen services configuratiepakketten met aangepaste sleutel-waardeparen die kunnen gelezen tijdens runtime worden opnemen. De waarden van deze instellingen kunnen ook worden onderscheiden door omgeving door het opgeven van een `ConfigOverride` in manifest van de toepassing.

Stel dat u hebt de volgende instelling in het bestand Config\Settings.xml voor de `Stateful1` service:


    <Section Name="MyConfigSection">
      <Parameter Name="MaxQueueSize" Value="25" />
    </Section>

Als u wilt deze waarde voor een specifieke toepassing/omgeving paar overschrijven, maak een `ConfigOverride` wanneer u de servicemanifest in manifest van de toepassing importeert.

    <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>

Deze parameter kan vervolgens worden geconfigureerd door omgeving zoals hierboven. U kunt dit doen door deze in de sectie parameters van manifest van de toepassing te declareren en omgeving / regiospecifieke waarden opgeven in de parameter toepassingsbestanden.

>[AZURE.NOTE] In het geval van service configuratie-instellingen, zijn er drie plaatsen waar de waarde van een sleutel kan worden ingesteld: de configuratie servicepakket, manifest van de toepassing en het bestand van de toepassing-parameter. Service stof wordt altijd kiezen uit de parameter-bestand van de toepassing eerste (indien opgegeven), klikt u vervolgens de manifest van de toepassing en ten slotte de configuratiepakket.


### <a name="application-parameter-files"></a>Parameter toepassingsbestanden

Het project van de toepassing Service stof kunt een of meer parameter toepassingsbestanden opnemen. Elk van deze de specifieke waarden voor de parameters die zijn gedefinieerd in manifest van de toepassing wordt gedefinieerd:

    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="2" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>

Standaard bevat een nieuwe toepassing twee parameter toepassingsbestanden, met de naam Local.xml en Cloud.xml:

![Parameter toepassingsbestanden in Solution Explorer][app-parameters-solution-explorer]

Als u wilt een nieuw parameterbestand maken, te kopiëren en plakken een bestaande en hieraan een nieuwe naam.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Identificerende omgeving / regiospecifieke parameters tijdens de implementatie

Bij de implementatie moet u de juiste parameter-bestand om toe te passen met uw toepassing kiezen. U kunt dit doen via het dialoogvenster publiceren in Visual Studio of via PowerShell.

### <a name="deploy-from-visual-studio"></a>Implementeren in Visual Studio

U kunt kiezen uit de lijst met beschikbare parameterbestanden wanneer u uw toepassing in Visual Studio publiceren.

![Kies een parameterbestand in het dialoogvenster publiceren][publishdialog]

### <a name="deploy-from-powershell"></a>Implementeren van PowerShell

De `Deploy-FabricApplication.ps1` PowerShell-script is opgenomen in de toepassing projectsjabloon accepteert geen profiel publiceren als een parameter en de PublishProfile bevat een verwijzing naar het bestand van de toepassing parameters.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over een aantal van de belangrijkste begrippen die worden beschreven in dit onderwerp, de [Service stof technisch overzicht](service-fabric-technical-overview.md). Zie [uw stof Service-toepassingen in Visual Studio beheren](service-fabric-manage-application-in-visual-studio.md)voor informatie over andere mogelijkheden voor het beheer van app die beschikbaar in Visual Studio zijn.

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
