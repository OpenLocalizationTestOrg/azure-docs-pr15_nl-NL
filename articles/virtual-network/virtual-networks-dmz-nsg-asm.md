<properties
   pageTitle="Voorbeeld van DMZ – maken een eenvoudige DMZ met NSGs | Microsoft Azure"
   description="Een DMZ netwerk beveiligingsgroepen (NSG) maken"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor;sivae"/>

# <a name="example-1--build-a-simple-dmz-with-nsgs"></a>Voorbeeld 1: een eenvoudige DMZ met NSGs maken

[Ga terug naar de beveiliging grens aanbevolen procedures pagina][HOME]

In dit voorbeeld wordt een eenvoudige DMZ maken met vier windows-servers en beveiligingsgroepen netwerk. Dit wordt ook Doorloop de relevante opdrachten op te geven van een beter begrip van elke stap. Er is ook een sectie verkeer Scenario te leveren een uitgebreidere stapsgewijze hoe de lagen wordt beveiligd in de DMZ verkeer doorlopen. In de verwijzingen is sectie ten slotte de volledige code en instructies voor het maken van deze omgeving om te testen en experimenteren met verschillende scenario's. 

![Binnenkomende DMZ met NSG][1]

## <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement dat de volgende onderdelen bevat:

- Twee cloud services: "FrontEnd001" en "BackEnd001"
- Een virtueel netwerk, "CorpNetwork", met twee subnetten; 'FrontEnd' en "BackEnd"
- Een netwerk-beveiligingsgroep die is toegepast op beide subnetten
- Een Windows-Server met een webserver toepassing ("IIS01")
- Twee windows-servers die terug toepassing beëindigen servers ("AppVM01", "AppVM02")
- Een Windows-server met een DNS-server ("DNS01")

In de sectie verwijzingen onder is er een PowerShell-script die wordt meestal de omgeving hierboven beschreven maken. Samenstellen van de VMs en virtuele netwerken, hoewel worden uitgevoerd door de voorbeeldscript wordt niet in worden beschreven in dit document. 

Maken van de omgeving;

  1.    Sla het netwerk config XML-bestand zijn opgenomen in de sectie Verwijzingen (bijgewerkt met de namen, locatie en IP-adressen zodat deze overeenkomen met het opgegeven scenario)
  2.    Bijwerken van de gebruikersvariabelen in het script zodat deze overeenkomt met de omgeving die het script is uitgevoerd voor (abonnementen, servicenamen, enzovoort)
  3.    Het script uitvoeren in PowerShell

**Opmerking**: de regio aangegeven in de PowerShell-script moet overeenkomen met de aangegeven in het netwerk van het XML-configuratiebestand regio.

Als het script is uitgevoerd met succes extra wordt optionele stappen kunnen worden genomen, wordt in de sectie verwijzingen zijn twee scripts voor het instellen van de webserver en de app-server met een eenvoudige webtoepassing toe te staan dat testen met deze DMZ-configuratie.

De volgende secties krijgt een gedetailleerde beschrijving van netwerk beveiligingsgroepen en hoe ze werken voor dit voorbeeld door regels van de PowerShell-script doorlopen.

## <a name="network-security-groups-nsg"></a>Beveiligingsgroepen netwerk (NSG)
In dit voorbeeld is een groep NSG ingebouwd en vervolgens met zes regels worden geladen. 

>[AZURE.TIP] In het algemeen, moet u eerst uw specifieke "Toestaan" regels maken en vervolgens de algemene "Weigeren" regels laatste. De toegewezen prioriteit bepaalt welke regels worden geëvalueerd eerste. Als verkeer toe te passen op een bepaalde regel wordt gevonden, wordt geen verdere regels worden geëvalueerd. NSG regels kunnen toepassen op een in de richting voor binnenkomende of uitgaande (vanuit het perspectief van het subnet).

Declaratieve, worden de volgende regels worden gemaakt voor binnenkomende verkeer:

1.  Interne DNS-verkeer (poort 53) is toegestaan
2.  RDP-verkeer (poort 3389) van Internet voor elke VM is toegestaan
3.  HTTP-verkeer (poort 80) van Internet naar de webserver (IIS01) is toegestaan
4.  Een verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan
5.  Een verkeer (alle poorten) van Internet op de gehele VNet (beide subnetten) is geweigerd
6.  Een verkeer (alle poorten) van het subnet Frontend naar de Backend-subnet is geweigerd

