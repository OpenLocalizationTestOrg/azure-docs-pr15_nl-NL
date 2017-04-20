<properties
   pageTitle="Azure architectuur referentie - IaaS: Maken van een Active Directory resource bos in Azure | Microsoft Azure"
   description="Het maken van een vertrouwd Active Directory-domein in Azure wordt aangegeven."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="creating-a-active-directory-directory-services-adds-resource-forest-in-azure"></a>Maken van een Active Directory Directory Services (Hiermee) resource bos in Azure wordt aangegeven

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Dit artikel wordt beschreven hoe u een Active Directory-domein maakt in Azure wordt aangegeven dat los van, maar vertrouwde door domeinen in uw on-premises implementatie bos.

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: [Resourcemanager] [ resource-manager-overview] en klassieke. Deze verwijzing architectuur gebruikt resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties.

Een organisatie die wordt uitgevoerd AD (Active Directory) on-premises implementatie wellicht een bos die bestaat uit veel verschillende domeinen. Zoals u afzonderlijke domeinen voor verschillende afdelingen of suborganizations mogelijk maken of nieuwe domeinen mogelijk zijn toegevoegd als gevolg van aanschaf of fusie van andere organisaties. U kunt domeinen moeten worden geïsoleerd tussen functiegebieden die moet worden geïsoleerd, mogelijk vanwege de beveiliging bieden, maar u kunt gegevens delen tussen domeinen door in te stellen vertrouwensrelaties.

Een organisatie die gebruikmaakt van afzonderlijke domeinen kunt profiteren van Azure door het verplaatsen van een of meer van deze domeinen in de cloud. U kunt ook een organisatie mogelijk wilt bewaren alle cloud resources logisch afzonderlijk vanuit deze aangehouden on-premises en sla de gegevens van cloud resources in hun eigen adreslijst in een domein ook gehouden in de cloud.

U kunt Active Directory implementeren in Azure op verschillende manieren, zoals wordt beschreven in de artikelen [Active Directory uitbreiden naar Azure] [ extending-ad-to-azure] en [Implementeren van Azure Active Directory][implementing-aad]. In dit document is gericht op een specifieke scenario: maken van een domein in de cloud die verschilt van alle domeinen gehouden on-premises implementatie, maar dat kan een vertrouwensrelatie met on-premises implementatie domeinen hebben. 

Veelvoorkomend gebruik dozen voor deze architectuur opnemen:

- Onderhouden beveiliging scheiding voor objecten en identiteiten gehouden in de cloud.

