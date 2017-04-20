<properties
   pageTitle="Typen van de knooppunten service stof en VM schaal Sets | Microsoft Azure"
   description="Wordt beschreven hoe de typen knooppunten Service stof zich verhouden tot VM schaal Sets en naar externe toegang krijgen tot een exemplaar VM schaal instellen of een clusterknooppunt."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>De relatie tussen de typen knooppunten Service stof en VM schaal sets weergegeven

VM schaal Sets zijn een resource Azure berekenen die u gebruiken kunt om te implementeren en beheren van een verzameling virtuele machines als een set. Elk knooppunttype die is gedefinieerd in een cluster Service stof is ingesteld als een afzonderlijke VM schaal instellen. Elk knooppunttype vervolgens kan worden vergroot omhoog of omlaag verschillende groepen poorten open belooft hebt en de doelstellingen van de verschillende capaciteit kan hebben.

De volgende schermafbeelding ziet u een cluster met twee typen van knooppunten: front-end en back-end.  Elk knooppunttype heeft vijf knooppunten.

![Schermafbeelding van een cluster met twee typen van knooppunten][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a>VM schaal instellen exemplaren toewijzen aan knooppunten

Zoals u boven ziet, start de exemplaren VM schaal instellen in exemplaar, 0 en klik vervolgens op Hiermee gaat u omhoog. De nummering wordt weergegeven in de namen. Bijvoorbeeld is BackEnd_0 exemplaar van 0 van de BackEnd VM schaal instellen. Deze bepaalde VM schaal instellen, heeft vijf exemplaren, met de naam BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 en BackEnd_4.

Wanneer u een VM schaal instellen schaal wordt een nieuw exemplaar wordt gemaakt. De nieuwe VM schaal instellen exemplaarnaam is meestal de naam van de VM schaal instellen + het exemplaarnummer van de volgende. In ons voorbeeld is het BackEnd_5.


## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a>VM schaal toewijzing instellen netwerktaakverdelers voor elk type knooppunt/VM schaal instellen

Als u uw cluster in de portal hebt geïmplementeerd of de voorbeeldsjabloon resourcemanager die wordt geleverd, klikt u vervolgens wanneer u een lijst met alle resources onder een resourcegroep hebt gebruikt ziet u de netwerktaakverdelers voor elk type VM schaal instellen of knooppunt.

De naam zou ongeveer zo: **kg -&lt;NodeType naam&gt;**. Bijvoorbeeld kg-sfcluster4doc-0, zoals in deze schermafbeelding:


![Resources][Resources]


## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a>Remote verbinding maken met een exemplaar VM schaal instellen of een clusterknooppunt
Elk type knooppunt die is gedefinieerd in een cluster is ingesteld als een afzonderlijke VM schaal instellen.  Dit betekent dat de knooppunttypen kunnen worden vergroot omhoog of omlaag onafhankelijk en kunnen worden gemaakt van verschillende VM SKU's. In tegenstelling tot één exemplaar VMs Verkrijg de exemplaren VM schaal instellen niet een virtueel IP-adres van hun eigen. Zodat kan deze lastig zijn enigszins wanneer u op zoek bent naar een IP-adres en poort die u kunt externe verbinding maken met een specifiek exemplaar.

Hier vindt u de stappen die u kunt volgen om te vinden.

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a>Stap 1: Vast te stellen de virtuele IP-adres van het knooppunttype en klik vervolgens op regels voor binnenkomende verbindingen NAT voor RDP

Om te krijgen die, moet u de binnenkomende NAT regels waarden die zijn gedefinieerd als onderdeel van de resource-definitie voor **Microsoft.Network/loadBalancers**.

Navigeer naar de belasting voor gelijkmatige verdeling blade en klik vervolgens op **Instellingen**in de portal.

![LBBlade][LBBlade]


Klik op **regels voor binnenkomende verbindingen NAT**- **Instellingen**. In dit nu kunt u het IP-adres en poort die u kunt externe verbinding maken met het eerste exemplaar van VM schaal instellen. In de onderstaande schermafbeelding is **104.42.106.156** en **3389**

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a>Stap 2: Zoeken vanuit de poort die u kunt gebruiken met externe verbinding maken met de instantie/knooppunt van de specifieke VM schaal instellen

Ik besproken hoe de exemplaren VM schaal instellen aan de knooppunten toewijzen eerder in dit document. We gebruiken die om te bepalen de exacte poort.

De poorten worden in oplopende volgorde van het exemplaar VM schaal instellen toegewezen. zodat in mijn voorbeeld voor het type van het knooppunt FrontEnd, zijn de poorten voor elk van de vijf exemplaren de volgende handelingen uit. u moet nu doen dezelfde toewijzing voor uw exemplaar VM schaal instellen.

|**VM schaal exemplaar instellen**|**Poort**|
|-----------------------|--------------------------|
|FrontEnd_0|3389|
|FrontEnd_1|3390|
|FrontEnd_2|3391|
|FrontEnd_3|3392|
|FrontEnd_4|3393|
|FrontEnd_5|3394|


### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a>Stap 3: Remote verbinding maken met de specifieke exemplaar van VM schaal instellen

Ik gebruik verbinding met extern bureaublad verbinding maken met de FrontEnd_1 in de onderstaande schermafbeelding:

![RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a>Het wijzigen van de waarden voor het RDP poort

### <a name="before-cluster-deployment"></a>Voordat u cluster-implementatie

Wanneer u het cluster met behulp van een sjabloon resourcemanager instelt, kunt u het bereik opgeven in het **inboundNatPools**.

Ga naar de definitie van de resource voor **Microsoft.Network/loadBalancers**. Klik onder die u de servicebeschrijving voor **inboundNatPools**vinden.  Vervang de waarden *frontendPortRangeStart* en *frontendPortRangeEnd* .

![InboundNatPools][InboundNatPools]


### <a name="after-cluster-deployment"></a>Na cluster-implementatie
Dit is minder snel en kan dit leiden tot de VMs ophalen gerecycled. U hebt nu nieuwe waarden via Azure PowerShell instellen. Zorg ervoor dat Azure PowerShell 1.0 of hoger op uw computer is geïnstalleerd. Als u dit nog niet hebt gedaan, ik het beste dat u de stappen in opvolgt [installeren en configureren van Azure PowerShell.](../powershell-install-configure.md)

Meld u aan bij uw Azure-account. Als deze PowerShell-opdracht om welke reden dan ook is mislukt, moet u controleren of er Azure PowerShell goed geïnstalleerd.

```
Login-AzureRmAccount
```

Voer de volgende u kunt gegevens over uw taakverdeling en u de waarden voor de beschrijving van **inboundNatPools**zien:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Stel nu *frontendPortRangeEnd* en *frontendPortRangeStart* naar de gewenste waarden.

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a>Volgende stappen

- [Overzicht van de functie "Implementeren elke locatie" en een vergelijking met clusters Azure worden beheerd](service-fabric-deploy-anywhere.md)
- [Cluster-beveiliging](service-fabric-cluster-security.md)
- [Service stof SDK en aan de slag](service-fabric-get-started.md)


<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
