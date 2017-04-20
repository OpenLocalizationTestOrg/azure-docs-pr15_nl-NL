<properties
   pageTitle="Service-stof en implementeren Containers | Microsoft Azure"
   description="Service stof en het gebruik van containers te implementeren van microservice-toepassingen. In dit artikel worden de mogelijkheden die Service stof voor containers biedt en het implementeren van de afbeelding van een Windows-container in een cluster"
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
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="preview-deploy-a-windows-container-to-service-fabric"></a>Voorbeeld: Een container Windows implementeren naar Service stof

> [AZURE.SELECTOR]
- [Windows-Container implementeren](service-fabric-deploy-container.md)
- [Docker Container implementeren](service-fabric-deploy-container-linux.md)

In dit artikel begeleidt u bij het samenstellen van beperkte services in Windows containers. 

>[AZURE.NOTE] Deze functie is in de proefversie voor Linux en momenteel niet beschikbaar voor gebruik met Windows Server 2016. Dit is alleen beschikbaar in de Preview-versie voor Windows Server 2016 in de volgende versie van Service stof en ondersteunde in de volgende versie daarna.

Service stof heeft verschillende container mogelijkheden die u helpen bij het bouwen van toepassingen die bestaan van microservices die beperkte zijn. Deze worden beperkte services genoemd. 

De mogelijkheden opnemen;

- Container afbeelding implementatie en activering
- Resource-beheermodel
- Opslagplaats verificatie
- Container poort host poort toewijzen aan
- Discovery naar-andere containers en communicatie
- Mogelijkheid om te configureren en omgevingsvariabelen instellen

U kunt uitzien op elk van de mogelijkheden beurtelings verpakking een beperkte service in uw toepassing moet worden opgenomen.

## <a name="packaging-a-windows-container"></a>Een container Windows verpakking

