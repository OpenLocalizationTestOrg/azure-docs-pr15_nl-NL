## <a name="how-to-create-a-vnet-using-a-network-config-file-from-powershell"></a>Het maken van een VNet met een netwerk config-bestand uit PowerShell

Azure gebruikt een XML-bestand om te bepalen van alle VNets beschikbaar op een abonnement. U kunt downloaden van dit bestand bewerken als u wilt wijzigen of verwijderen van bestaande VNets en nieuwe bestanden maken. In dit document, wordt u meer informatie over het downloaden van dit bestand, netwerk-configuratie (of netcgf) bestand, genoemd en bewerken als u wilt een nieuwe VNet maken. Hiermee schakelt u het [schema van Azure virtuele netwerk configuratie](https://msdn.microsoft.com/library/azure/jj157100.aspx) voor meer informatie over het configuratiebestand netwerk.

Als u wilt maken van een VNet met een netcfg-bestand via PowerShell, de onderstaande stappen uit te voeren.

1. Als u nooit Azure PowerShell gebruikt nog, raadpleegt u [het installeren en configureren van Azure PowerShell](../articles/powershell-install-configure.md) en volg de instructies helemaal naar het einde aan te melden bij Azure en selecteert u uw abonnement.
2. Vanuit de Azure PowerShell-console, gebruikt u de cmdlet **Get-AzureVnetConfig** downloaden van het configuratiebestand netwerk door de volgende opdracht uit te voeren. 

        Get-AzureVNetConfig -ExportToFile c:\NetworkConfig.xml

    Verwachte output:

        XMLConfiguration                                                                                                     
        ----------------                                                                                                     
        <?xml version="1.0" encoding="utf-8"?>...  

3. Open het bestand dat u hebt opgeslagen in stap 2 hierboven het gebruik van een XML- of tekst editor-toepassing en zoek de **<VirtualNetworkSites>** element. Als er geen netwerken al hebt gemaakt, elk netwerk wordt weergegeven als een eigen **<VirtualNetworkSite>** element.
4. Als wilt maken van het virtuele netwerk die worden beschreven in dit scenario, voegt u de volgende XML net onder de **<VirtualNetworkSites>** element:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

9.  Sla het bestand van de configuratie netwerk.
10. Van de Azure PowerShell-console, gebruikt u de cmdlet **Set-AzureVnetConfig** het bestand te uploaden netwerk configuratie door de volgende opdracht uit te voeren. Zoals u ziet de uitvoer onder de opdracht, ziet u **geslaagd** onder **OperationStatus**. Als dat wil zeggen niet het geval is, schakelt u het XML-bestand op fouten.

        Set-AzureVNetConfig -ConfigurationPath c:\NetworkConfig.xml

    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

        OperationDescription OperationId                          OperationStatus
        -------------------- -----------                          ---------------
        Set-AzureVNetConfig  49579cb9-3f49-07c3-ada2-7abd0e28c4e4 Succeeded 
    
11. Vanuit de Azure PowerShell-console, gebruikt u de cmdlet **Get-AzureVnetSite** om te bevestigen dat het nieuwe netwerk is toegevoegd door de volgende opdracht uit te voeren. 

        Get-AzureVNetSite -VNetName TestVNet

    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

        AddressSpacePrefixes : {192.168.0.0/16}
        Location             : Central US
        AffinityGroup        : 
        DnsServers           : {}
        GatewayProfile       : 
        GatewaySites         : 
        Id                   : b953f47b-fad9-4075-8cfe-73ff9c98278f
        InUse                : False
        Label                : 
        Name                 : TestVNet
        State                : Created
        Subnets              : {FrontEnd, BackEnd}
        OperationDescription : Get-AzureVNetSite
        OperationId          : 3f35d533-1f38-09c0-b286-3d07cd0904d8
        OperationStatus      : Succeeded