Met deze regels is gebonden aan elk subnet, als een HTTP-aanvraag inkomende van Internet naar de webserver, beide regels 3 is (toestaan) en 5 (weigeren) wilt toepassen, maar aangezien regel 3 een hogere prioriteit heeft slechts zou worden toegepast en regel 5 niet afspelen komen zou. Dus de HTTP-aanvraag mogen de webserver. Als dat dezelfde verkeer is probeert te bereiken van de server DNS01, regel 5 (weigeren) zou de eerste om toe te passen en het verkeer niet mogen worden doorgegeven aan de server. Regel 6 (weigeren) wordt geblokkeerd het subnet Frontend in gesprekken met de Backend-subnet (behalve toegestane verkeer in regels 1 en 4), beveiligt u hiermee de Backend-netwerk in hoofdletters/kleine letters een aanvaller compromissen de webtoepassing op de Frontend, de hacker zou hebben beperkte toegang tot het Backend "beveiligde" netwerk (alleen voor resources die worden aangeboden op de server AppVM01).

Er is een standaard uitgaande regel waarmee verkeer af met internet. In dit voorbeeld bent we uitgaand verkeer toestaan en alle uitgaande regels niet wordt gewijzigd. Als u wilt vergrendelen verkeer in beide richtingen, is gebruiker gedefinieerd routering vereist, dit in "Voorbeeld 3" is verkend hieronder.

Elke regel is als volgt in detail beschreven (**notitie**: een item in de onderstaande lijst in die begint met een dollarteken (bijvoorbeeld: $NSGName) een door de gebruiker gedefinieerde variabele van het script in het gedeelte naslaginformatie van dit document):

1. Een beveiligingsgroep netwerk moeten eerst worden gemaakt waarin de regels:

        New-AzureNetworkSecurityGroup -Name $NSGName `
            -Location $DeploymentLocation `
            -Label "Security group for $VNetName subnets in $DeploymentLocation"
 
2.  De eerste regel in dit voorbeeld wordt de DNS-verkeer tussen alle interne netwerken aan de DNS-server in de backend-subnet toestaan. Er zijn enkele belangrijke parameters voor de regel:
  - "Typ" duidt op in welke richting van verkeer met deze regel wordt pas van kracht; Dit is vanuit het perspectief van de subnet of VM (afhankelijk van waar deze NSG afhankelijk is). Dus als Type is "Inkomend" en verkeer is het subnet invoeren, de regel toepassen wilt en verkeer blijft het subnet niet door deze regel geldt.
  - "Prioriteit" Hiermee stelt u de volgorde waarin netwerkverkeer wordt geëvalueerd. Hoe lager het nummer hoe hoger de prioriteit. Zodra u een regel is van toepassing op specifieke netwerkverkeer, wordt geen verdere regels worden verwerkt. Dus als een regel met prioriteit 1 verkeer staat, en een regel met prioriteit 2 al dan niet toestaan verkeer, en beide regels op verkeer toepassen wordt het verkeer mogen flow (Aangezien regel 1 een hogere prioriteit had dit effect duurde en geen verdere regels zijn toegepast).
  - "Actie" duidt als verkeer waarop deze regel is geblokkeerd of toegestaan.

            Get-AzureNetworkSecurityGroup -Name $NSGName | `
                Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
                -Type Inbound -Priority 100 -Action Allow `
                -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
                -DestinationAddressPrefix $VMIP[4] `
                -DestinationPortRange '53' `
                -Protocol *

3.  Deze regel wordt RDP verkeer van internet op de RDP-poort van een server in beide subnet in de VNET toestaan. Deze regel gebruikt twee speciale typen adres voorvoegsels voor eenheden; "VIRTUAL_NETWORK" en "INTERNET". Dit is een eenvoudige manier om een grotere categorie adres voorvoegsels voor eenheden.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" `
            -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '3389' `
            -Protocol *

4.  Deze regel kunt binnenkomende internetverkeer te raken, de webserver. Dit wijzigen niet het routeren gedrag; staat verkeer voor IIS01 om door te geven. Dus als verkeer van Internet de webserver had als doel deze regel zou gebruik hiervan is toegestaan en stoppen nadere verwerking van regels. (In de regel prioriteit 140 alle andere inkomende internetverkeer is geblokkeerd). Als u alleen HTTP-verkeer verwerkt bent, kan deze regel verder worden beperkt als u wilt dat alleen bestemming poort 80.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable Internet to $VMName[0]" `
            -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] `
            -DestinationPortRange '*' `
            -Protocol *