- Afzonderlijke domeinen migreren vanuit on-premises naar de cloud.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram wordt de belangrijke onderdelen in deze architectuur (*Klik op om in te zoomen*) gemarkeerd. Lees voor meer informatie over de onderdelen die niet beschikbaar bij [uitvoering van een beveiligde hybride netwerkarchitectuur in Azure wordt aangegeven] [ implementing-a-secure-hybrid-network-architecture] en [implementeren van een beveiligde hybride netwerkarchitectuur met internettoegang in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **On-premises netwerk.** De on-premises netwerk bevat een eigen AD bos en domeinen.

- **AD-Servers.** Hierna ziet u domeincontrollers implementatie van directoryservices (AD DS) wordt uitgevoerd met VMs in de cloud. Deze servers hosten een bos met een of meer afzonderlijke vanuit deze bevindt on-premises domeinen.

- **Eenzijdige vertrouwensrelatie.** Het voorbeeld in het diagram ziet een eenzijdige vertrouwensrelatie van het domein in de cloud naar de on-premises domein. Deze relatie kunt on-premises gebruikers te krijgen tot bronnen in het domein in de cloud, maar niet andersom. Dit is een algemene hoofdletters/kleine letters. U kunt echter een tweerichtingsvertrouwensrelatie maken als cloud gebruikers ook toegang tot on-premises implementatie resources.

- **Active Directory-subnet.** De AD DS-servers worden gehost in een afzonderlijk subnet. Regels voor NSG de AD DS-servers beveiligen en een firewall tegen verkeer uit onverwachte bronnen kunnen bevatten.

- **Web laag subnet**, **bedrijven laag subnet**en **gegevens trapsgewijs subnet**. Deze subnetten hosten de servers en de onderdelen die toepassingen in de cloud uitvoeren. Zie voor meer informatie [VMs uitgevoerd voor een architectuur N lagen op Azure][running-VMs-for-an-N-tier-architecture-on-Azure]. De resources en VMs in dit subnet bevinden zich in de cloud-domein.

- **Azure Gateway**. De gateway Azure biedt een verbinding tussen de on-premises netwerk en het VNet Azure. Dit is een [VPN-verbinding] [ azure-vpn-gateway] of [Azure ExpressRoute][azure-expressroute]. Zie voor meer informatie, [uitvoering van een beveiligde hybride netwerkarchitectuur in Azure wordt aangegeven][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Aanbevelingen

Deze sectie bevat een lijst met aanbevelingen op basis van de essentiële onderdelen die nodig zijn voor het implementeren van de eenvoudige architectuur. Deze aanbevelingen omvatten:

- Hiermee instellingen, en

- Configuratie van de relatie vertrouwen.

Er mogelijk aanvullende of verschillende vereisten uit die hier worden beschreven. U kunt de items in deze sectie gebruiken als uitgangspunt voor het aanpassen van de architectuur voor uw eigen systeem bezig.

### <a name="adds-recommendations"></a>Hiermee aanbevelingen

Voor specifieke aanbevelingen over het implementeren van Active Directory in de cloud, raadpleegt u het document [Active Directory uitbreiden naar Azure][extending-ad-to-azure]. Het artikel [richtlijnen voor het implementeren van Windows Server Active Directory op Azure virtuele Machines] [ ad-azure-guidelines] extra gedetailleerde gegevens bevat.

### <a name="trust-recommendations"></a>Aanbevelingen vertrouwen

De domeinen van de on-premises implementatie bevinden zich in een ander domein uit de domeinen in de cloud. Als u wilt de verificatie van on-premises gebruikers in de cloud inschakelt, moeten de domeinen in de cloud het aanmeldingsdomein in de on-premises implementatie bos vertrouwen. Op dezelfde manier als de cloud een aanmeldingsdomein voor externe gebruikers biedt, het mogelijk nodig voor de on-premises implementatie bos te vertrouwen van het domein van de cloud.

U kunt vertrouwensrelaties op het niveau van bos definiëren door te [maken van bos vertrouwensrelaties][creating-forest-trusts], of op het domeinniveau van het door te [maken van externe vertrouwensrelaties][creating-external-trusts]. Een niveau vertrouwen bos Hiermee maakt u een relatie tussen alle domeinen in twee forests. Een externe niveau voor het domeinvertrouwen alleen Hiermee maakt u een relatie tussen twee opgegeven domeinen. U moet alleen externe domeinnaam niveau vertrouwensrelaties tussen domeinen in verschillende forests maken.

Vertrouwensrelaties kunnen zijn één richting (één richting) of bidirectionele (twee richtingen):

- Een eenzijdige vertrouwensrelatie kan gebruikers in een domein of bos (bekend als de *binnenkomende* domein of bos) voor toegang tot de bronnen die in een andere (de *uitgaande* domein of bos). 

- Een tweerichtingsvertrouwensrelatie kan gebruikers in een domein- of bos gehouden in de andere informatiebronnen.

De volgende tabel worden vertrouwen configuraties voor enkele eenvoudige scenario's:

| Scenario | On-premises implementatie vertrouwen | Cloud vertrouwen |
|----------|-------------------|-------------|
| On-premises implementatie gebruikers moeten toegang hebben tot bronnen in de cloud, maar niet andersom | Eén, binnenkomende | Eén, uitgaande |
| Gebruikers in de cloud moeten toegang hebben tot lokale bronnen, maar niet andersom | Eén, uitgaande | Eén, binnenkomende |
| Gebruikers in de cloud en on-premises is vereist voor toegang tot bronnen in de cloud en on-premises gehouden | Twee richtingen, inkomende en uitgaande | Twee richtingen, inkomende en uitgaande |

## <a name="security-considerations"></a>Veiligheidsoverwegingen

Bos niveau vertrouwensrelaties zijn overgankelijke. Als u een niveau bos-vertrouwensrelatie tussen een bos on-premises implementatie en een bos in de cloud hebt ingesteld, wordt deze vertrouwen uitgebreid naar andere nieuwe domeinen die is gemaakt in een van beide bos. Als u domeinen gebruiken om de scheiding om veiligheidsredenen, kunt u vertrouwensrelaties maken op het domeinniveau van alleen. Domein niveau vertrouwensrelaties zijn niet-transitieve.

Zie het gedeelte *beveiligingsoverwegingen* in [Active Directory uitbreiden naar Azure]voor AD-specifieke beveiligingsoverwegingen,[extending-ad-to-azure].

## <a name="availability-considerations"></a>Aandachtspunten voor de beschikbaarheid

Implementeer ten minste twee domeincontrollers voor elk domein. Hiermee worden automatische replicatie tussen servers. Maak een beschikbaarheid instellen voor de VMs die fungeert als elk domein de verwerking van AD-servers. Zorg ervoor dat er ten minste twee servers in de set. 

U kunt wellicht ook een of meer servers in elk domein aanwijzen als [stand-by operations outmodellen] [ standby-operations-masters] geval connectiviteit met een server die fungeert als een FSMO-functie is mislukt.

## <a name="scalability-considerations"></a>Aandachtspunten voor de schaalbaarheid

Er kan AD worden automatisch aangepast voor domeincontrollers die deel van hetzelfde domein uitmaken. Aanvragen zijn verdeeld over elke afzonderlijk binnen een domein. U kunt een andere domeincontroller toevoegen en worden automatisch gesynchroniseerd met het domein. Een afzonderlijk taakverdeling om verkeer controller binnen het domein niet configureert. Zorgt ervoor dat alle domeincontrollers onvoldoende geheugen en opslagruimte bronnen om u te verwerken van de domeindatabase. Alle domeincontroller VMs even groot maken.

## <a name="management-and-monitoring-considerations"></a>Beheer en bewaking overwegingen

Zie voor informatie over het beheer en bewaking overwegingen, de overeenkomstige secties in [Active Directory uitbreiden naar Azure][extending-ad-to-azure]. 

Zie voor meer informatie, [Monitoring Active Directory][monitoring_ad]. U kunt installeren hulpprogramma's zoals [Microsoft Systems Center] [ microsoft_systems_center] op een monitoring server in het subnet management om te helpen deze taken uitvoeren. 

## <a name="solution-components"></a>Onderdelen van oplossingen

Een oplossing voorbeeldscript, [Deploy-ReferenceArchitecture.ps1][solution-script], is beschikbaar die u gebruiken kunt voor de uitvoering van de architectuur die volgt op de aanbevelingen in dit artikel beschreven. Dit script maakt gebruik van Azure resourcemanager sjablonen. De sjablonen zijn beschikbaar als een set fundamentele bouwstenen, die elk een specifieke actie zoals een VNet maken of te configureren van een NSG uitvoeren. Het doel van het script is goedkeuringen sjabloonimplementatie.

De sjablonen zijn van parameters voorzien, met de parameters gehouden in afzonderlijke JSON-bestanden. De parameters in deze bestanden configureren voor de implementatie aan uw eigen vereisten voldoet, kunt u wijzigen. U hoeft niet te wijzigen van de sjablonen zelf. U moet de schema's van de objecten in de parameterbestanden niet wijzigen.

Wanneer u de sjablonen hebt bewerkt, maakt u objecten die de naamconventies die worden beschreven in [Naamgevingsconventies voor aanbevolen voor Azure Resources][naming-conventions].

De voorbeeld-oplossing wordt gemaakt en een omgeving in de cloud waarmee een domein met de naam *treyresearch.com*configureert. Deze omgeving omvat de Hiermee subnet en servers, DMZ weblaag, zakelijke laag en laag voor gegevenstoegang, VPN gateway en management laag. De oplossing voorbeeld bevat ook een optionele configuratie voor het maken van een gesimuleerd on-premises omgeving met een eigen domein, *contoso.com*. De oplossing bevat scripts die een vertrouwensrelatie tot stand tussen deze domeinen waarmee on-premises gebruikers brengen te krijgen tot objecten in het domein *treyresearch.com* in de cloud.

De volgende secties worden de elementen van de on-premises implementatie en configuraties cloud.

### <a name="on-premises-components"></a>Onderdelen van de on-premises implementatie

>[AZURE.NOTE] Deze onderdelen zijn niet de nadruk van de architectuur die worden beschreven in dit document en overzichtelijk wilt geeft u de gelegenheid om te testen van de cloud-omgeving veilig, in plaats van met een reële productieomgeving worden gegeven. Daarom in deze sectie alleen bevat een overzicht van de belangrijkste parameterbestanden. U kunt instellingen zoals de IP-adressen of de grootte van de VMs wijzigen, maar het is raadzaam te laat veel van de andere parameters ongewijzigd.

Deze omgeving bestaat uit een AD-bos het domein *contoso.com* . Het domein bevat twee Hiermee servers met IP-adressen 192.168.0.4 en 192.168.0.5. Deze twee servers worden ook de DNS-service uitvoeren. Het lokale beheerdersaccount op beide VMs heet `testuser` met wachtwoord `AweS0me@PW`. Daarnaast kunnen bepaalt de configuratie een VPN-gateway voor de verbinding met de VNet in de cloud. U kunt de configuratie wijzigen door het bewerken van de volgende JSON-bestanden die zich bevindt in de [**parameters/lokale**] [ on-premises-folder] map:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Dit bestand bepaalt de ruimte voor het adres van netwerk voor de on-premises omgeving.

- ** [virtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Dit bestand bevat de configuratie voor de on-premises implementatie VMs Hiermee hostingservices. Standaard worden twee *standaard-DS3-v2* VMs gemaakt.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** en ** [connection.parameters.json][on-premises-connection-parameters]**. Deze bestanden houdt u de instellingen voor de VPN-verbinding met de gateway Azure VPN in de cloud, met inbegrip van de gedeelde sleutel moet worden gebruikt om te beveiligen verkeer doorlopen van de gateway.

De resterende bestanden in de map bevatten de configuratiegegevens gebruikt om te maken van de on-premises domein (*contoso.com*) met deze infrastructuur. U deze gebruiken voor het installeren van Hiermee, setup DNS, en de on-premises implementatie bos maken.

De oplossing wordt ook gebruikt voor het volgende script, met de naam [inkomende-trust.ps1][incoming-trust], die wordt uitgevoerd op een computer in de on-premises domein:

```Powershell
# Run the following powershell script in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)

$TrustedDomainName = "contoso.com"
#$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

$TrustingDomainName = "treyresearch.com"
$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustingDomainDnsIpAddresses
$ForwardDomainName = $TrustingDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Dit script voegt het IP-adressen van de AD DS-servers in de cloud (Zie de volgende sectie) de lokale DNS-service, en gebruikt u vervolgens de [netdom] [ netdom] opdracht om te maken van een binnenkomende eenzijdige vertrouwensrelatie van het domein in de cloud (*treyresearch.com*).

### <a name="cloud-components"></a>Onderdelen van de cloud

Deze onderdelen vormen een belangrijk onderdeel van deze architectuur. Ze de infrastructuur voor het domein *treyresearch.com* instellen en de vertrouwensrelaties maken met het domein van de on-premises implementatie *contoso.com* . De [**parameters/azure**] [ azure-folder] map bevat de volgende parameterbestanden voor het configureren van de volgende onderdelen:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Structuur van de VNet door dit bestand wordt gedefinieerd voor de VMs en andere elementen in de cloud. Het bevat instellingen, zoals de naam, -adresruimte, subnetten en de adressen van de DNS-servers vereist. De DNS-adressen uit deze verwijzing voorbeeld de IP-adressen van de lokale DNS-servers en ook de standaard Azure DNS-server. Deze adressen om te verwijzen naar uw eigen DNS-instellingen als u geen gebruik van de steekproef on-premises omgeving wijzigen:
    
    ```json
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg",
        "addressPrefixes": [
          "10.0.0.0/16"
        ],
        "subnets": [
          {
            "name": "dmz-private-in",
            "addressPrefix": "10.0.0.0/27"
          },
          {
            "name": "dmz-private-out",
            "addressPrefix": "10.0.0.32/27"
          },
          {
            "name": "dmz-public-in",
            "addressPrefix": "10.0.0.64/27"
          },
          {
            "name": "dmz-public-out",
            "addressPrefix": "10.0.0.96/27"
          },
          {
            "name": "mgmt",
            "addressPrefix": "10.0.0.128/25"
          },
          {
            "name": "GatewaySubnet",
            "addressPrefix": "10.0.255.224/27"
          },
          {
            "name": "web",
            "addressPrefix": "10.0.1.0/24"
          },
          {
            "name": "biz",
            "addressPrefix": "10.0.2.0/24"
          },
          {
            "name": "data",
            "addressPrefix": "10.0.3.0/24"
          },
          {
            "name": "adds",
            "addressPrefix": "10.0.4.0/27"
          }
        ],
        "dnsServers": [
          "10.0.4.4",
          "10.0.4.5",
          "168.63.129.16"
        ]
      }
    }
    ```

- ** [virtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **. Dit bestand configureert het VMs Hiermee uitgevoerd in de cloud. De configuratie bestaat uit twee VMs. Wijzig de admin-gebruikersnaam en wachtwoord in het `virtualMachineSettings` sectie, en u kunt desgewenst het formaat VM zodat deze overeenkomen met de vereisten van het domein wijzigen:

    Zie voor meer informatie, [Active Directory uitbreiden naar Azure][extending-ad-to-azure].
    
    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-ad",
        "computerNamePrefix": "aad",
        "size": "Standard_DS3_v2",
        "osType": "Windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "adds",
            "privateIPAllocationMethod": "Static",
            "startingIPAddress": "10.0.4.4",
            "enableIPForwarding": false,
            "dnsServers": [
            ],
            "isPrimary": "true"
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 127,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [
        ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-as"
        }
      }
    },
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg"
      }
    },
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ```

- ** [toevoegen-telt-domein-controller.parameters.json][add-adds-domain-controller-parameters]**. Dit bestand bevat de instellingen voor het maken van het domein dat *treyresearch.com* die de servers Hiermee in beslag nemen. Aangepaste uitbreidingen die stand brengen van het domein en de servers Hiermee toevoegen aan deze wordt gebruikt. Tenzij u aanvullende Hiermee-servers maken (in dat geval moet u deze toevoegen aan de `vms` matrix), hun namen wijzigen van de standaard of wilt maken van een domein met een andere naam die u niet nodig hebt om het bestand.

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Dit bestand bevat de instellingen voor het maken van de gateway Azure VPN in de cloud gebruikt om te verbinden met de on-premises netwerk. U mag wijzigen de `sharedKey` waarde in de `connectionsSettings` sectie zodat deze overeenkomt met die van het apparaat van de VPN on-premises implementatie. Zie [een hybride netwerkarchitectuur met Azure en On-premises VPN implementeren]voor meer informatie,[hybrid-azure-on-prem-vpn].

- ** [dmz-private.parameters.json] [ dmz-private-parameters] ** en ** [dmz-public.parameters.json ] [ dmz-public-parameters] **. Deze bestanden configureren de binnenkomende (openbaar) en uitgaande (privé) zijden van de VMs waaruit de DMZ, beveiligen van de servers in de cloud. Zie voor meer informatie over deze elementen en hun configuratie, [uitvoering van een DMZ tussen Azure en Internet][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer web.parameters.json][loadBalancer-web-parameters]**, ** [loadBalancer biz.parameters.json][loadBalancer-biz-parameters]**, en ** [loadBalancer data.parameters.json][loadBalancer-data-parameters]**. Deze bestanden parameters bevatten de VM specificaties voor het web en business data access-lagen en configureren van netwerktaakverdelers voor elke laag. Hierna ziet u de VMs die host de WebApps en -databases en de werkbelasting bedrijven uitvoeren voor de organisatie. De VMs in de weblaag worden toegevoegd aan het domein in de cloud met behulp van de instellingen in de ** [web-vm-domein-join.parameters.json] [ web-vm-domain-join-parameters] ** bestand.

    Elk bestand bevat twee sets met configuratieparameters die. De `virtualMachineSettings` sectie definieert de VMs die als de ADFS-service in de cloud host. Standaard het script Hiermee maakt u twee of meer van de volgende VMs in bepaalde beschikbaarheid. De volgende beide weergeven de relevante onderdelen van het bestand *loadBalancer-web.parameters.json* :

    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-web",
        "computerNamePrefix": "web",
        "size": "Standard_DS1_v2",
        "osType": "windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "web",
            "privateIPAllocationMethod": "Dynamic",
            "isPrimary": "true",
            "enableIPForwarding": false,
            "dnsServers": [ ]
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 128,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [ ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-web-vm-as"
        }
      }
    },
    ...
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ````
    U kunt de grootte en het aantal VMs in elke laag wijzigen op basis van uw vereisten.

    De `loadBalancerSettings` gedeelte bevat de beschrijving van de verdeling van de belasting voor deze VMs. De taakverdeling geeft verkeer dat wordt weergegeven op poort 80 (HTTP) en poort 443 (HTTPS) aan een of andere van de VMs. 

    >[AZURE.NOTE] De regel voor poort 80 gebruikt een TCP-verbinding in plaats van HTTP. Dit komt omdat de installatie van IIS op de weblaag is geconfigureerd voor Windows-verificatie alleen ondersteuning. Anonieme verificatie is uitgeschakeld. U probeert te *ping* mislukt poort 80 via een HTTP-verbinding met een 401 (onbevoegde) fout, dat alleen met behulp van een TCP-verbinding wordt gedetecteerd of de poort actief is:

    ```json
    "loadBalancerSettings": {
      "value": {
        "name": "ra-adtrust-web-lb",
        "frontendIPConfigurations": [
          {
            "name": "ra-adtrust-web-lb-fe",
            "loadBalancerType": "internal",
            "internalLoadBalancerSettings": {
              "privateIPAddress": "10.0.1.254",
              "subnetName": "web"
            }
          }
        ],
        "backendPools": [
          {
            "name": "ra-adtrust-web-lb-bep",
            "nicIndex": 0
          }
        ],
        "loadBalancingRules": [
          {
            "name": "http-rule",
            "frontendPort": 80,
            "backendPort": 80,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "http-probe",
            "enableFloatingIP": false
          },
          {
            "name": "https-rule",
            "frontendPort": 443,
            "backendPort": 443,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "https-probe",
            "enableFloatingIP": false
          }
        ],
        "probes": [
          {
            "name": "http-probe",
            "port": 80,
            "protocol": "Tcp",
            "requestPath": null
          },
          {
            "name": "https-probe",
            "port": 443,
            "protocol": "Tcp",
            "requestPath": null
          }
        ],
        "inboundNatRules": [ ]
      }
    }
    ```

