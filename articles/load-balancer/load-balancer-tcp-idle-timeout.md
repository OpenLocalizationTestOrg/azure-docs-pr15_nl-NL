<properties
   pageTitle="Laden de verdeling van TCP-inactiviteit configureren | Microsoft Azure"
   description="Laden de verdeling van TCP-inactiviteit configureren"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>TCP inactiviteit instellingen configureren voor de verdeling van Azure belasting

Azure-taakverdeling heeft in de standaardconfiguratie, een outinstellingen van 4 minuten. Als een periode van inactiviteit langer dan de time-outwaarde is, is er geen garantie die de sessie TCP of HTTP wordt bijgehouden tussen de client en uw cloudservice.

Wanneer de verbinding is verbroken, wordt de clienttoepassing het volgende foutbericht weergegeven: "de onderliggende verbinding is gesloten: een verbinding verwachte leven wordt gehouden door de server is gesloten."

Een gebruikelijk is om een permanente TCP te gebruiken. Deze procedure blijft de verbinding actief voor een langere periode. Zie voor meer informatie in deze [voorbeelden van .NET](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Met permanente ingeschakeld, pakketten worden verzonden tijdens de periode van inactiviteit in de verbinding. Deze permanente pakketten Zorg ervoor dat de niet-actieve time-outwaarde nooit is bereikt en de verbinding wordt bijgehouden voor een lange periode.

Deze instelling werkt voor alleen binnenkomende verbindingen. Om te voorkomen dat de verbinding verliezen, moet u de permanente TCP met een interval kleiner is dan de outinstellingen configureren of de niet-actieve time-outwaarde vergroten. Als u wilt deze scenario's wordt ondersteund, er is ondersteuning voor een configureerbare inactiviteit toegevoegd. U kunt deze nu instellen voor een duur van 4 tot 30 minuten.

TCP permanente werkt ook voor scenario's waar levensduur niet een beperking is. Het wordt niet aanbevolen voor mobiele toepassingen. Met een permanente in een mobiele toepassing TCP laten de kleine apparaat sneller.

![TCP-out](./media/load-balancer-tcp-idle-timeout/image1.png)

De volgende secties wordt beschreven hoe inactiviteit-instellingen wijzigen in virtuele machines en cloud services.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>De TCP-time-out configureren voor uw exemplaar niveau openbare IP-adres 15 minuten

    Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15

`IdleTimeoutInMinutes`is optioneel. Als dit niet is ingesteld, is de standaard-time-out 4 minuten. Het bereik time-out voor acceptabel is 4 tot 30 minuten.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Inactiviteit instellen bij het maken van een Azure eindpunt op een virtuele machine

U kunt de instelling time-out voor een eindpunt wijzigen, gebruikt u de volgende:

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM

Gebruik de volgende opdracht uit om op te halen uw configuratie inactiviteit:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>De TCP-time-out op een set van taakverdeling eindpunt instellen

Als eindpunten deel van een set eindpunt verdeeld uitmaken, moet de TCP-out worden ingeschakeld voor het instellen van taakverdeling eindpunt. Bijvoorbeeld:

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15

## <a name="change-timeout-settings-for-cloud-services"></a>Time-outinstellingen voor cloudservices wijzigen

U kunt de SDK Azure bijwerken van uw cloudservice. U aanbrengen eindpunt instellingen voor cloudservices in het bestand .csdef. Bijwerken van de TCP-out voor de implementatie van een cloudservice, moet een upgrade implementatie. Een uitzondering is als de time-out TCP alleen voor een openbare IP-adres is opgegeven. Openbare IP-instellingen in het bestand .cscfg zijn en u ze kunt bijwerken door de update van de implementatie- en.

De wijzigingen .csdef voor eindpunt instellingen zijn:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
      </Endpoints>
    </WorkerRole>

De wijzigingen .cscfg voor de time-out op openbare IP-adressen zijn:

    <NetworkConfiguration>
      <VirtualNetworkSite name="VNet"/>
      <AddressAssignments>
        <InstanceAddress roleName="VMRolePersisted">
        <PublicIPs>
          <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
        </PublicIPs>
        </InstanceAddress>
      </AddressAssignments>
    </NetworkConfiguration>

## <a name="rest-api-example"></a>Voorbeeld van de REST API

U kunt de niet-actieve TCP-time-out configureren met behulp van de API voor het servicebeheer. Zorg ervoor dat de `x-ms-version` koptekst is ingesteld op versie `2014-06-01` of hoger. Werk de configuratie van de opgegeven taakverdeling invoer eindpunten op alle virtuele machines in een implementatie.

### <a name="request"></a>Aanvragen

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Antwoord

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
        <LocalPort>local-port-number</LocalPort>
        <Port>external-port-number</Port>
        <LoadBalancerProbe>
          <Path>path-of-probe</Path>
          <Port>port-assigned-to-probe</Port>
          <Protocol>probe-protocol</Protocol>
          <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
          <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
        </LoadBalancerProbe>
        <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
        <Protocol>endpoint-protocol</Protocol>
        <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
        <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
        <EndpointACL>
          <Rules>
            <Rule>
              <Order>priority-of-the-rule</Order>
              <Action>permit-rule</Action>
              <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
              <Description>description-of-the-rule</Description>
            </Rule>
          </Rules>
        </EndpointACL>
      </InputEndpoint>
    </LoadBalancedEndpointList>

## <a name="next-steps"></a>Volgende stappen

[Overzicht van de verdeling van interne laden](load-balancer-internal-overview.md)

[Aan de slag een taakverdeling internetgerichte configureren](load-balancer-get-started-internet-arm-ps.md)

[Een modus voor het laden gelijkmatige verdeling configureren](load-balancer-distribution-mode.md)
