<properties
   pageTitle="Configureren van taakverdeling verdeling modus | Microsoft Azure"
   description="Het configureren van Azure laden gelijkmatige verdeling modus ter ondersteuning van de bron-IP-affiniteit"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="configure-the-distribution-mode-for-load-balancer"></a>De modus verdeling voor taakverdeling configureren

## <a name="hash-based-distribution-mode"></a>Verdeling op basis van hash-modus

De algoritme van de standaardverdeling is een 5-tupel (bron-IP, bronpoort doel-IP-, doelpoort type protocol) hash moet worden toegewezen verkeer beschikbare servers. Deze wordt kleverigheid alleen binnen een transportsessie. Pakketten in dezelfde sessie doorgestuurd naar dezelfde datacenter IP-(DIP)-sessie achter het eindpunt van taakverdeling. Als de client een nieuwe sessie van de dezelfde bron-IP start, wordt de bronpoort gewijzigd en wordt het verkeer naar een ander DIP-eindpunt.

![hash op basis van taakverdeling](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Afbeelding 1-5-tupel-verdeling

## <a name="source-ip-affinity-mode"></a>Affiniteitsmodus bron-IP

We hebben een andere verdeling modus bron IP-affiniteit (ook wel bekend als sessie affiniteit of IP-clientaffiniteit) genoemd. Azure taakverdeling kan worden geconfigureerd voor gebruik van een 2-tupel (bron-IP, doel-IP-) of 3-tupel (bron-IP, doel-, IP-Protocol) verkeer toewijzen aan de beschikbare servers. Met behulp van de bron-IP affiniteit gaat verbindingen gestart vanaf dezelfde clientcomputer naar hetzelfde DIP eindpunt.

In het volgende diagram ziet u een 2-tupel-configuratie. Zoals u ziet hoe de 2-tupel tot en met de taakverdeling met virtuele machine 1 (VM1) die vervolgens back-up door VM2 en VM3 gemaakt wordt wordt uitgevoerd.

![sessie affiniteit](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Afbeelding 2-2-tupel-verdeling

Bron-IP-affiniteit is opgelost incompatibiliteit tussen de Azure taakverdeling en het externe bureaublad (RD) Gateway. U kunt nu een RD gateway-farm in een enkel cloudservice maken.

Een ander scenario voor het geval van gebruik is media uploaden waar de gegevens upload gebeurt via UDP, maar het vlak van het besturingselement wordt bereikt tot en met TCP:

- Een client eerst een TCP-sessie omgezet in de openbare adres van taakverdeling start, wordt doorgestuurd naar een specifieke DIP dit kanaal blijft de status van de verbinding kunnen controleren
- Een nieuwe UDP-sessie vanuit dezelfde clientcomputer wordt gestart aan hetzelfde taakverdeling openbare eindpunt, de verwachting hier is dat deze verbinding, ook wordt doorgestuurd naar hetzelfde DIP eindpunt, zoals de vorige TCP-verbinding zodat media uploaden kan worden uitgevoerd bij hoge doorvoer behoud van ook een kanaal besturingselement tot en met TCP.

>[AZURE.NOTE] Wanneer een set taakverdeling wordt gewijzigd (verwijderen of toevoegen van een virtuele machine), de verdeling van clientverzoeken opnieuw berekend. U kunt geen hangt af van nieuwe verbindingen van bestaande clients op dezelfde server loopt. Daarnaast kunnen door bron-IP verdeling affiniteitsmodus mogelijk dat een ongelijke verdeling van verkeer. Clients met achter proxy's kunnen worden beschouwd als één unieke clienttoepassing.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Configuratie van bron-IP affiniteit de instellingen voor de belasting voor documentconversies

Voor virtuele machines, kunt u PowerShell time-outinstellingen wijzigen:

Een eindpunt Azure toevoegen aan een virtuele Machine en laden gelijkmatige verdeling modus instellen

    Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM

LoadBalancerDistribution kan worden ingesteld op sourceIP voor 2-tupel (bron-IP, doel-IP-) taakverdeling, sourceIPProtocol voor taakverdeling 3-tupel (bron-IP, doel-, IP-protocol) of geen als u wilt dat het standaardgedrag van taakverdeling 5-tupel.

Gebruik de volgende manieren te werk om op te halen een configuratie eindpunt laden gelijkmatige verdeling modus:

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

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
    LoadBalancerDistribution : sourceIP

Als de LoadBalancerDistribution-element niet aanwezig is wordt de Azure taakverdeling de algoritme van de standaard-5-tupel.

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a>Stel de verdeling-modus op een set van taakverdeling eindpunt

Als de eindpunten deel uitmaken van een set van taakverdeling eindpunt, moet de modus verdeling worden ingesteld op de set van taakverdeling eindpunt:

    Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP

### <a name="cloud-service-configuration-to-change-distribution-mode"></a>Configuratie van de cloud-Service verdeling modus wijzigen

U kunt gebruikmaken van de Azure SDK voor .NET-2,5 (om te worden uitgebracht in November) als u wilt bijwerken van uw Cloudservice. Eindpunt-instellingen voor Cloudservices worden aangebracht in de .csdef. Een upgrade implementatie is om te werken in de modus laden gelijkmatige verdeling voor een Cloud Services-implementatie, vereist.
Hier volgt een voorbeeld van .csdef wijzigingen voor eindpunt instellingen:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
      </Endpoints>
    </WorkerRole>
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

## <a name="api-example"></a>API-voorbeeld

U kunt de belasting gelijkmatige verdeling met de API voor het servicebeheer configureren. Controleer of om toe te voegen de `x-ms-version` koptekst is ingesteld op versie `2014-09-01` of hoger.

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a>De configuratie van de opgegeven taakverdeling set in een implementatie bijwerken

#### <a name="request-example"></a>Voorbeeld van de aanvraag

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet    x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

De waarde van LoadBalancerDistribution kan zijn sourceIP voor 2-tupel affiniteit, sourceIPProtocol voor 3-tupel affiniteit of geen (voor geen affiniteit. dat wil zeggen 5-tupel)

#### <a name="response"></a>Antwoord

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Volgende stappen

[Overzicht van de verdeling van interne laden](load-balancer-internal-overview.md)

[Aan de slag een Internet die tegenover elkaar liggen taakverdeling configureren](load-balancer-get-started-internet-arm-ps.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