- ** [virtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Dit bestand bevat de configuratie voor de lijnsprong vak/management VMs. Alleen is het mogelijk om te krijgen van aanmeldings- en beheerderstoegang tot de VMs in het web, business en niveaus van de gegevens in het vak sprong. Standaard het script Hiermee maakt u een enkel *Standard_DS1_v2* VM, maar u kunt dit bestand als u wilt maken van groter te maken of aanvullende VMs als de werklast management is waarschijnlijk aanzienlijk aanpassen.

De configuratie wordt ook gebruikt voor de [uitgaande trust.ps1] [ outgoing-trust] script om te maken van een eenzijdige uitgaande vertrouwensrelatie met het domein *contoso.com* hieronder wordt weergegeven:

```powershell
# prerequiste: 
# You need to first run incoming-trust.ps1 in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)
# Then,
# Run the following powershell script in ra-adtrust-ad-vm1 (ip 10.0.4.4)

$TrustedDomainName = "contoso.com"
$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

#$TrustingDomainName = "treyresearch.com"
#$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustedDomainDnsIpAddresses
$ForwardDomainName = $TrustedDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

#netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Dit script is vergelijkbaar met het *inkomende-trust.ps1* script hierboven. Het IP-adressen van de on-premises implementatie worden toegevoegd AD DS-servers op de lokale DNS-service, en gebruikt u vervolgens de [netdom] [ netdom] opdracht om de uitgaande vertrouwensrelatie maken.

## <a name="solution-deployment"></a>Implementatie van oplossingen

De oplossing wordt ervan uitgegaan van de volgende vereisten:

- U hebt een bestaand Azure-abonnement waarin u resourcegroepen kunt maken.

- U hebt gedownload en geïnstalleerd van de meest recente versie van Azure Powershell. Zie [hier] [ azure-powershell-download] voor instructies.

Het script uitvoeren dat de oplossing implementeert:

1. Verplaatsen naar een handige map op uw lokale computer en de volgende submappen maken:

    - Scripts

    - Scripts/Parameters

    - Scripts/Parameters/lokale

    - Scripts/Parameters/Azure

2. Download de [Deploy-ReferenceArchitecture.ps1] [ solution-script] bestand naar de map

3. De inhoud van de [parameters/lokale] downloaden[ on-premises-folder] map naar de map Scripts/Parameters/lokale:

4. De inhoud van de [parameters/azure] downloaden[ azure-folder] map naar de map Scripts/Parameters/Azure.

5. Bewerk het bestand Deploy-ReferenceArchitecture.ps1 in de map Scripts, waarna de volgende regels als u wilt aangeven welke resourcegroepen dat moeten worden gemaakt of voor het opslaan van de resources die zijn gemaakt door het script wijzigen:

    ```powershell
    # Azure Onpremise Deployments
    $onpremiseNetworkResourceGroupName = "ra-adtrust-onpremise-rg"

    # Azure ADDS Deployments
    $azureNetworkResourceGroupName = "ra-adtrust-network-rg"
    $workloadResourceGroupName = "ra-adtrust-workload-rg"
    $securityResourceGroupName = "ra-adtrust-security-rg"
    $addsResourceGroupName = "ra-adtrust-adds-rg"
    ```

6. Bewerk de parameterbestanden in de Scripts/Parameters/lokale en Scripts/Parameters/Azure-mappen. De resource groep verwijzingen in deze bestanden zodat deze overeenkomt met de namen van de resourcegroepen die zijn toegewezen aan de variabelen in het bestand Deploy-ReferenceArchitecture.ps1 bijwerken. De volgende tabel ziet u welke parameterbestanden verwijzen naar welke resourcegroep. De resourcegroepen *AB-adfs-werkbelasting-rg*, *AB-adfs-beveiliging-rg* *AB-adfs-telt-rg*, *AB-adfs-adfs-rg*en *AB-AD FS-proxy-rg* worden alleen gebruikt in de PowerShell-script en niet worden weergegeven in de parameterbestanden.

  	|Resourcegroep|Parameterbestanden|
  	|--------------|--------------|
  	|AB-adtrust-lokale-rg|parameters\onpremise\connection.parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.parameters.JSON<br />parameters\onpremise\virtualNetwork.parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON
  	|AB-adtrust-netwerk-rg|parameters\onpremise\connection.parameters.JSON<br />parameters\azure\dmz-Private.parameters.JSON<br />parameters\azure\dmz-Public.parameters.JSON<br />parameters\azure\loadBalancer-Biz.parameters.JSON<br />parameters\azure\loadBalancer-Data.parameters.JSON<br />parameters\azure\loadBalancer-web.parameters.JSON<br />parameters\azure\virtualMachines-ADDS.parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.parameters.JSON<br />parameters\azure\virtualNetwork-ADDS-DNS.parameters.JSON<br />parameters\azure\virtualNetwork.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON (*twee exemplaren*)

    Daarnaast kunnen instellen van de configuratie van de on-premises en cloud onderdelen, zoals wordt beschreven in de [Oplossing Components] [ solution-components] sectie.

7. Open een Azure PowerShell-venster, verplaatsen naar de map Scripts en voer de volgende opdracht:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Vervang `<subscription id>` met uw Azure abonnement-ID.

    Voor `<location>`, typt u een Azure gebied, bijvoorbeeld `eastus` of `westus`.

    De `<mode>` parameter kunt beschikken over een van de volgende waarden:

    - `Onpremise`, de gesimuleerd on-premises omgeving te maken.

    - `Infrastructure`, om te maken van de VNet-infrastructuur en springen van vak in de cloud.

    - `CreateVpn`, voor het bouwen van Azure virtuele netwerkgateway en verbindt u deze naar de on-premises netwerk.

    - `AzureADDS`, als u wilt samenstellen de VMs die fungeert als Hiermee servers, Active Directory implementeren naar deze VMs en maken van het domein in de cloud.

    - `WebTier`, die wordt gemaakt met het web VMs trapsgewijs en de belasting voor documentconversies.

    - `Prepare`, waarin alle voorafgaande taken kunt uitvoeren. **Dit is de aanbevolen optie als u een volledig nieuwe implementatie samenstelt en u een bestaande on-premises implementatie-infrastructuur geen hebt.**

    - `Workload`de laag zakelijke en gegevens VMs maken en netwerktaakverdelers. Deze VMs zijn geen deel uit van de `Prepare` optie.

    >[AZURE.NOTE] Als u de `Prepare` optie, het script wordt enkele uren om te voltooien.

8.  Als u de configuratie van de on-premises implementatie voorbeeld gebruikt:

    1. Verbinding maken met het vak sprong (*AB-adtrust-mgmt-vm1* in de resourcegroep *AB-adtrust-beveiliging-rg* ). Meld u aan als *testuser* met wachtwoord *AweS0me@PW*.

    2.  Open op het vak sprong een RDP-sessie op de eerste VM in het domein *contoso.com* (het domein van de on-premises implementatie). Deze VM heeft het IP-adres 192.168.0.4. De gebruikersnaam is *contoso\testuser* met wachtwoord *AweS0me@PW*.

    3. Download de [inkomende-trust.ps1] [ incoming-trust] script en uitvoeren om u te maken van de binnenkomende vertrouwensrelatie van het domein *treyresearch.com* .

9. Als u de infrastructuur van uw eigen on-premises implementatie gebruikt:

    1. Download de [inkomende-trust.ps1] [ incoming-trust] script.

    2. Bewerk het script en vervang de waarde van de `$TrustedDomainName` variabele met de naam van uw eigen domein.

    3. Het script uitvoeren.

10. Klik in de sprong-verbinding maken met de eerste VM in het domein *treyresearch.com* (het domein in de cloud). Deze VM heeft het IP-adres 10.0.4.4. De gebruikersnaam is *treyresearch\testuser* met wachtwoord *AweS0me@PW*.

11. Download de [uitgaande trust.ps1] [ outgoing-trust] script en uitvoeren om u te maken van de binnenkomende vertrouwensrelatie van het domein *treyresearch.com* . Als u uw eigen machines on-premises implementatie gebruikt, klikt u vervolgens het script eerst bewerkt. Stel de `$TrustedDomainName` variabele op de naam van uw domein on-premises implementatie, en geef het IP-adressen van de AD DS-servers voor dit domein in de `$TrustedDomainDnsIpAddresses` variabele.

12. Op een lokale computer, voert u de stappen in het artikel [een vertrouwensrelatie controleren] [ verify-a-trust] om te bepalen of de vertrouwensrelatie correct is geconfigureerd tussen de domeinen van de *contoso.com* en *treyresearch.com* . Mogelijk moet u wachten op een paar minuten na het voltooien van de vorige stappen voordat de vertrouwen kan worden gevalideerd.

De overige optionele stappen hoe om te bepalen of het voor het domeinvertrouwen werkt zoals verwacht. Deze stappen vereisen dat u toegang tot een computer development met Visual Studio is geïnstalleerd hebt.

1.  Klik in het venster Azure PowerShell Voer de volgende opdracht de weblaag maken als u geen hebt ingesteld deze eerder (met behulp van de `Prepare` optie):

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> WebTier
    ```

    Deze opdracht maakt de weblaag en wordt de VMs toegevoegd aan het domein *treyresearch.com* .

2. Met Visual Studio op de computer van de ontwikkeling, maakt u een ASP.NET-webtoepassing met de naam *TreyResearchWebApp*. .NET Framework 4.5.2 gebruiken.

3. Selecteer de sjabloon *MVC* en wijzig de verificatie in *Windows-verificatie*. Een App-Service niet worden maken in de cloud.

3. Maken en uitvoeren van de toepassing test de verificatie. Controleer of de gebruikersnaam van uw huidige Windows wordt weergegeven in de menubalk boven aan de pagina naar rechts.

4. Sluit Internet Explorer.

5. Klik in het *Oplossingverkenner* -venster met de rechtermuisknop op het project TreyResearchWebApp, klik op *publiceren*.

6. Klik in het venster *Web publiceren* op *aangepast*. Een aangepast profiel benoemde *TreyResearchWebApp*maken.

7. Klik op de pagina *verbinding* de *publiceermethode* instellen op *Bestandssysteem* en geef een map genaamd *TreyResearchWebApp*, zich bevindt in een handige locatie op de computer.

8. Stel de *configuratie* naar *versie*op de pagina *-Instellingen* .

9. Klik op *publiceren*op de pagina *Preview* .

10. Verbinding maken met elk VM in de weblaag beurtelings (via het vak sprong) en de volgende taken uitvoeren. De IP-adressen van het web laag VMs zijn 10.0.1.4 en 10.0.1.5. De gebruikersnaam voor beide VMs is *treyresearch\testuser* met wachtwoord *AweS0me@PW*:

    1. Kopieer de map *TreyResearchWebApp* en de inhoud van de computer van de ontwikkeling naar de map *C:\inetpub* .

    2. De beheerconsole van Internet Information Services (IIS) gebruikt, navigeer naar *de website Sites\Default* op de computer.

    3. Klik in het deelvenster *Acties* op *Basisinstellingen*en het fysieke pad van de website wijzigen in *%SystemDrive%\inetpub\TreyResearchWebApp*.

    4. Dubbelklik in het deelvenster *Functies voor weergave* op *verificatie*. Controleer of *Windows-verificatie* is ingeschakeld en *Anonieme verificatie* is uitgeschakeld.

11. open Internet Explorer vanaf een lokale computer, en blader naar de website op http://10.0.1.254 (dit is het adres van de verdeling van de web laag belasting).

12. Voer de referenties van een gebruiker in de on-premises domein in het dialoogvenster *Windows-beveiliging* . Geef een gebruikersnaam die niet in het domein *treyresearch bestaat* . Als u de gesimuleerd on-premises omgeving gebruikt voor het maken van een gebruiker in het domein *contoso* is het eerste en geef de referenties van deze gebruiker.

13. Wanneer de startpagina van Lotus wordt weergegeven, controleert u of het juiste domein en de gebruikersnaam die worden weergegeven op de menubalk boven aan de pagina naar rechts.

## <a name="next-steps"></a>Volgende stappen

- Informatie over de aanbevolen procedures voor het [uitbreiden van uw domein on-premises implementatie Hiermee Azure][adds-extend-domain]

- Informatie over de aanbevolen procedures voor het [maken van een ADFS-infrastructuur] [ adfs] in Azure wordt aangegeven.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[implementing-aad]: ./guidance-identity-aad.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[running-VMs-for-an-N-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[creating-forest-trusts]: https://technet.microsoft.com/library/cc816810(v=ws.10).aspx
[creating-external-trusts]: https://technet.microsoft.com/library/cc816837(v=ws.10).aspx
[naming-conventions]: ./guidance-naming-conventions.md
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[verify-a-trust]: https://technet.microsoft.com/library/cc753821.aspx
[netdom]: https://technet.microsoft.com/library/cc835085.aspx
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://github.com/mspnp/reference-architectures/blob/master/guidance-identity-adds-trust/parameters/onpremise/connection.parameters.json
[incoming-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/incoming-trust.ps1
[outgoing-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/outgoing-trust.ps1
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-adds.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/add-adds-domain-controller.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-public.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-mgmt.parameters.json
[web-vm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/web-vm-domain-join.parameters.json
[solution-components]: [#solution_components]
[0]: ./media/guidance-identity-aad-resource-forest/figure1.png "Secure hybride netwerkarchitectuur afzonderlijke AD-domeinen"