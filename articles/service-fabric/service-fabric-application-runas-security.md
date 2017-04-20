<properties
   pageTitle="Informatie over de toepassing voor de Service stof en service beveiligingsbeleid voor apparaten | Microsoft Azure"
   description="Een overzicht van het uitvoeren van een toepassing voor de Service stof Klik onder systeem en lokale accounts, inclusief het SetupEntry punt waar een toepassing enkele bevoegdheden actie uitvoeren moet voordat u begint"
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
   ms.date="09/22/2016"
   ms.author="mfussell"/>

# <a name="configure-security-policies-for-your-application"></a>Beveiligingsbeleid voor apparaten voor uw toepassing configureren
Azure-Service stof biedt de mogelijkheid om te beveiligen van toepassingen die in het cluster onder verschillende gebruikersaccounts. Service stof beveiligt ook de resources die worden gebruikt door de toepassingen op het moment van implementatie onder het gebruikersaccount zoals bestanden, mappen en certificaten. Hierdoor actieve toepassingen, zelfs in een gedeelde gehoste omgeving, beveiligde van elkaar. 

Standaard stof Service-toepassingen worden uitgevoerd onder het account dat het proces Fabric.exe wordt uitgevoerd onder. Service stof biedt ook de mogelijkheid om uit te voeren toepassingen onder een lokale gebruikersaccount of lokale systeemaccount in van de toepassing manifest hebt opgegeven. Accounttypen ondersteunde lokaal systeem voor zijn **lokalegebruiker**, **Netwerkservice** **lokale**en **Lokaal systeem**.

 Wanneer u zich Service stof in Windows Server in uw datacenter met het installatieprogramma zelfstandige, kunt u domeinaccounts AD (Active Directory).

Gebruikersgroepen kunnen worden gedefinieerd en gemaakt zodat een of meer gebruikers kunnen worden toegevoegd aan elke groep samen worden beheerd. Dit is handig wanneer er meerdere gebruikers verschillende service invoer wordt verwezen en ze moeten zijn bepaalde algemene bevoegdheden die beschikbaar op het groepeerniveau van de zijn.

## <a name="configure-the-policy-for-service-setupentrypoint"></a>Het beleid voor SetupEntryPoint-service configureren

Zoals is beschreven in het [toepassingsmodel](service-fabric-application-model.md) is de **SetupEntryPoint** een bevoegdheden ingangspunt die compatibel is met dezelfde referenties als Service stof (meestal de *Netwerkservice* -account) voordat u andere ingangspunt. Het uitvoerbare bestand opgegeven door **ingangspunt** is meestal de langdurige ServiceHost, dus met een afzonderlijke instellingen ingangspunt dat u voorkomt moet uitvoeren van de ServiceHost uitvoerbare met hoge bevoegdheden voor langere tijd. Het uitvoerbare bestand opgegeven door **ingangspunt** wordt uitgevoerd nadat **SetupEntryPoint** is afgesloten. De resulterende proces wordt beheerd en opnieuw is opgestart, opnieuw beginnen met **SetupEntryPoint**, als dit ooit wordt beëindigd of loopt vast.

Hierna volgt een voorbeeld van eenvoudige service manifest waarop u de SetupEntryPoint en de belangrijkste ingangspunt voor de service.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
~~~

### <a name="configure-the-policy-using-a-local-account"></a>Het beleid met een lokale account configureren

Nadat u de service zodat u een instelling ingangspunt hebt geconfigureerd, kunt u de machtigingen die wordt uitgevoerd onder in het toepassingsmanifest wijzigen. Het volgende voorbeeld ziet hoe u de service uitvoeren onder gebruikersbevoegdheden beheerder-account configureren.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
~~~

Maak eerst een sectie **principes** met de gebruikersnaam van een, bijvoorbeeld SetupAdminUser. Hiermee wordt aangeduid dat de gebruiker lid zijn van de groep Administrators systeem is.

