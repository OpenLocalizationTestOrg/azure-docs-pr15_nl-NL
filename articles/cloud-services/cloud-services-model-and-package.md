<properties
    pageTitle="Wat is een Cloudservice model en pakket | Microsoft Azure"
    description="Beschrijving van de cloud service-model (.csdef, .cscfg) en pakket (.cspkg) in Azure wordt aangegeven"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>
<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>

# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Wat is het model Cloudservice en hoe ik het pakket Inpakken?
Een cloudservice wordt gemaakt van drie onderdelen, de definitie van service _(.csdef)_, de service config _(.cscfg)_en een service-pakket _(.cspkg)_. De **ServiceDefinition.csdef** **ServiceConfig.cscfg** bestanden en worden op XML gebaseerde en beschrijven van de structuur van de cloudservice en hoe deze geconfigureerd; genoemd het model. De **ServicePackage.cspkg** is een zipbestand dat is gegenereerd uit de **ServiceDefinition.csdef** en onder andere, bevat alle van de vereiste afhankelijkheden op basis van een binair getal. Azure maakt u een cloudservice van de **ServicePackage.cspkg** zowel de **ServiceConfig.cscfg**.

Zodra de cloudservice wordt uitgevoerd in Azure wordt aangegeven, kunt u deze configureren via het bestand **ServiceConfig.cscfg** , maar u kunt de definitie niet wijzigen.

## <a name="what-would-you-like-to-know-more-about"></a>Wat wilt u meer weten?

