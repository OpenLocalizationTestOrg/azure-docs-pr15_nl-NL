<properties
   pageTitle="Aan de slag voor het maken van een internetverbinding van taakverdeling in klassieke implementatie model met voor cloudservices | Microsoft Azure"
   description="Informatie over het maken van een internetverbinding van taakverdeling in klassieke implementatiemodel cloudservices"
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
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Aan de slag voor het maken van een internetverbinding van taakverdeling voor cloudservices

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel heeft betrekking op het implementatiemodel klassieke. U kunt ook [informatie over het maken van taakverdeling met Azure Resource Manager van een internetverbinding](load-balancer-get-started-internet-arm-cli.md).

Cloudservices worden automatisch geconfigureerd met een taakverdeling en kunnen worden aangepast via de ServiceModel.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Een taakverdeling met behulp van de service definitie-bestand maken

U kunt gebruikmaken van de Azure SDK voor .NET-2,5 naar het bijwerken van uw cloudservice. Eindpunt-instellingen voor cloudservices worden aangebracht in de [service](https://msdn.microsoft.com/library/azure/gg557553.aspx)definitiebestand .csdef.

Het volgende voorbeeld ziet hoe een servicedefinition.csdef-bestand voor een cloud-implementatie is geconfigureerd:

De snipet voor het .csdef-bestand dat is gegenereerd door een cloud-implementatie controleren, kunt u het externe eindpunt geconfigureerd voor gebruik van poorten HTTP op poort 10000, 10001 en 10002 zien.


    <ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
    <Endpoints>
        <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
        <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
        <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

        <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

        <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
           <AllocatePublicPortFrom>
              <FixedPortRange min=“10110” max=“10120“  />
           </AllocatePublicPortFrom>
        </InstanceInputEndpoint>
        <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
    </Endpoints>
    </WorkerRole>
    </ServiceDefinition>




## <a name="check-load-balancer-health-status-for-cloud-services"></a>Status van laden verdeling servicestatus voor cloudservices controleren


Hier volgt een voorbeeld van een test status:

        <LoadBalancerProbes>
        <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
        </LoadBalancerProbes>

De taakverdeling combineert de gegevens van het eindpunt en de gegevens van de test te maken van een URL in de vorm van http://{DIP van VM}:80/Probe.aspx die kunnen worden gebruikt om query's in de status van de service.

De service wordt gedetecteerd periodiek sondes van hetzelfde IP-adres. Dit is de status test aanvraag die afkomstig zijn uit de host van het knooppunt waarop de virtuele machine wordt uitgevoerd.
De service heeft om te reageren met een HTTP 200 statuscode voor de verdeling van de belasting naar wordt ervan uitgegaan dat de service correct is. Een andere HTTP-statuscode (bijvoorbeeld 503) rechtstreeks duurt de virtuele machine afmelden bij de draaiing.

De definitie test bepaalt u ook de frequentie van de test. De taakverdeling is het eindpunt elke 5 sec scannen in ons voorbeeld hierboven. Als er geen positief antwoord wordt ontvangen voor 10 sec (twee test intervallen), wordt uitgegaan van de test omlaag en draaiing, de virtuele machine wordt uitgeschakeld. Op dezelfde manier als de service afmelden bij de draaiing is en een positief antwoord wordt ontvangen, is de service zet terug draaiing direct af. Als de service is veranderen tussen orde en beschadigd, wordt de taakverdeling kunt beslissen de opnieuw introductie van de service terug naar de draaiing wachttijd voordat deze voor een aantal sondes in orde is.

Hiermee schakelt u het schema van de service definitie voor de [systeemstatus-test](https://msdn.microsoft.com/library/azure/jj151530.aspx) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een modus voor het laden gelijkmatige verdeling configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)