5.  Deze regel staat verkeer door te geven van de server IIS01 op de server AppVM01, een hoger regel blokken alle andere Frontend naar Backend-verkeer is toegestaan. Om te verbeteren met deze regel, als de poort bekend is dat moet worden toegevoegd. Bijvoorbeeld als de IIS-server alleen SQL Server op AppVM01 raken is, het bereik van de poort bestemming moet worden gewijzigd van "*" (willekeurig) naar 1433 (de SQL-poort), zodat een kleinere inkomende aanvallen op AppVM01 of moet de webtoepassing ooit worden is gehackt.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] to $VMName[2]" `
            -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
 
6.  Deze regel al dan niet toestaan van verkeer van internet voor servers in het netwerk. In combinatie met de regel prioriteit 110 en 120, kunt u alleen binnenkomende internetverkeer naar de firewall en RDP-poorten naar andere servers en blokken rest. 

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $VNetName VNet from the Internet" `
            -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '*' `
            -Protocol *
 
7.  De laatste regel al dan niet toestaan van verkeer uit het subnet Frontend de Backend-subnet. Aangezien dit een binnenkomende alleen regel, worden omgekeerde verkeer is toegestaan (vanuit de Backend naar de Frontend).

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" `
            -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix `
            -DestinationPortRange '*' `
            -Protocol * 

## <a name="traffic-scenarios"></a>Verkeer scenario 's
#### <a name="allowed-web-to-web-server"></a>(*Toegestaan*) Web naar de webserver
1.  HTTP-pagina van Internet gebruiker serviceaanvragen van FrontEnd001.CloudApp.Net (Internet die tegenover elkaar liggen Cloud Service)
2.  Cloud service stadia verkeer via openen endpoint op poort 80 richting IIS01 (de webserver)
3.  Frontend subnet begint met binnenkomende regel verwerking:
  1.    NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
  2.    NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
  3.    NSG regel 3 (Internet naar IIS01) is van toepassing, wordt verkeer is toegestaan, zelfs niet meer regel verwerking
4.  Verkeer raakt interne IP-adres van de webserver IIS01 (10.0.1.5)
5.  IIS01 luistert naar webverkeer, dit verzoek ontvangt en start de verwerking van de aanvraag
6.  IIS01 wordt gevraagd om de SQL-Server op AppVM01 informatie
7.  Geen uitgaande regels op Frontend subnet verkeer is toegestaan
8.  De Backend-subnet begint met binnenkomende regel verwerking:
  1.    NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
  2.    NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
  3.    NSG regel 3 (Internet voor de Firewall) niet toepassen, naar de volgende regel verplaatsen
  4.    NSG regel 4 (IIS01 naar AppVM01) is van toepassing, wordt verkeer is toegestaan, regel verwerking stoppen
9.  AppVM01 ontvangt van de SQL-Query en reageert
10. Aangezien er geen uitgaande regels op de Backend-subnet is het antwoord toegestaan
11. Frontend subnet begint met binnenkomende regel verwerking:
  1.    Er is geen NSG-regel die van toepassing op inkomend verkeer van de Backend-subnet naar het Frontend subnet, zodat geen van de NSG regels toepassen
  2.    De standaard-systeem regel het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
12. De IIS-server ontvangt van het SQL-antwoord en is voltooid het HTTP-antwoord en wordt verzonden naar degene
13. Aangezien er geen uitgaande regels op de Frontend subnet het antwoord is toegestaan en Internet wordt de pagina met webonderdelen aangevraagd.