Klik vervolgens onder de sectie **ServiceManifestImport** een beleid te deze hoofdsom toepassen op **SetupEntryPoint**configureren. Hiermee wordt aangegeven Service stof wanneer het bestand **MySetup.bat** wordt uitgevoerd, het RunAs met beheerdersbevoegdheden zijn moet. Gezien het feit dat u hebt een beleid *niet* toegepast op het punt hoofdvermelding, de code in **MyServiceHost.exe** compatibel is met het systeem **Netwerkservice** -account. Dit is het standaardaccount die worden uitgevoerd via alle service invoer wordt verwezen.

Nu we het bestand MySetup.bat toevoegen aan het project Visual Studio om te testen van de beheerdersbevoegdheden. In Visual Studio, met de rechtermuisknop op de service-project en voeg een nieuw bestand MySetup.bat genoemd.

Controleer vervolgens of het bestand MySetup.bat is opgenomen in de servicepakket. Standaard is deze niet. Selecteer het bestand, met de rechtermuisknop op om het snelmenu en kies **Eigenschappen**. Zorg ervoor dat **kopiëren naar uitvoer Directory** is ingesteld op **kopiëren als nieuwere**in het dialoogvenster Eigenschappen. Zie de volgende schermopname.

![Visual Studio CopyToOutput voor SetupEntryPoint batchbestand][image1]

Nu opent u het bestand MySetup.bat en toevoegen van de volgende opdrachten:

~~~
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
~~~

Vervolgens bouwen en implementeren van de oplossing aan een cluster plaatselijke ontwikkeling.  Zodra de service is gestart, zoals wordt weergegeven in de Service stof Explorer, kunt u zien dat de MySetup.bat geslaagd in een op twee manieren. Open een opdrachtprompt PowerShell en typ:

~~~
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
~~~

Noteer de naam van het knooppunt waar de service is geïmplementeerd en gestart in de Verkenner Service stof bijvoorbeeld knooppunt 2. Ga vervolgens naar de map application exemplaar werk het out.txt-bestand met de waarde van **TestVariable**te vinden. Voor voorbeeld als deze service is geïmplementeerd op knooppunt 2 en u u naar dit pad voor het **MyApplicationType gaat kunt**:

~~~
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
~~~

###  <a name="configure-the-policy-using-local-system-accounts"></a>Het beleid via lokaal systeemaccounts configureren
Vaak verdient het opstartscript gebruikt in een lokale systeemaccount in plaats van een beheerdersaccount zoals voorafgaand aan de wilt uitvoeren. Met de RunAs werkt beleid als beheerders meestal niet goed aangezien machines gebruikers toegang Gebruikersaccountbeheer (UAC) is standaard ingeschakeld hebben. In dit geval **de aanbeveling als wordt uitgevoerd de SetupEntryPoint lokaal systeem in plaats van een lokale gebruiker toegevoegd aan de beheerdersgroep**. Het volgende voorbeeld ziet de SetupEntryPoint om uit te voeren als lokaal systeem instellen.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
~~~

##  <a name="launch-powershell-commands-from-a-setupentrypoint"></a>PowerShell-opdrachten uit een SetupEntryPoint starten
Als u wilt uitvoeren PowerShell vanaf het punt **SetupEntryPoint** , kunt u **PowerShell.exe** uitvoeren in een batchbestand die naar een PowerShell-bestand verwijst. Eerst een PowerShell-bestand toevoegen aan het project service, zoals **MySetup.ps1**. Onthoud dat de eigenschap *kopiëren als nieuwere* instellen zodat het bestand ook in de servicepakket is opgenomen. Het volgende voorbeeld ziet u een voorbeeldbestand van de batch starten een PowerShell-bestand genaamd MySetup.ps1, die Hiermee stelt u de omgevingsvariabele van een systeem **TestVariable**genoemd.


MySetup.bat aan starten PowerShell-bestand.

~~~
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
~~~

Voeg het volgende om in te stellen van een systeemomgevingsvariabele in het bestand PowerShell:

~~~
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
~~~

**Notitie:** Standaard wanneer het batchbestand wordt uitgevoerd kijkt deze naar de map application bestanden aangeroepen **werken** . In dit geval wanneer MySetup.bat wordt uitgevoerd willen we Hiermee kunt u de MySetup.ps1 zoeken in dezelfde map, dat wil de map application **code-pakket zeggen** . Instellen als u wilt deze map wijzigen, de werkmap zoals wordt weergegeven in de volgende.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
~~~

