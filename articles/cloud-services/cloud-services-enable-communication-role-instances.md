<properties 
pageTitle="Communicatie voor rollen in Cloudservices | Microsoft Azure" 
description="Exemplaren van de rol in de Cloud Services kunnen eindpunten (http, https, tcp, udp) voor hen gedefinieerd die met de buitenkant of tussen de exemplaren van andere rol communiceren hebben." 
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

# <a name="enable-communication-for-role-instances-in-azure"></a>Communicatie inschakelen voor exemplaren van de rol in Azure wordt aangegeven

Cloud service rollen communiceren via de interne en externe verbindingen. Externe verbindingen heten **invoer eindpunten** terwijl interne verbindingen **interne eindpunten**worden genoemd. In dit onderwerp wordt beschreven hoe de [definitie van service](cloud-services-model-and-package.md#csdef) als u wilt maken van de eindpunten.


## <a name="input-endpoint"></a>Invoer-eindpunt
De invoer eindpunt wordt gebruikt wanneer u wilt een poort naar de buitenkant. U geeft het protocoltype en de poort van het eindpunt dat voor de interne en externe poorten voor het eindpunt geldt. Als u wilt, kunt u een andere interne poort voor het eindpunt opgeven met het kenmerk [LokalePoort](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) .

De invoer eindpunt de beschikking over de volgende protocollen: **http, https, tcp, udp**.

Als wilt maken van een invoer eindpunt, voegt u de **InputEndpoint** onderliggend element aan het element van de **eindpunten** van een web- of werknemer rol.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Exemplaar invoer eindpunt
Exemplaar invoer eindpunten zijn vergelijkbaar met input eindpunten, maar kunt u specifieke openbare poorten voor elk exemplaar van de afzonderlijke rol toewijzen via poort doorschakelen op de taakverdeling. U kunt één openbare poort of een bereik van poorten opgeven.

Het exemplaar invoer eindpunt kunt alleen **tcp** of **udp** gebruiken als het protocol.

Als wilt een exemplaar invoer eindpunt maken, voegt u de **InstanceInputEndpoint** onderliggend element aan het element van de **eindpunten** van een web- of werknemer rol.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Interne eindpunt
Interne eindpunten zijn beschikbaar voor communicatie exemplaar-naar-exemplaar. De poort is optioneel en indien weggelaten een dynamische poort is toegewezen aan het eindpunt. Een bereik van poorten kan worden gebruikt. Er is een limiet van vijf interne eindpunten per rol.

Het interne eindpunt de beschikking over de volgende protocollen: **http, tcp, udp, een**.

Als wilt maken van een interne invoer eindpunt, voegt u de **InternalEndpoint** onderliggend element aan het element van de **eindpunten** van een web- of werknemer rol.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

U kunt ook een poortbereik gebruiken.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Werknemer rollen versus Web rollen

Er is een secundaire verschil met de eindpunten tijdens het werken met zowel werknemer en web rollen. De Webrol moet ten minste een eenmalige invoer eindpunt via het **HTTP** -protocol.


```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>De .NET SDK gebruiken voor toegang tot een eindpunt
De bibliotheek Azure beheerde biedt methoden voor rol exemplaren om te communiceren tijdens runtime. Vanuit de code die binnen een exemplaar van de rol wordt uitgevoerd, kunt u gegevens over de aanwezigheid van andere rol exemplaren en de eindpunten, en informatie over het huidige exemplaar van de rol ophalen.

> [AZURE.NOTE] U kunt alleen informatie ophalen over rol exemplaren die worden uitgevoerd in de cloudservice en die ten minste één interne eindpunt te definiëren. U kunt geen gegevens over rol exemplaren uitgevoerd in een andere service.

U kunt de eigenschap [exemplaren](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) gebruiken om op te halen exemplaren van een rol. Gebruik eerst de [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) als resultaat een verwijzing naar het huidige exemplaar van de functie en gebruikt u de eigenschap [rol](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) als resultaat een verwijzing naar de rol zelf.

Wanneer u verbinding met een exemplaar van de rol via een programma via de .NET SDK maakt, hoeft u relatief eenvoudig toegang tot alle eindpuntgegevens. Nadat u al met een specifieke rol-omgeving verbonden hebt, kunt u bijvoorbeeld de poort van een bepaald eindpunt met deze code opvragen:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

De eigenschap **exemplaren** geeft als resultaat een verzameling **RoleInstance** objecten. Deze verzameling bevat altijd het huidige exemplaar. Als de rol geen een interne eindpunt gedefinieerd, wordt de collectie bevat het huidige exemplaar, maar geen andere exemplaren. Het aantal exemplaren van de rol in de siteverzameling zijn altijd 1 in het geval waarbij geen interne eindpunt is gedefinieerd voor de rol. Als de rol een interne eindpunt definieert, de exemplaren die kunnen worden gevonden tijdens runtime en het aantal exemplaren in de verzameling komt overeen met het aantal exemplaren die zijn opgegeven voor de rol in het configuratiebestand service.

> [AZURE.NOTE] De bibliotheek Azure beheerde biedt geen een manier om te bepalen de status van de andere rol exemplaren, maar u kunt implementeren deze beoordeling systeemstatus uzelf als uw service nodig deze functionaliteit heeft. U kunt [Azure diagnostische hulpprogramma's](cloud-services-dotnet-diagnostics.md) voor informatie over het uitvoeren van exemplaren van de rol.

Om te bepalen het poortnummer voor een interne eindpunt op een exemplaar van de rol, kunt u de eigenschap [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) om terug te keren een woordenlijstobject dat eindpunt namen en hun bijbehorende IP-adressen en poorten bevat. De eigenschap [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) retourneert de IP-adres en poort voor een opgegeven eindpunt. De eigenschap **PublicIPEndpoint** geeft als resultaat de poort voor een taakverdeling-eindpunt. Het gedeelte IP-adres van de eigenschap **PublicIPEndpoint** wordt niet gebruikt.

Hier volgt een voorbeeld die wordt herhaald rol exemplaren.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Hier volgt een voorbeeld van een werknemer rol die wordt het eindpunt die worden aangeboden door de servicedefinitie en wordt begonnen met gebruikers zonder licentie voor verbindingen.

> [AZURE.WARNING] Deze code werkt alleen voor een uitgevouwen service. Wanneer u zich in de Emulator voor het berekenen van Azure, worden service-configuratie-elementen die directe poort eindpunten (**InstanceInputEndpoint** elementen maakt) genegeerd.

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;
        
        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;
                
        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);
                
        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Regels voor netwerk-verkeer om te bepalen rol communicatie
Nadat u interne eindpunten definieert, kunt u regels voor netwerk-verkeer (gebaseerd op de eindpunten die u hebt gemaakt) toevoegen om te bepalen hoe de rol exemplaren met elkaar kunnen communiceren. In het volgende diagram ziet u enkele veelvoorkomende scenario's voor rol communicatie bepalen:

![Netwerkverkeer regels scenario 's] (./media/cloud-services-enable-communication-role-instances/scenarios.png "Netwerkverkeer regels scenario 's")

Het volgende voorbeeld ziet u roldefinities voor de rollen die wordt weergegeven in het vorige diagram. De roldefinitie van elke bevat ten minste één interne eindpunt gedefinieerd:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
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
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [AZURE.NOTE] Beperking van de communicatie tussen rollen kan zich voordoen bij interne eindpunten van beide vaste en automatisch toegewezen poorten.

Standaard nadat er is een interne eindpunt gedefinieerd, kunt communicatie flow uit een rol naar het interne eindpunt van een rol zonder beperkingen. Als u wilt de communicatie beperken, moet u een element **NetworkTrafficRules** toevoegen aan het element **ServiceDefinition** in het definitiebestand service.

### <a name="scenario-1"></a>Scenario 1
Alleen mogen wijzigen netwerkverkeer van **WebRole1** **WorkerRole1**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Scenario 2
Staat slechts netwerkverkeer van **WebRole1** naar **WorkerRole1** en **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Scenario 3
Staat slechts netwerkverkeer van **WebRole1** naar **WorkerRole1**en **WorkerRole1** naar **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Scenario 4
Staat slechts netwerkverkeer van **WebRole1** naar **WorkerRole1**, **WebRole1** naar **WorkerRole2**en **WorkerRole1** naar **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Een XML-schema-verwijzing voor de elementen boven vindt u [hier](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het Cloudservice- [model](cloud-services-model-and-package.md).