#### <a name="allowed-rdp-to-backend"></a>(*Toegestaan*) RDP naar Backend
1.  De beheerder van de server op internet aanvraagt RDP-sessie met AppVM01 op BackEnd001.CloudApp.Net:xxxxx waar xxxxx het willekeurig poortnummer voor RDP naar AppVM01 is (de toegewezen poort vindt u in de Portal Azure- of via PowerShell)
2.  Back-end subnet begint met binnenkomende regel verwerking:
  1.    NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
  2.    NSG regel 2 (RDP) is van toepassing, wordt verkeer is toegestaan, zelfs niet meer regel verwerking
3.  Met geen uitgaande regels standaardregels toepassen en afzender verkeer is toegestaan
4.  RDP-sessie is ingeschakeld
5.  AppVM01 gebruikersnaam wachtwoord gevraagd

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(*Toegestaan*) Web Server DNS opzoeken op DNS-server
1.  Web Server, IIS01, een gegevensfeed bij www.data.gov behoeften, maar behoeften voor het oplossen van het adres.
2.  De netwerkconfiguratie voor de lijsten VNet DNS01 (10.0.2.4 op de Backend-subnet) als de primaire DNS-server, in IIS01 wordt de DNS-aanvraag verzonden naar DNS01
3.  Geen uitgaande regels op Frontend subnet verkeer is toegestaan
4.  Back-end subnet begint met binnenkomende regel verwerking:
  1.     NSG regel 1 (DNS) is van toepassing, wordt verkeer is toegestaan, zelfs niet meer regel verwerking
5.  DNS-server ontvangt de aanvraag
6.  DNS-server geen het adres in cache opgeslagen en wordt gevraagd om een hoofdsite DNS-server op internet
7.  Geen uitgaande regels op Backend subnet verkeer is toegestaan
8.  Internet-DNS-server reageert, omdat deze sessie intern is gestart, is het antwoord toegestaan
9.  DNS-server in de cache opgeslagen het antwoord en reageert op de eerste aanvraag terug naar IIS01
10. Geen uitgaande regels op Backend subnet verkeer is toegestaan
11. Frontend subnet begint met binnenkomende regel verwerking:
  1.    Er is geen NSG-regel die van toepassing op inkomend verkeer van de Backend-subnet naar het Frontend subnet, zodat geen van de NSG regels toepassen
  2.    De standaard-systeem regel het verkeer tussen subnetten zou dat deze verkeer is toegestaan, zodat het verkeer is toegestaan
12. IIS01 krijgt de reactie van DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Toegestaan*) Web Server access-bestand op AppVM01
1.  IIS01 wordt gevraagd om een bestand in AppVM01
2.  Geen uitgaande regels op Frontend subnet verkeer is toegestaan
3.  De Backend-subnet begint met binnenkomende regel verwerking:
  1.    NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
  2.    NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
  3.    NSG regel 3 (Internet naar IIS01) niet toepassen, naar de volgende regel verplaatsen
  4.    NSG regel 4 (IIS01 naar AppVM01) is van toepassing, wordt verkeer is toegestaan, regel verwerking stoppen
4.  AppVM01 de aanvraag ontvangt en beantwoordt met bestand (ervan uitgaande dat er toegang wordt geverifieerd)
5.  Aangezien er geen uitgaande regels op de Backend-subnet is het antwoord toegestaan
6.  Frontend subnet begint met binnenkomende regel verwerking:
  1.    Er is geen NSG-regel die van toepassing op inkomend verkeer van de Backend-subnet naar het Frontend subnet, zodat geen van de NSG regels toepassen
  2.    De standaard-systeem regel het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
7.  Het bestand is ontvangen de IIS-server