## <a name="using-console-redirection-for-local-debugging"></a>Consoleomleiding voor lokale foutopsporing gebruiken
Soms is het handig om de console-uitvoer van het uitvoeren van een script voor foutopsporing weer te geven. Hiervoor kunt u een beleid voor het omleiding van console, waarin de uitvoer naar een bestand geschreven instellen. De bestandsuitvoer is geschreven naar de map application **log** genoemd op het knooppunt waar de toepassing is geïmplementeerd en uitgevoerd (Zie waar om dit te vinden in het voorgaande voorbeeld).

**Notitie: nooit** het beleid van console omleiding gebruiken in een toepassing geïmplementeerd in productie, aangezien dit van invloed op de overname van toepassing zijn kan. **Alleen** Gebruik deze opdracht voor lokale ontwikkeling en foutopsporing.  

Het volgende voorbeeld ziet de console-omleiding met een waarde FileRetentionCount instellen.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
~~~

Als u het bestand MySetup.ps1 u schrijft een opdracht **Echo** nu wijzigt, wordt dit naar het uitvoerbestand voor foutopsporing schrijven.

~~~
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
~~~

**Nadat u uw script hebt gecontroleerd, onmiddellijk dit beleid console-omleiding verwijderen**

## <a name="configure-policy-for-service-code-packages"></a>Beleid voor code servicepakketten configureren 
In de voorgaande stappen, u hebt gezien RunAs beleid toepassen op SetupEntryPoint. Laten we even naar iets grondigere bij het maken van verschillende principes die kunnen worden toegepast als de service-beleid.

### <a name="create-local-user-groups"></a>Lokale gebruikersgroepen maken
Gebruikersgroepen kunnen worden gedefinieerd en gemaakt die een of meer gebruikers toestaan deel te worden toegevoegd aan een groep. Dit is vooral handig als er meerdere gebruikers verschillende service invoer wordt verwezen en ze moeten zijn bepaalde algemene bevoegdheden die beschikbaar op het groepeerniveau van de zijn. Het volgende voorbeeld ziet u een groep met de naam van **LocalAdminGroup** met beheerdersbevoegdheden. Twee gebruikers, Customer1 en Customer2, worden lid van deze groep gemaakt.

~~~
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
~~~

### <a name="create-local-users"></a>Lokale gebruikers maken
U kunt een lokale gebruiker die kan worden gebruikt voor het beveiligen van een service in de toepassing maken. Wanneer een accounttype **lokalegebruiker** is opgegeven in de sectie principes van manifest van de toepassing, Service stof wordt gemaakt van lokale gebruikersaccounts op computers waarop de toepassing wordt geïmplementeerd. Deze accounts hebben standaard geen dezelfde naam als die zijn opgegeven in de manifest van de toepassing (bijvoorbeeld Customer3 in het onderstaande voorbeeld). In plaats daarvan deze dynamisch worden gegenereerd en willekeurige wachtwoorden hebt.

~~~
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
~~~