Wanneer u een container het verpakken, kunt u een met een project-sjabloon voor Visual Studio of [handmatig de toepassingspakket maken](#manually). Gebruik van Visual Studio, worden de toepassing pakketstructuur en manifest-bestanden gemaakt door de wizard Nieuw project voor u (dit is binnenkort in de volgende versie).

## <a name="using-visual-studio-to-package-an-existing-container-image"></a>Gebruik van Visual Studio inpakken van een bestaande container-afbeelding

>[AZURE.NOTE] In toekomstige versie van de Visual Studio gereedschap SDK, is mogelijk een container toevoegen aan een toepassing op een soortgelijke manier u Gast uitvoerbare vandaag kunt toevoegen. Zie [Deploy uitvoerbare naar Service stof Gast van een](service-fabric-deploy-existing-app.md) onderwerp. U moet momenteel handmatige verpakking doen, zoals hieronder beschreven.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-a-container"></a>Handmatig het verpakken en implementeren van een container
Het proces van het handmatig verpakking een beperkte service is gebaseerd op de volgende stappen uit:

1. De containers die zijn gepubliceerd naar uw bibliotheek.
2. Maak de mapstructuur pakket.
3. Het manifest servicebestand bewerken.
4. De toepassing manifest bestand bewerken.

## <a name="container-image-deployment-and-activation"></a>Container afbeelding implementatie en activering.
In de Service stof [toepassingsmodel](service-fabric-application-model.md)vertegenwoordigt een container een toepassinghost waarin meerdere service replica's worden geplaatst. Om te implementeren en een container activeren, moet u de naam van de container afbeelding in een `ContainerHost` element in het servicemanifest.

In het servicemanifest toevoegen een `ContainerHost` voor het fragment wijst en stel de `ImageName` moeten de naam van de container bibliotheek en de afbeelding. Het volgende gedeeltelijke manifest toont een voorbeeld van de container *myimage:v1* aangeroepen vanuit een bibliotheek genoemd *myrepo* implementeren

    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimagename:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>

Opdrachten invoer naar de afbeelding van de container kan worden verstrekt door het opgeven van het optionele `Commands` element door een komma is scheidingsteken opdrachten om uit te voeren in de container. 

## <a name="resource-governance"></a>Resource-beheermodel
Resource-beheermodel is een mogelijkheid van de container en zorgt ervoor dat de resources die de container op de host kunt gebruiken. De `ResourceGovernancePolicy`, opgegeven in de toepassingsmanifest, biedt de mogelijkheid om te declareren resource limieten voor een code servicepakket. Limieten voor de resource kunnen worden ingesteld voor;

- Geheugen
- MemorySwap
- CpuShares (CPU relatieve gewicht)
- MemoryReservationInMB  
- BlkioWeight (relatieve BlockIO het gewicht). 

>[AZURE.NOTE] Klik in een toekomstige release ondersteuning voor het opgeven van bepaald tekstblok IO limieten zoals IO's / s, alleen-lezen/schrijven BPS en anderen is mogelijk.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500" 
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>


## <a name="repository-authentication"></a>Opslagplaats verificatie
Als u wilt downloaden van een container is het wellicht login referenties naar de bibliotheek van de container op te geven. De aanmeldingsreferenties, opgegeven in de *toepassing* manifest worden gebruikt om op te geven van de aanmeldingsgegevens of SSH-sleutel, voor het downloaden van de afbeelding van de container uit de bibliotheek van de afbeelding.  Het volgende voorbeeld ziet u een account genoemd *TestUser* samen met het wachtwoord als gewone tekst. Hiermee wordt **niet** aanbevolen.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Het wachtwoord kunt en moet worden versleuteld met een certificaat dat is geïmplementeerd op de computer.

Het volgende voorbeeld ziet u een account genoemd *TestUser* met het wachtwoord versleuteld met een certificaat *MyCert*genoemd. U kunt de `Invoke-ServiceFabricEncryptText` Powershell-opdracht om te maken van de geheime versleutelde tekst voor het wachtwoord. Zie dit artikel [geheimen in stof Service-toepassingen beheren](service-fabric-application-secret-management.md) voor meer informatie over. De persoonlijke sleutel van het certificaat decoderen het wachtwoord moet worden geïmplementeerd op de lokale computer in een methode out-van-band (in Azure dit via ARM is). Klik wanneer het pakket met service-Service stof in de computer implementeren, hoeft u kunnen ontsleutelen het geheim en verifiëren samen met de naam van het account, met de container-bibliotheek met deze referenties.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

## <a name="container-port-to-host-port-mapping"></a>Container poort host poort toewijzen aan
U kunt configureren dat een host poort gebruikt om te communiceren met de container door het opgeven van een `PortBinding` in toepassingsmanifest. De binding poort Hiermee wijst u de poort die de service op in de container op een poort op de host luistert.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>


## <a name="container-to-container-discovery-and-communication"></a>Discovery naar-andere containers en communicatie
Met de `PortBinding` beleid kunt u een container poort afzetten een `Endpoint` in het servicemanifest zoals wordt weergegeven in het volgende voorbeeld. Het eindpunt `Endpoint1` kan een vaste poort opgeven, bijvoorbeeld poort 80 of geen poort helemaal opgeven in dat geval een willekeurige poort uit de clusters toepassing poortbereik is gekozen voor u.

Voor gast containers, geven een `Endpoint` zoals in het servicemanifest Hierdoor kunnen Service stof automatisch dit eindpunt publiceren naar de Naming service, zodat andere services worden uitgevoerd in het cluster deze container met de query's in oplossen service REST kunnen detecteren. 

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

U kunt eenvoudig-naar-andere containers communicatie in de code in de container met de [reverse-proxy](service-fabric-reverseproxy.md)registreert met de Naming service, doen. U hoeft te is reverse-proxy http poort luisteren en de naam van de services die u communiceren wilt met door deze als omgevingsvariabelen opgeven. Raadpleeg het volgende gedeelte over hoe u dit wilt doen.  

## <a name="configure-and-set-environment-variables"></a>Configureren en omgevingsvariabelen instellen
Omgevingsvariabelen kunnen opgegeven tegenstander elk pakket code in de service manifest voor beide services geïmplementeerd in containers of als processen/Gast uitvoerbare bestanden zijn. Deze waarden kunnen worden overschreven specifiek in manifest van de toepassing of tijdens de implementatie als toepassingsparameters opgegeven.

Het volgende servicemanifest-XML-fragment toont een voorbeeld van de omgevingsvariabelen voor een pakket code opgeven. 

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>

Deze omgevingsvariabelen kunnen worden overschreven op het niveau van manifest:

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>

In het bovenstaande voorbeeld we hebben een expliciete waarde opgegeven voor de `HttpGateway` omgevingsvariabele (19000) terwijl de waarde voor `BackendServiceName` parameter zo is ingesteld de `[BackendSvc]` toepassing parameter. Hiermee kunt u opgeven van de waarde voor `BackendServiceName`waarde op het moment van de toepassing implementeren, en geen een vaste waarde in het manifest. 

## <a name="complete-examples-for-application-and-service-manifest"></a>Voorbeelden van toepassing en servicemanifest voltooien
Hierna volgt een voorbeeld van de toepassingsmanifest waarin de container-functies.


    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>


Hierna volgt een voorbeeld servicemanifest (opgegeven in de voorgaande manifest van toepassing) waarin de container-functies

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless frontend in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Eendpoints>
                <Endpoint Name="Endpoint1" Port="80"  UriScheme="http" />
            </Eendpoints>
        </Resources>
    </ServiceManifest>