* Ik wil meer weten over de [ServiceDefinition.csdef](#csdef) en [ServiceConfig.cscfg](#cscfg) -bestanden.
* Ik al weten over die, geef mij [enkele voorbeelden](#next-steps) op wat ik kunt configureren.
* Ik wil de [ServicePackage.cspkg](#cspkg)maken.
* Ik gebruik Visual Studio, maar ik wil …
    * [Een nieuwe cloudservice maken][vs_create]
    * [Een bestaande cloudservice configureren][vs_reconfigure]
    * [Een project Cloudservice implementeren][vs_deploy]
    * [Extern bureaublad in een exemplaar van de service cloud][remotedesktop]

<a name="csdef"></a>
## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
Het bestand **ServiceDefinition.csdef** Hiermee geeft u de instellingen die worden gebruikt door Azure voor het configureren van een cloudservice. Het [Azure Service definitie Schema (.csdef bestand)](https://msdn.microsoft.com/library/azure/ee758711.aspx) vindt u de toegestane-indeling voor een definitiebestand service. Het volgende voorbeeld ziet de instellingen die kunnen worden gedefinieerd voor het Web en werknemer rollen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

U kunt verwijzen naar de [[Service definitie Schema]] voor een beter begrip van de XML-schema hier gebruikt, maar hier is een snelle uitleg van enkele elementen van de:

**Sites**  
Bevat de definities voor websites of web-toepassingen die worden gehost in IIS7.

**InputEndpoints**  
Bevat de definities voor eindpunten die worden gebruikt voor het contact opnemen met de cloudservice.

**InternalEndpoints**  
Bevat de definities voor eindpunten die worden gebruikt door rol exemplaren met elkaar communiceren.

**ConfigurationSettings**  
Bevat de definities voor functies van een specifieke rol.

**Certificaten**  
Bevat de definities voor certificaten die u nodig voor een rol hebt. Het vorige voorbeeld ziet u een certificaat dat wordt gebruikt voor de configuratie van Azure verbinding maken.

**LocalResources**  
Bevat de definities voor lokale opslag resources. Een resource lokale opslag is een gereserveerde map in het bestandssysteem van de virtuele machine waarin een exemplaar van een rol wordt uitgevoerd.

**Invoer**  
Bevat de definities voor geïmporteerde modules. Het vorige voorbeeld ziet u de modules voor verbinding met extern bureaublad en Azure verbinding te maken.

**Opstarten**  
Bevat de taken die worden uitgevoerd wanneer de rol wordt gestart. De taken die zijn gedefinieerd in een cmd of uitvoerbaar bestand.



<a name="cscfg"></a>
## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
De configuratie van de instellingen voor uw cloudservice wordt bepaald door de waarden in het bestand **ServiceConfiguration.cscfg** . U het aantal exemplaren die u wilt implementeren voor elke rol in dit bestand. De waarden voor de configuratie-instellingen die u hebt gedefinieerd in het definitiebestand service worden toegevoegd aan het configuratiebestand service. De vingerafdrukken voor alle management certificaten die zijn gekoppeld aan de cloudservice worden ook toegevoegd aan het bestand. Het [Azure Service configuratieschema (.cscfg bestand)](https://msdn.microsoft.com/library/azure/ee758710.aspx) vindt u de toegestane-indeling voor een configuratiebestand service.

Het configuratiebestand service niet wordt geleverd met de toepassing, maar wordt geüpload naar Azure als een afzonderlijk bestand en wordt gebruikt voor het configureren van de cloudservice. U kunt een nieuwe service-configuratiebestand uploaden zonder opnieuw te implementeren uw cloudservice. De configuratiewaarden voor de cloudservice kunnen worden gewijzigd terwijl de cloudservice wordt uitgevoerd. Het volgende voorbeeld ziet de configuratie-instellingen die kunnen worden gedefinieerd voor het Web en werknemer rollen:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

U gebruikt om te verwijzen naar de [Service configuratieschema](https://msdn.microsoft.com/library/azure/ee758710.aspx) voor informatie over het XML-schema hier gebruikt, maar hier is een snelle uitleg van de elementen:

**Exemplaren**  
Hiermee configureert u het aantal actieve exemplaren voor de rol. Als u wilt voorkomen dat uw cloudservice mogelijk niet langer beschikbaar bij upgrades, is het aanbevolen dat u meer dan één exemplaar van uw web-omlaag rollen implementeert. Hiermee geeft voldoet u aan de richtlijnen van de [Azure berekenen Service niveau overeenkomst (SLA)](http://azure.microsoft.com/support/legal/sla/), externe connectivity 99.95% voor internetgerichte rollen waarborgt wanneer twee of meer exemplaren van de rol van een service zijn geïmplementeerd.

**ConfigurationSettings**  
Hiermee configureert u de instellingen voor de actieve exemplaren voor een rol. De naam van de `<Setting>` elementen moeten overeenkomen met de instellingsdefinities in het definitiebestand service.

**Certificaten**  
Hiermee configureert u de certificaten die worden gebruikt door de service. Het vorige voorbeeld ziet u het definiëren van het certificaat voor de Remote Access-module. De waarde van het kenmerk *vingerafdruk* moet worden ingesteld op de vingerafdruk van het certificaat om te gebruiken.

<p/>

 >[AZURE.NOTE] De vingerafdruk voor het certificaat kan worden toegevoegd aan het configuratiebestand met behulp van een teksteditor, of de waarde kan worden toegevoegd op het tabblad **certificaten** van de pagina **Eigenschappen** van de rol in Visual Studio.



## <a name="defining-ports-for-role-instances"></a>Poorten naar exemplaren van de rol definiëren
Azure kunt slechts één ingangspunt aan een Webrol. Dit betekent dat al het verkeer vindt plaats via één IP-adres. U kunt uw websites als u wilt delen van een poort door het configureren van de koptekst host als u de aanvraag naar de juiste locatie wilt configureren. U kunt ook uw toepassingen moeten beluisteren bekende poorten op het IP-adres.

In het onderstaande voorbeeld ziet u de configuratie voor een Webrol met een toepassing voor de website- en web. De website is geconfigureerd als een locatie voor de standaard-vermelding op poort 80 en de webtoepassingen zijn geconfigureerd voor het aanvragen van een andere host koptekst die heet 'mail.mysite.cloudapp.net' ontvangen.

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" <mark>port="80"</mark> />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" <mark>hostheader="mail.mysite.cloudapp.net"</mark> />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>De configuratie van een rol wijzigen
Terwijl deze wordt uitgevoerd in Azure wordt aangegeven, zonder dat u offline u de service, kunt u de configuratie van uw cloudservice bijwerken. Als u wilt wijzigen configuratiegegevens, kunt u een nieuwe configuratiebestand uploaden of bewerken van het configuratiebestand op hun plaats staan en deze toepassen op uw actieve service. De volgende wijzigingen kunnen worden aangebracht in de configuratie van een service:

- **De waarden van de configuratie-instellingen wijzigen**  
Wanneer een configuratie-instelling van wijzigingen, een exemplaar van de rol kunt kiezen om de wijziging toepassen terwijl het exemplaar online is, of als u wilt het exemplaar zonder problemen Prullenbak en pas de wijziging terwijl het exemplaar offline is.

- **De service-topologie van de exemplaren die rol wijzigen**  
Wijzigingen in de netwerktopologie hebben geen invloed op actieve exemplaren, behalve waar een exemplaar wordt verwijderd. Alle resterende exemplaren in het algemeen hoeft niet gerecycled; u kunt echter Prullenbak exemplaren van de rol in antwoord op een topologiewijziging.

- **Vingerafdruk van het certificaat wijzigen**  
Wanneer een exemplaar van de rol offline is, kunt u alleen een certificaat bijwerken. Als een certificaat is toegevoegd, verwijderd of gewijzigd terwijl een exemplaar van de rol online is, gaat Azure zonder problemen het exemplaar offline naar het certificaat bijwerken en weer online brengen nadat de wijziging doorgevoerd is.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Verwerking van configuratiewijzigingen met Service Runtime gebeurtenissen
De [Azure Runtime Library](https://msdn.microsoft.com/library/azure/mt419365.aspx) bevat de naamruimte [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) klassen biedt voor de communicatie met de Azure-omgeving van de code die wordt uitgevoerd in een exemplaar van een rol. De klasse [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) definieert de volgende gebeurtenissen die zijn ingediend voor en na de configuratie aan te passen:

- **De gebeurtenis [wijzigen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx)**  
Dit gebeurt voordat het wijzigen van de configuratie wordt toegepast op een opgegeven exemplaar van een rol waarin u kunt de rol exemplaren ondernemen als tijdens vereist.
- **[Gewijzigde](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) gebeurtenis**  
Deze gebeurtenis vindt plaats nadat wijzigen van de configuratie is toegepast op een opgegeven exemplaar van een rol.

> [AZURE.NOTE] Omdat certificaat wijzigingen altijd offline te de exemplaren van een rol zetten, kunnen ze niet de gebeurtenissen RoleEnvironment.Changing of RoleEnvironment.Changed verhogen.

<a name="cspkg"></a>
## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Als u wilt een toepassing als een cloudservice in Azure implementeren, moet u eerst de toepassing in de juiste indeling pakket. **CSPack** hulpprogramma voor de opdrachtregel (geïnstalleerd met de [Azure SDK](https://azure.microsoft.com/downloads/)) kunt u het pakketbestand maakt als alternatief voor Visual Studio.

**CSPack** wordt de inhoud van de definitie servicebestand en configuratie van servicebestand definiëren van de inhoud van het pakket. **CSPack** genereert een toepassing pakketbestand (.cspkg) die u naar Azure uploaden kunt met behulp van de [Azure-portal](cloud-services-how-to-create-deploy-portal.md#create-and-deploy). Standaard het pakket heet `[ServiceDefinitionFileName].cspkg`, maar u kunt een andere naam opgeven met behulp van de `/out` optie van **CSPack**.

**CSPack** bevindt zich in het algemeen  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

>[AZURE.NOTE]
CSPack.exe (in windows) is beschikbaar door de **opdrachtprompt van Microsoft Azure** -snelkoppeling die is geïnstalleerd met de SDK uit te voeren.  
>  
>Het programma CSPack.exe uitvoeren door zelf om de documentatie over alle mogelijke schakelopties en opdrachten weer te geven.

<p />

>[AZURE.TIP]
Uw cloudservice lokaal wordt uitgevoerd in de **Microsoft Azure berekenen Emulator**, gebruikt u de **/copyonly** die deze optie worden de binaire bestanden voor de toepassing op een mapindeling waaruit ze kunnen worden uitgevoerd in de emulator berekeningscluster gekopieerd.

### <a name="example-command-to-package-a-cloud-service"></a>Opdracht naar een cloudservice als pakket Inpakken
Het volgende voorbeeld wordt een toepassingspakket met de gegevens voor een Webrol. De opdracht Hiermee geeft u de service-definitiebestand moet worden gebruikt, de map waar de binaire bestanden kunnen worden gevonden, en de naam van het pakketbestand.

    cspack [DirectoryName]\[ServiceDefinition]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /out:[OutputFileName]

Als de toepassing een Webrol zowel een rol werknemer bevat, wordt de volgende opdracht wordt gebruikt:

    cspack [DirectoryName]\[ServiceDefinition]
           /out:[OutputFileName]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]

Waar de variabelen worden als volgt gedefinieerd:

| Variabele                  | Waarde |
| ------------------------- | ----- |
| \[DirectoryName\]         | De submap onder de hoofdmap van het project dat het bestand .csdef van de Azure project bevat.|
| \[ServiceDefinition\]     | De naam van het definitiebestand service. Dit bestand heet standaard ServiceDefinition.csdef.  |
| \[OutputFileName\]        | De naam voor het pakketbestand wordt gegenereerd. Dit is meestal ingesteld op de naam van de toepassing. Als u geen bestandsnaam opgeeft, het toepassingspakket is gemaakt als \[ApplicationName\].cspkg.|
| \[Functienaam\]              | De naam van de rol zoals gedefinieerd in het definitiebestand service.|
| \[RoleBinariesDirectory] | De locatie van de binaire bestanden voor de rol.|
| \[VirtualPath\]           | De fysieke mappen voor elke virtuele pad gedefinieerd in de sectie Sites van de definitie van de service.|
| \[PhysicalPath\]          | De fysieke mappen van de inhoud voor elke virtuele pad gedefinieerd in het knooppunt site van de definitie van de service.|
| \[RoleAssemblyName\]      | De naam van het binair bestand voor de rol.| 


## <a name="next-steps"></a>Volgende stappen

Ik ben een servicepakket cloud maken en ik wil...

* [Extern bureaublad instellen voor een exemplaar van de service cloud][remotedesktop]
* [Een project Cloudservice implementeren][deploy]

Ik gebruik Visual Studio, maar ik wil …

* [Een nieuwe cloudservice maken][vs_create]
* [Een bestaande cloudservice configureren][vs_reconfigure]
* [Een project Cloudservice implementeren][vs_deploy]
* [Extern bureaublad instellen voor een exemplaar van de service cloud][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