<!-- If an application requires that the user account and password be same on all machines (for example, to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true. The cluster manifest must also specify an NTLMAuthenticationPasswordSecret that will be used to generate the same password across all machines.

<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
-->

### <a name="assign-policies-to-the-service-code-packages"></a>Beleid toewijzen aan de code-servicepakketten
De sectie **RunAsPolicy** voor een **ServiceManifestImport** Hiermee geeft u het account uit de sectie principes die moet worden gebruikt voor het uitvoeren van een code-pakket. Ook wordt code-pakketten van de servicemanifest gekoppeld aan gebruikersaccounts in de sectie principes. U kunt dit opgeven voor de installatie of hoofdvermelding wordt verwezen, of u kunt opgeven `All` op beide toepassen. Het volgende voorbeeld ziet verschillende beleid wordt toegepast:

~~~
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
~~~

Als **EntryPointType** niet is opgegeven, de standaardinstelling is ingesteld op EntryPointType = "Hoofdgegeven". Opgeven **SetupEntryPoint** is vooral handig als u wilt uitvoeren van bepaalde bewerking van de instelling hoog bevoegdheidsniveau onder een systeemaccount. De werkelijke servicecode kan worden uitgevoerd binnen een kleine bevoegdheidsniveau-account.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Een standaardbeleid toepassen op alle servicepakketten code
De sectie **DefaultRunAsPolicy** wordt gebruikt om op te geven van een standaard-gebruikersaccount voor alle code-pakketten die geen een specifieke **RunAsPolicy** gedefinieerd hebben. Als het grootste deel van de code-pakketten dat is opgegeven in servicemanifest die worden gebruikt door een toepassing wilt uitvoeren onder dezelfde gebruiker, kunt een standaardbeleid RunAs met die account alleen in de toepassing definiëren. Het volgende voorbeeld wordt ingesteld dat het pakket code onder de **MyDefaultAccount** die is opgegeven in de sectie principes wordt uitgevoerd als een pakket code geen een **RunAsPolicy** die is opgegeven.

~~~
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
~~~
### <a name="using-an-active-directory-domain-group-or-user"></a>Gebruik van een gebruiker of groep voor Active Directory-domein
Service stof geïnstalleerd op Windows Server met het installatieprogramma van zelfstandige kunt u, de service onder de referenties voor een AD (Active Directory)-gebruiker of groepsaccount uitvoeren. Opmerking: Dit is Active Directory on-premises binnen uw domein en is niet met Azure Active Directory (AAD). Met behulp van een domeingebruiker of groep, kunt u andere bronnen in het domein (bijvoorbeeld bestandsshares) die machtigingen zijn toegekend vervolgens openen.

Het volgende voorbeeld ziet u een AD-gebruiker *TestUser* aangeroepen met hun domeinwachtwoord versleuteld met een certificaat *MyCert*genoemd. U kunt de `Invoke-ServiceFabricEncryptText` Powershell-opdracht om te maken van de geheime versleutelde tekst. Zie dit artikel [geheimen in stof Service-toepassingen beheren](service-fabric-application-secret-management.md) voor meer informatie over. De persoonlijke sleutel van het certificaat decoderen het wachtwoord moet worden geïmplementeerd op de lokale computer in een methode out-van-band (in Azure dit via de Resource Manager is). Vervolgens wanneer het pakket met service-Service stof in de computer implementeren, hoeft u kunnen ontsleutelen het geheim en samen met de gebruikersnaam in te voeren, verifiëren met AD onder deze referenties worden uitgevoerd.

~~~
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
~~~


## <a name="assign-securityaccesspolicy-for-http-and-https-endpoints"></a>SecurityAccessPolicy voor HTTP en HTTPS eindpunten toewijzen
Als u een beleid RunAs op een service toepassen en servicemanifest gedeclareerd eindpunt resources met het HTTP-protocol, moet u een **SecurityAccessPolicy** om ervoor te zorgen dat poorten toegewezen aan deze eindpunten correct toegangsbeheer vermeld voor de RunAs-gebruikersaccount die compatibel is zijn met de service. Anders **http** heeft geen toegang tot de service en krijgt u fouten met gesprekken van de client. Het volgende voorbeeld wordt het account Customer3 geldt voor een eindpunt genoemd **ServiceEndpointName**, door deze te machtigen voor volledige toegang.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
~~~

Voor het eindpunt HTTPS hebt u ook om aan te geven van de naam van het certificaat om terug te keren naar de klant. U kunt dit doen met behulp van **EndpointBindingPolicy**, met het certificaat dat is gedefinieerd in een sectie certificaten in manifest van de toepassing.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
~~~


## <a name="a-complete-application-manifest-example"></a>Een volledige toepassing manifest voorbeeld
De manifest van de volgende toepassing ziet veel van de verschillende instellingen:

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
~~~


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het toepassingsmodel](service-fabric-application-model.md)
* [Resources in een servicemanifest opgeven](service-fabric-service-manifest-resources.md)
* [Een toepassing implementeren](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
