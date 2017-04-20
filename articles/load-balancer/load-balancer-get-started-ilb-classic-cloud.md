<properties
   pageTitle="Een interne taakverdeling voor cloudservices maken in het implementatiemodel klassieke | Microsoft Azure"
   description="Informatie over het maken van een interne taakverdeling via PowerShell in het implementatiemodel klassieke"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Een interne taakverdeling (klassieke) te maken voor cloudservices aan de slag

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

<BR>

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe [u deze stappen uitvoert met het model resourcemanager](load-balancer-get-started-ilb-arm-ps.md).


## <a name="configure-internal-load-balancer-for-cloud-services"></a>Interne taakverdeling voor cloudservices configureren

Interne taakverdeling wordt ondersteund voor zowel virtuele machines en cloudservices. Een interne laden verdeling eindpunt gemaakt in een cloudservice die zich buiten een regionale virtueel netwerk zijn toegankelijk is alleen vanuit de cloudservice.

De configuratie van de verdeling van interne laden heeft worden ingesteld tijdens het maken van de eerste implementatie in de cloudservice, zoals wordt weergegeven in het onderstaande voorbeeld.

>[AZURE.IMPORTANT] Een vereiste voor het uitvoeren van de onderstaande stappen is dat een virtueel netwerk al hebt gemaakt voor de cloud-implementatie. Moet u de virtuele netwerknaam en de subnetnaam maken de interne taakverdeling.

### <a name="step-1"></a>Stap 1

Open de service-configuratiebestand (.cscfg) voor de cloud-implementatie in Visual Studio en toevoegen van de volgende sectie als u wilt maken van de interne taakverdeling onder de laatste "`</Role>`" item voor de netwerkconfiguratie.




    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="name of the load balancer">
          <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>


Laten we telt de waarden voor het netwerk configuratiebestand om weer te geven hoe deze eruitziet. In het voorbeeld wordt ervan uitgegaan dat u hebt gemaakt met een subnet "test_vnet" naam met een subnet 10.0.0.0/24 test_subnet en een statische IP-adres 10.0.0.4 genoemd. De taakverdeling naam testLB.

    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="testLB">
          <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>

Zie voor meer informatie over het schema van de verdeling van laden, [de belasting voor documentconversies toevoegen](https://msdn.microsoft.com/library/azure/dn722411.aspx).

### <a name="step-2"></a>Stap 2


De definitie (.csdef) servicebestand om toe te voegen eindpunten naar het interne taakverdeling wijzigen. Het tijdstip waarop dat het exemplaar van een rol is gemaakt, wordt het definitiebestand service de rol exemplaren toevoegen aan het interne taakverdeling.


    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
      </Endpoints>
    </WorkerRole>

Na de dezelfde waarden uit het vorige voorbeeld, moet u laten we de waarden toevoegen aan de definitie servicebestand.

    <WorkerRole name=WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
      </Endpoints>
    </WorkerRole>

Het netwerkverkeer is verdeeld met de testLB taakverdeling met poort 80 voor verzoeken voor oproepen, verzenden naar een werknemer rol exemplaren ook op poort 80.


## <a name="next-steps"></a>Volgende stappen

[Een laden gelijkmatige verdeling modus bron IP-affiniteit met configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)