#### <a name="denied-web-to-backend-server"></a>(*Geweigerd*) Web naar endserver
1.  Internet-gebruiker probeert te openen van een bestand op AppVM01 via de BackEnd001.CloudApp.Net-service
2.  Aangezien er geen eindpunten geopend om bestandsshare, dit zou de Cloud-Service niet doorgeven en de server wouldn't bereiken
3.  Als de eindpunten geopend om welke reden waren, wordt NSG regel 5 (Internet naar VNet) dit verkeer geblokkeerd

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(*Geweigerd*) Web DNS opzoeken op DNS-server
1.  Internet-gebruiker probeert aan te melden voor het opzoeken van een interne DNS-record op DNS01 via de BackEnd001.CloudApp.Net-service
2.  Aangezien er geen eindpunten openen voor DNS, dit zou de Cloud-Service niet doorgeven en de server wouldn't bereiken
3.  Als de eindpunten geopend om welke reden waren, NSG regel 5 (Internet naar VNet) dit verkeer wordt geblokkeerd (notitie: deze regel 1 (DNS) niet van toepassing om twee redenen, eerst het bronadres internet is, deze regel geldt alleen voor de lokale VNet als bron, dit is tevens een regel voor het toestaan, zodat deze wordt nooit verkeer geweigerd)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(*Geweigerd*) Web op SQL-toegang via Firewall
1.  Internet-gebruiker aanvraagt SQL-gegevens uit FrontEnd001.CloudApp.Net (Internet die tegenover elkaar liggen Cloud Service)
2.  Aangezien er geen eindpunten openen voor SQL, dit zou de Cloud-Service niet doorgeven en de firewall wouldn't bereikt
3.  Als de eindpunten waren geopend om welke reden, begint het subnet Frontend binnenkomende regel verwerking:
  1.    NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
  2.    NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
  3.    NSG regel 3 (Internet naar IIS01) is van toepassing, wordt verkeer is toegestaan, zelfs niet meer regel verwerking
4.  Verkeer raakt interne IP-adres van de IIS01 (10.0.1.5)
5.  IIS01 is niet luisteren op poort 1433, dus geen antwoord op de aanvraag

## <a name="conclusion"></a>Sluiten
Dit is een relatief eenvoudig en rechtstreeks doorsturen manier voor het back-end-subnet van binnenkomende verkeer isoleren.

Als u meer voorbeelden en een overzicht van het netwerk beveiligingsgrenzen vindt u [hier][HOME].

## <a name="references"></a>Verwijzingen
### <a name="main-script-and-network-config"></a>Belangrijkste Script en netwerkconfiguratie
Sla het volledige Script in een PowerShell-script-bestand. De netwerkconfiguratie opslaan in een bestand met de naam 'NetworkConf1.xml'.
Wijzig de variabelen door de gebruiker gedefinieerde desgewenst. Het script uitvoeren en voert u de Firewall regel setup instructie opgenomen in de bovenstaande voorbeeld 1-sectie.

#### <a name="full-script"></a>Volledige Script
Dit script wordt, op basis van de gebruiker gedefinieerde variabelen,

1.  Verbinding maken met een Azure-abonnement
2.  Maak een nieuwe opslag-account
3.  Een nieuwe VNet en twee subnetten zoals gedefinieerd in het netwerk configuratiebestand maken
4.  4 windows server VMs maken
5.  Configureer NSG waaronder:
  - Een NSG maken
  - Deze vullen met regels
  - De NSG binden naar de juiste subnetten

Deze PowerShell-script moet lokaal op worden uitgevoerd dat een internet verbonden PC of server.

>[AZURE.IMPORTANT] Wanneer dit script wordt uitgevoerd, zijn er mogelijk waarschuwingen of andere informatieve berichten die pop-in PowerShell. Alleen foutberichten worden weergegeven in het rood zijn oorzaak van belang.


    <# 
     .SYNOPSIS
      Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared
      
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.
    
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.5
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
      
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 0
        #       - The AppVM1 Server must be VM 1
        #       - The DNS server must be VM 3
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
        # VM 1 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 2 - The Second Application Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 3 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #   

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                Remove-AzureEndpoint -Name "PowerShell" | `
                New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Note: A Remote Desktop endpoint is automatically created when each VM is created.
            $i++
        }
        # Add HTTP Endpoint for IIS01
        Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) to $($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
            -Protocol *
        
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *
    
        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
      

#### <a name="network-config-file"></a>Configuratiebestand netwerk
Dit xml-bestand opslaan met de bijgewerkte locatie en de koppeling toevoegen aan dit bestand naar de variabele $NetworkConfigFile in het bovenstaande script.
    
    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Toepassing voorbeeldscripts
Als u wilt bij het installeren van een toepassing voor de steekproef voor dit en andere voorbeelden DMZ, een is voldaan aan de volgende koppeling: [Voorbeeldscript van toepassing][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Binnenkomende DMZ met NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

