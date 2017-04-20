<properties
   pageTitle="Voorbeeld DMZ – maken een DMZ netwerken met een Firewall, UDR en NSG beschermen | Microsoft Azure"
   description="Maken van een DMZ met een Firewall, door de gebruiker gedefinieerde routeren (UDR) en beveiligingsgroepen netwerk (NSG)"
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

# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a>Voorbeeld 3: een DMZ netwerken met een Firewall, UDR en NSG beschermen maken

[Ga terug naar de beveiliging grens aanbevolen procedures pagina][HOME]

In dit voorbeeld wordt een DMZ maken met een firewall, vier windows-servers, gebruiker gedefinieerd routering, IP-doorsturen en beveiligingsgroepen netwerk. Dit wordt ook Doorloop de relevante opdrachten op te geven van een beter begrip van elke stap. Er is ook een sectie verkeer Scenario te leveren een uitgebreidere stapsgewijze hoe de lagen wordt beveiligd in de DMZ verkeer doorlopen. In de verwijzingen is sectie ten slotte de volledige code en instructies voor het maken van deze omgeving om te testen en experimenteren met verschillende scenario's. 

![Bidirectionele DMZ met NVA, NSG en UDR][1]

## <a name="environment-setup"></a>Omgeving instellen
In dit voorbeeld is er een abonnement dat de volgende onderdelen bevat:

- Drie cloud services: "SecSvc001", "FrontEnd001" en "BackEnd001"
- Een virtueel netwerk "CorpNetwork", met drie subnetten: "SecNet", 'FrontEnd' en "BackEnd"
- Een netwerk virtuele toestel, in dit voorbeeld een firewall, verbonden met het subnet SecNet
- Een Windows-Server met een webserver toepassing ("IIS01")
- Twee windows-servers die terug toepassing beëindigen servers ("AppVM01", "AppVM02")
- Een Windows-server met een DNS-server ("DNS01")

In de sectie verwijzingen onder is er een PowerShell-script die wordt meestal de omgeving hierboven beschreven maken. Samenstellen van de VMs en virtuele netwerken, hoewel worden uitgevoerd door de voorbeeldscript wordt niet in worden beschreven in dit document.

De omgeving maken:

  1.    Sla het netwerk config XML-bestand zijn opgenomen in de sectie Verwijzingen (bijgewerkt met de namen, locatie en IP-adressen zodat deze overeenkomen met het opgegeven scenario)
  2.    Bijwerken van de gebruikersvariabelen in het script zodat deze overeenkomt met de omgeving die het script is uitgevoerd voor (abonnementen, servicenamen, enzovoort)
  3.    Het script uitvoeren in PowerShell

**Opmerking**: de regio aangegeven in de PowerShell-script moet overeenkomen met de aangegeven in het netwerk van het XML-configuratiebestand regio.

Zodra het script uitgevoerd kunnen de volgende post script stappen worden genomen:

1.  De firewallregels hebt ingesteld, hierover vindt u in het gedeelte hierna getiteld: Beschrijving van de regel Firewall.
2.  (Optioneel) zijn in de sectie Verwijzingen in twee scripts voor het instellen van de webserver en de app-server met een eenvoudige webtoepassing toe te staan dat testen met deze DMZ-configuratie.

Zodra het script uitgevoerd in de firewall regels moet worden uitgevoerd, zijn dit van toepassing op in de sectie: firewallregels.

## <a name="user-defined-routing-udr"></a>Door gebruiker gedefinieerde routeren (UDR)
Standaard worden de volgende systeem routes gedefinieerd als:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

De VNETLocal is altijd de prefix(es) gedefinieerde adres van de VNet voor dat specifieke netwerk (ie verandert van VNet VNet afhankelijk van hoe elke specifieke VNet wordt gedefinieerd). De resterende systeem routes zijn statische en standaard als hierboven.

Als voor de prioriteit, routes via de methode langste voorvoegsel vergelijken (LPM) worden verwerkt, dus de meest specifieke route in de tabel wilt toepassen op een bepaald doele-mailadres.

Daarom zou verkeer (bijvoorbeeld naar de server DNS01 10.0.2.4) voor het lokale netwerk (10.0.0.0/16) worden gerouteerd via de VNet naar de bestemming vanwege de route 10.0.0.0/16. Met andere woorden, voor 10.0.2.4 is de route 10.0.0.0/16 de meest specifieke route, hoewel de 10.0.0.0/8 en 0.0.0.0/0 ook kunnen toepassen, maar omdat ze kleiner specifieke ze niet van invloed zijn op dit verkeer. Het verkeer naar 10.0.2.4 zou hebben een volgende hop van de lokale VNet en dus gewoon doorsturen naar de bestemming.

Als het verkeer is bedoeld voor 10.1.1.1 bijvoorbeeld, de route 10.0.0.0/16 wouldn't toepassen, maar de 10.0.0.0/8 zou het meest specifieke en dit het verkeer is overgeslagen ("zwart gaan") omdat de volgende hop Null is. 

Als de bestemming niet op een van de voorvoegsels null-waarden of de voorvoegsels VNETLocal toepassen en deze u de specifieke voert zou uit naar Internet worden gerouteerd als de volgende hop en dus af van de Azure internet rand en te routeren, 0.0.0.0/0.

Als er twee identieke voorvoegsels voor eenheden in de routetabel zijn, ziet hieronder u de volgorde van voorkeur in op basis van het kenmerk routes 'source'.

1.  "VirtualAppliance" = van een gebruiker gedefinieerde Route handmatig worden toegevoegd aan de tabel
2.  "VPNGateway" = de Route van een dll (BGP gebruikt in combinatie met hybride-netwerken te gebruiken), die worden toegevoegd door een dynamische netwerkprotocol, deze routes kunnen wijzigen tijdens het dynamische protocol wordt automatisch in peered netwerk aangepast
3.  "Standaard" = de Routes systeem, de lokale VNet en de statische items, zoals wordt weergegeven in de bovenstaande tabel.

>[AZURE.NOTE] U kunt nu gebruiker gedefinieerd routering (UDR) gebruiken met ExpressRoute en VPN Gateways afdwingen van binnenkomende en uitgaande cross-premises verkeer naar een netwerk virtuele toestel (NVA) worden doorgestuurd.

#### <a name="creating-the-local-routes"></a>De lokale routes maken

In dit voorbeeld worden twee routeren tabellen nodig, één voor de front-end en back-end-subnetten. Elke tabel is met statische routes geschikt te maken voor het opgegeven subnet geladen. In dit voorbeeld heeft elke tabel drie routes:

1. Lokale subnetverkeer met geen volgende Hop gedefinieerd als u wilt dat lokale subnet verkeer is toegestaan, worden omzeild de firewall
2. Virtuele netwerkverkeer met een volgende Hop gedefinieerd als firewall, hierdoor wordt de standaardregel waarmee lokaal VNet verkeer om te leiden rechtstreeks
3. Alle resterende verkeer (0/0) met een volgende Hop gedefinieerd als de firewall

Nadat u hebt gemaakt met de routering tabellen zijn ze hun subnetten afhankelijk. Voor het subnet Frontend ziet routeren tabel, eenmaal hebt gemaakt en die is gebonden aan het subnet er als volgt:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


In dit voorbeeld de volgende opdrachten worden gebruikt om te maken van de tabel, de route van een door de gebruiker gedefinieerde toevoegen en maak vervolgens afhankelijk van de tabel een subnet (notitie; items onder die begint met een dollarteken (bijvoorbeeld: $BESubnet) door de gebruiker gedefinieerde variabelen uit het script in het gedeelte naslaginformatie van dit document zijn):

1.  Eerst moet de routering basistabel worden gemaakt. Het codefragment van deze ziet u het maken van de tabel voor het back-end-subnet. In het script is ook een bijbehorende tabel gemaakt voor het subnet Frontend.

        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"

2.  Nadat de routetabel is gemaakt, kunnen u specifieke door de gebruiker gedefinieerde routes toevoegen. In dit snipped, worden al het verkeer (0.0.0.0/0) gerouteerd via de virtuele toestel (een variabele, $VMIP [0], wordt gebruikt om door te geven in het IP-adres dat is toegewezen wanneer het virtuele toestel eerder in het script is gemaakt). In het script is ook een bijbehorende regel gemaakt in de tabel Frontend.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

3. De bovenstaande routevermelding wordt overschreven door de standaard "0.0.0.0/0" route, maar de standaard 10.0.0.0/16 regel nog steeds bestaande waarmee verkeer binnen de VNet om te leiden rechtstreeks naar de bestemming en niet op het netwerk virtuele toestel. Juiste dit gedrag van de regel volgen moet worden toegevoegd.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

4. Er is op dit moment een keuze moet plaatsvinden. Met de bovenstaande twee routes doorsturen al het verkeer naar de firewall voor beoordeling, even verkeer binnen één subnet. Dit mogelijk wenselijk, maar dat verkeer is toegestaan in een subnet voor het routeren lokaal zonder betrokkenheid van de firewall van een derde, zeer specifieke regel kan worden toegevoegd. Deze route Staten die een willekeurig-mailadres voor het lokale subnet kunt just destine routeren er rechtstreeks (NextHopType = VNETLocal).

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal

5.  Ten slotte de routering tabel gemaakt en gevuld met een door de gebruiker gedefinieerde routes, moet de tabel nu zijn gekoppeld aan een subnet. In het script, de front-end van route-tabel ook afhankelijk van het subnet Frontend. Hier ziet u het script binding voor het back-end-subnet.

        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
           -SubnetName $BESubnet `
           -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>IP-doorsturen
Een functie companion naar UDR, is het IP-doorsturen. Dit is een instelling op een virtuele toestel die kunnen worden ontvangen verkeer niet specifiek is geadresseerd aan het toestel en stuurt vervolgens dat verkeer naar de uiteindelijke bestemming.

Als u bijvoorbeeld als verkeer van AppVM01 zorgt ervoor dat een verzoek om naar de server DNS01 zou UDR doorsturen dit naar de firewall. Met het IP-doorsturen ingeschakeld, worden wordt het verkeer naar de bestemming van de DNS01 (10.0.2.4) geaccepteerd door het toestel (adres 10.0.0.4) en vervolgens worden doorgestuurd naar de uiteindelijke bestemming (10.0.2.4). Zonder het IP-doorsturen ingeschakeld op de Firewall, zou verkeer niet worden geaccepteerd door het toestel Hoewel de routetabel de firewall als de volgende hop bevat. 

>[AZURE.IMPORTANT] Het is belangrijk moet denken om het IP-doorsturen inschakelen in combinatie met de gebruiker gedefinieerde routering.

Instellen van het IP-doorschakelen is één opdracht en op de aanmaaktijd van een VM kan worden uitgevoerd. Het codefragment is naar het einde van het script en gegroepeerd, waarin de opdrachten UDR voor de stroom van dit voorbeeld:

1.  Bel het exemplaar VM die uw virtuele toestel, de firewall in dit geval is en IP-doorsturen inschakelen (notitie; een item in het rood die begint met een dollarteken (bijvoorbeeld: $VMName[0]) is een door de gebruiker gedefinieerde variabele van het script in het gedeelte naslaginformatie van dit document. De nul vierkante haken, [0], staat voor de eerste VM in de matrix van VMs, om het voorbeeldscript te werken zonder wijzigingen, de eerste VM (VM 0), moet de firewall):

        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
           Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Beveiligingsgroepen netwerk (NSG)
In dit voorbeeld is een groep NSG ingebouwd en vervolgens met een enkele regel is geladen. Deze groep wordt nu gebonden alleen aan de front-end en back-end-subnetten (niet de SecNet). Declaratieve wordt de volgende regel opgebouwd:

1.  Al het verkeer (alle poorten) van Internet naar de hele VNet (alle subnetten) is geweigerd

Hoewel NSGs in dit voorbeeld worden gebruikt, wordt het primaire doel is als een secundaire laag beveiligd tegen handmatige onjuiste configuratie. We wilt blokkeren, alle binnenkomende verkeer van internet naar een de Frontend of Backend-subnetten, verkeer alleen door de SecNet subnet, de firewall moeten stromen (en vervolgens als nodig bij de Frontend of Backend subnetten). Plus, met de regels UDR op hun plaats staan, zou al het verkeer die u deze in de Frontend of Backend-subnetten plaatst doorgestuurd uit naar de firewall (waardering UDR). De firewall ziet dit als een asymmetrische stroom en uitgaand verkeer wilt neerzetten. Dus zijn er drie lagen van beveiliging beschermen de front-end en back-end-subnetten; 1) geen geopende eindpunten op de FrontEnd001 en BackEnd001 cloud services, 2) NSGs verkeer van Internet, 3) de firewall weghalen asymmetrische verkeer weigeren.

Een interessant punt met betrekking tot de beveiligingsgroep van netwerk in dit voorbeeld is het tabblad bevat slechts één regel, hieronder, namelijk weigeren internetverkeer naar het hele virtuele netwerk waarin het subnet beveiliging wilt opnemen. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Echter, aangezien het NSG is alleen gekoppeld aan de front-end en back-end-subnetten, de regel niet is verwerkt op verkeer naar het subnet beveiliging (inkomend). Hoewel de regel NSG wordt gemeld er geen internetverkeer naar een adres op de VNet, dat omdat de NSG is nooit afhankelijk van het subnet beveiliging, wordt daardoor verkeer naar het subnet beveiliging flow.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName
    
    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Firewallregels
Klik op de firewall moet doorstuurregels worden gemaakt. Aangezien de firewall blokkeren of doorsturen van alle inkomend, uitgaand en binnen VNet verkeer is zijn veel firewallregels nodig. De beveiligingsservice openbare IP-adres (op verschillende poorten), wordt ook, druk op alle binnenkomende verkeer om te worden verwerkt door de firewall. Een goede gewoonte is de logische loopt voordat u de subnetten instelt diagram te en later Herschrijf de firewallregels om te voorkomen. De volgende afbeelding is een logische weergave van de firewallregels voor dit voorbeeld:
 
![Logische weergave van de firewallregels][2]

>[AZURE.NOTE] Op basis van het netwerk virtuele toestel gebruikt, varieert de poorten management. In dit voorbeeld die een Barracuda NextGen Firewall wordt verwezen gebruikt die poorten 22, 801 en 807. Zie de documentatie van de leverancier toestel als u wilt zoeken naar de exacte poorten gebruikt voor het beheer van het apparaat wordt gebruikt.

### <a name="logical-rule-description"></a>Logische Regelbeschrijving
In het bovenstaande logische diagram het subnet beveiliging niet wordt weergegeven omdat de firewall het enige resource op dat subnet is, en dit diagram wordt weergegeven de firewallregels en hoe ze logisch toestaan of verkeersstromen en niet de werkelijke routering pad weigeren. Daarnaast de externe poorten geselecteerd voor de RDP-verkeer hoger zijn varieerde poorten (8014 – 8026) en zijn geselecteerd enigszins uitlijnen met de laatste twee achttallen van het lokale IP-adres voor gemakkelijker leesbaarheid (bijvoorbeeld lokale serveradres 10.0.1.4 is gekoppeld aan externe poort 8014), maar een hogere niet-conflicterende poorten kunnen worden gebruikt.

In dit voorbeeld we nodig 7 soorten regels, soort van de regel worden beschreven als volgt:

- Externe regels (voor binnenkomende verkeer):
  1.    Firewall Management regel: Met deze App omleidings-regel staat verkeer worden doorgegeven aan de poorten beheer van het netwerk virtuele toestel.
  2.    Regels voor RDP (voor elke windows-server): deze vier regels (één voor elke server) wordt beheer van de verschillende servers via RDP toestaan. Dit kan ook worden samengevoegd in één regel, afhankelijk van de mogelijkheden van het netwerk virtuele toestel die wordt gebruikt.
  3.    Regels van toepassing-verkeer: Er zijn twee toepassing verkeer regels, de eerste voor de front-end van web-verkeer en de tweede voor het back-end-verkeer (bijvoorbeeld webserver naar gegevenslaag). De configuratie van deze regels worden, hangt af van de netwerkarchitectuur (waar uw servers worden geplaatst) en verkeer loopt (welke richting de verkeersstromen en welke poorten worden gebruikt).
      - De eerste regel, kan het verkeer werkelijke toepassing de toepassingsserver te bereiken. Terwijl de andere regels toestaan voor beveiliging, management, enzovoort, worden regels van toepassing zijn wat toestaan van externe gebruikers of services voor toegang tot de toepassingen. In dit voorbeeld is er één webserver op poort 80, dus een regel van de toepassing één firewall binnenkomende verkeer worden omgeleid naar de externe IP-adres, het web servers interne IP-adres. De sessie omgeleide verkeer zou worden NAT gehad toe aan het interne server.
      - De tweede regel van de toepassing-verkeer is de back-end-regel toe te staan dat de webserver contact opnemen met de AppVM01-server (maar niet AppVM02) via een poort.
- Interne regels (voor binnen-VNet verkeer)
  4.    Uitgaande op Internet regel: met deze regel wordt dat verkeer is toegestaan met een netwerk worden doorgegeven aan de geselecteerde netwerken. Deze regel is meestal een standaardregel al op de firewall, maar in de fase uitgeschakeld. Deze regel moet worden ingeschakeld voor dit voorbeeld.
  5.    DNS-regel: Met deze regel staat alleen DNS (poort 53) verkeer worden doorgegeven aan de DNS-server. Voor deze omgeving die meeste verkeer van de Frontend naar de Backend is geblokkeerd, kunt deze regel DNS specifiek bij een lokale subnet.
  6.    Subnet naar Subnet regel: met deze regel is toe te staan dat een server in de back-end-subnet verbinding maken met een server op de front-end van subnet (maar niet andersom).
- Foutveilige regel (voor verkeer die niet voldoet aan een van de bovenstaande):
  7.    Alle verkeer regel weigeren: Dit moet altijd de laatste regel (in prioriteit), en als zodanig als een verkeersstromen niet overeenkomt met de voorgaande regels die deze door deze regel wordt verwijderd. Dit is een standaardregel en meestal wordt geactiveerd, geen wijzigingen in het algemeen nodig zijn.

>[AZURE.TIP] Klik op de tweede regel in de verkeer toepassing, elke poort is toegestaan voor eenvoudig voorbeeld in een scenario voor reële de meest specifieke poort en -adresbereiken moeten worden gebruikt voor het beperken van aanvallen van deze regel.

<br />

>[AZURE.IMPORTANT] Zodra alle bovenstaande regels zijn gemaakt, is het belangrijk om te controleren van de prioriteit van elke regel om ervoor te zorgen verkeer wordt toegestaan of geweigerd naar wens. In dit voorbeeld worden de regels in de volgorde van prioriteit. Het is eenvoudig afmelden bij de firewall vanwege verkeerd geordende regels worden vergrendeld. Controleer of dat het beheer van de firewall zelf is altijd de absolute hoogste prioriteit regel ten minste.

### <a name="rule-prerequisites"></a>Vereisten voor de regel
Een vereiste voor de virtuele Machine de firewall actief zijn openbare eindpunten. De openbare eindpunten moeten zijn geopend voor de firewall verkeer verwerken. Er zijn drie typen verkeer in dit voorbeeld; 1) management verkeer naar besturingselement de firewall en firewallregels, 2) RDP-verkeer om te bepalen de windows-servers en 3) toepassing-verkeer is toegestaan. Dit zijn de drie kolommen van de typen verkeer in de rechterbovenhoek helft van de logische weergave van de firewallregels hierboven.

>[AZURE.IMPORTANT] Een belangrijke takeway hier is te onthouden dat **al** het verkeer via de firewall wordt verzonden. Dus met extern bureaublad naar de server IIS01, hoewel in de Front End-Cloudservice en klik op de front-end van subnet is, toegang tot deze server we RDP moet aan de firewall op poort 8014 en laat de firewall voor het routeren van de aanvraag intern naar de IIS01 RDP-poort. De knop 'Verbinden' van de Azure portal werken niet omdat er geen directe RDP-pad naar IIS01 (als de portal zien kan). Dit houdt in alle verbindingen van internet op de Service beveiliging en een poort, bijvoorbeeld secscv001.cloudapp.net:xxxx (overzicht het bovenstaande diagram voor de toewijzing van externe poort en interne IP- en poort).

Een eindpunt kan worden geopend op het moment VM gemaakt of stelt u opbouwen wanneer wordt uitgevoerd in het voorbeeldscript en hieronder wordt weergegeven in deze codefragment (notitie; elk item die begint met een dollarteken (bijvoorbeeld: $VMName[$i]) is een door de gebruiker gedefinieerde variabele van het script in het gedeelte naslaginformatie van dit document. De "$i" in haken bevinden, [$i], het nummer van de matrix van een specifieke VM in een matrix van VMs voorstelt):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Hoewel het niet duidelijk weergegeven hier vanwege het gebruik van variabelen, maar de eindpunten worden **alleen** geopend in de Cloud Security Service. Dit is om ervoor te zorgen dat alle inkomende verkeer wordt verwerkt (gerouteerd, NAT, verloren) door de firewall.

Een management-client moet zijn geïnstalleerd op een PC voor het beheren van de firewall en de configuraties nodig maken. Zie de documentatie van uw firewall (of andere NVA)-leverancier voor het beheren van het apparaat. De rest van deze sectie en de volgende sectie, Firewall regels maken, wordt de configuratie van de firewall zelf, via de leveranciers management-client (dat wil zeggen niet in de portal van Azure of PowerShell) beschreven.

Instructies voor het downloaden van de client en verbinding maakt met de Barracuda in dit voorbeeld gebruikt vindt u hier: [Barracuda NG beheerder](https://techlib.barracuda.com/NG61/NGAdmin)

Zodra aangemeld op de firewall, maar vóór firewallregels maakt, zijn er twee vereiste objectklassen waardoor maken van de regels eenvoudiger; Netwerk & Service objecten.

In dit voorbeeld moet drie benoemde netwerkobjecten gedefinieerde (account voor het subnet Frontend en de Backend-subnet, ook een object network voor het IP-adres van de DNS-server). Maken van een benoemde netwerk. vanaf het Barracuda NG-beheerdersdashboard, Ga naar het tabblad configuratie, in de sectie operationele configuratie regelset, klikt u vervolgens klikt u op 'Netwerken' in het menu Firewall objecten, klik op Nieuw in het menu Bewerken-netwerken te gebruiken. Het netwerkobject kan nu worden gemaakt, door de naam en het voorvoegsel te voegen:

![Een Object FrontEnd netwerk maken][3]
 
Hiermee maakt u een benoemde netwerk voor het FrontEnd subnet, een vergelijkbaar object moet worden gemaakt voor het back-end-subnet ook. De subnetten kunnen nu gemakkelijker verwezen met de naam in de firewallregels.

Voor het Object op de DNS-Server:

![Een DNS-Server-Object maken][4]

Deze één IP-Adresverwijzing gebruikt in een regel DNS later in het document.

De tweede vereiste objecten zijn Services objecten. Hierin worden de poorten RDP-verbinding voor elke server. Aangezien het bestaande RDP-object is gekoppeld aan de standaardpoort naar RDP, kunnen 3389, nieuwe Services worden gemaakt als u wilt dat verkeer is toegestaan in de externe poorten (8014-8026). De nieuwe poorten kunnen ook worden toegevoegd aan de bestaande RDP-service, maar voor eenvoudige demonstratie, een afzonderlijke regel voor elke server kan worden gemaakt. Naar een nieuwe RDP regel maken voor een server. vanaf het Barracuda NG-beheerdersdashboard, Ga naar het tabblad configuratie, in de sectie operationele configuratie op regelset, 'Services' in het menu Firewall objecten, schuif omlaag in de lijst met services en selecteer de service die "RDP". Klik met de rechtermuisknop en selecteer kopiëren, en vervolgens met de rechtermuisknop op en selecteer plakken. Er is nu een RDP-Copy1 Service-Object dat kan worden bewerkt. Met de rechtermuisknop op RDP-Copy1 en selecteert u bewerken, de Service bewerken Object venster omhoog zoals hier wordt weergegeven:

![Kopie van de standaard RDP regel][5]

De waarden kunnen worden bewerkt voor de RDP-service voor een specifieke server. Voor AppVM01 de bovenstaande standaard RDP-regel moet worden gewijzigd zodat een nieuwe servicenaam, beschrijving en externe RDP-poort die wordt gebruikt in het diagram figuur 8 (notitie: de poorten van de standaardwaarde RDP van 3389 worden gewijzigd in de externe poort wordt gebruikt voor deze specifieke server, in het geval van AppVM01 is de externe poort 8025) de gewijzigde service wordt weergegeven onder :

![AppVM01 regel][6]
 
Dit proces moet worden herhaald RDP-Services op de resterende servers; maken AppVM02, DNS01 en IIS01. Het maken van deze Services, kunt u het maken van de regel eenvoudiger en duidelijker in het volgende gedeelte.

>[AZURE.NOTE] Een RDP-service voor de Firewall is niet nodig om twee redenen; 1) eerste de firewall VM een afbeelding Linux gebaseerd is zodat SSH wordt gebruikt op poort 22 voor VM management in plaats van RDP en 2) poort 22 en twee andere management-poorten zijn toegestaan in de eerste management regel zoals hieronder beschreven staan voor het beheer van connectivity.

### <a name="firewall-rules-creation"></a>Firewall regels maken
Er zijn drie soorten firewallregels in dit voorbeeld gebruikt, worden alle hebt u verschillende pictogrammen:

De regel toepassing omleiden: ![Omleiden toepassingspictogram][7]

De bestemming NAT-regel: ![Bestemming NAT-pictogram][8]

De regel keer: ![Pictogram doorgeven][9]

U vindt meer informatie over deze regels op de website Barracuda.

Als u wilt maken van de volgende regels (of bestaande standaardregels verifiëren), beginnend bij de Barracuda NG client beheerdersdashboard, Ga naar het tabblad configuratie, in de operationele configuratie sectie klikt u op regelset. Een raster genoemd, "Hoofdgegeven regels" wordt de bestaande active en gedeactiveerd regels op deze firewall weergegeven. In de rechterbovenhoek van dit raster is een klein, groen 'en' naar de knop, klik hierop als u wilt een nieuwe regel maken (notitie: uw firewall mogelijk zijn 'vergrendeld' voor wijzigingen, als u de knop gemarkeerd "Vergrendelen" ziet en u niet kunt maken of bewerken van regels, klikt u op deze knop als u wilt de regelset "ontgrendelen" en bewerken toestaan). Als u bewerken van een bestaande regel wilt, selecteert u deze regel, met de rechtermuisknop op en selecteer regel bewerken.

Zodra de regels zijn gemaakt en/of gewijzigd, moet worden verplaatst naar de firewall en wordt geactiveerd, als dit niet is gedaan de wijzigingen van de regel wordt pas van kracht. Het proces push en activering vindt u hieronder de beschrijvingen van de regel voor meer informatie.

De details van elke regel die is vereist voor het voltooien van dit voorbeeld worden als volgt beschreven:

- **Regel voor het beheer van Firewall**: deze App omleiden regel staat verkeer worden doorgegeven aan de poorten beheer van het netwerk virtuele apparaat, in dit voorbeeld een Barracuda NextGen Firewall. De management-poorten zijn 801, 807 (optioneel) en 22. De interne en externe-poorten zijn hetzelfde (dat wil zeggen geen poortvertaling). Deze regel, SETUP-MGMT-ACCESS, is een standaardregel en standaard ingeschakeld (in Barracuda NextGen Firewall versie 6.1).

    ![Regel voor het beheer van Firewall][10]

>[AZURE.TIP] De bron-adresruimte in deze regel is alle, als het beheer van IP-adresbereiken bekend is, deze scope wordt afgetrokken ook sneller aanvallen naar de poorten management.

- **Regels voor RDP**: deze bestemming NAT regels worden beheer van de verschillende servers via RDP toestaan.
Er zijn vier Kritiek (velden) die nodig zijn voor deze regel maakt:
  1.    Bron – RDP zodat vanaf elke locatie, de verwijzing 'Een' wordt gebruikt in het veld bron.
  2.    Service: Gebruik de desbetreffende Service-Object eerder hebt gemaakt, in dit geval 'AppVM01 RDP', de externe poorten doorsturen naar de servers lokaal IP-adres en naar poort 3386 (de RDP standaardpoort). Deze specifieke regel is voor RDP toegang tot AppVM01.
  3.    Doel: moeten de *lokale poort op de firewall*, 'DCHP 1 lokaal IP-' of eth0 als statische IP-adressen gebruikt. De rangtelwoord (eth0, eth1, enzovoort) is mogelijk anders als uw toestel netwerk meerdere lokale interfaces heeft. Dit is de poort de firewall vanuit verstuurt (mogelijk hetzelfde als de ontvangst poort), de bestemming van de werkelijke routering is in het veld doellijst.
  4.    Omleiding – deze sectie wordt aangegeven het virtuele toestel waar dit verkeer uiteindelijk doorsturen. De eenvoudigste omleiding is het IP- en poort (optioneel) te plaatsen in het veld doellijst. Als er geen poort wordt gebruikt de doelpoort op de binnenkomende aanvraag worden gebruikt (ie geen vertaling), als een poort is de poort aangewezen worden ook NAT zou samen met het IP-mailadres.

    ![Firewall RDP regel][11]

    Een totaal van vier RDP regels moet worden gemaakt: 

  	|   De regelnaam van de    | Server  |   Service   |  Doellijst  |
  	|----------------|---------|-------------|---------------|
  	| RDP-naar-IIS01   | IIS01   | IIS01 RDP   | 10.0.1.4:3389 |
  	| RDP-naar-DNS01   | DNS01   | DNS01 RDP   | 10.0.2.4:3389 |
  	| RDP-naar-AppVM01 | AppVM01 | AppVM01 RDP | 10.0.2.5:3389 |
  	| RDP-naar-AppVM02 | AppVM02 | AppVm02 RDP | 10.0.2.6:3389 |
  
>[AZURE.TIP] Verkleinen u het bereik van de bron- en Service-velden worden aanvallen verkleinen. Het meest beperkt bereik waarmee functionaliteit moet worden gebruikt.

- **Regels voor het verkeer van toepassing**: Er zijn twee toepassing verkeer regels voor de eerste voor de front-end van web-verkeer en de tweede voor het back-end-verkeer (bijvoorbeeld webserver naar gegevenslaag). Deze regels worden, hangt af van de netwerkarchitectuur (waar uw servers worden geplaatst) en verkeer loopt (welke richting de verkeersstromen en welke poorten worden gebruikt).

    Eerst worden besproken, is de front-end van-regel voor webverkeer:

    ![Firewall Web regel][12]
 
    Deze regel bestemming NAT kan het verkeer werkelijke toepassing de toepassingsserver te bereiken. Terwijl de andere regels toestaan voor beveiliging, management, enzovoort, worden regels van toepassing zijn wat toestaan van externe gebruikers of services voor toegang tot de toepassingen. In dit voorbeeld is er één webserver op poort 80, dus de regel van de toepassing één firewall binnenkomende verkeer worden omgeleid naar de externe IP-adres, het web servers interne IP-adres.

    **Opmerking**: dat er is geen poort toegewezen in het veld doellijst, dus de binnenkomende poort 80 (of 443 voor de geselecteerde Service) wordt gebruikt in de omleiding van de webserver. Als op een andere poort luistert de webserver, bijvoorbeeld poort 8080, het veld doellijst kan worden bijgewerkt naar 10.0.1.4:8080 toe te staan dat voor de poortomleiding ook.

    De volgende regel van de toepassing-verkeer is de back-end-regel toe te staan dat de webserver contact opnemen met de AppVM01-server (maar niet AppVM02) via een service:

    ![Firewall AppVM01 regel][13]

    Deze regel keer toegestaan voor een IIS-server op het subnet Frontend bereiken van de AppVM01 (IP-adres 10.0.2.5) op een willekeurige poort, met behulp van een Protocol naar access-gegevens die nodig zijn voor de webtoepassing.

    In dit schermopname een '\<expliciete dest\>"wordt gebruikt in het doelveld om aan te duiden 10.0.2.5 als de bestemming. Dit kan zijn een expliciete zoals wordt weergegeven of een benoemd netwerkobject (zoals werden geëffend in de vereisten voor de DNS-server). Dit is snel aan de beheerder van de firewall welke methode wordt gebruikt. Als u wilt toevoegen 10.0.2.5 als een Explict Desitnation, dubbelklikt u op de eerste lege rij onder \<expliciete dest\> en voert u het adres in het venster dat verschijnt.

    Met deze regel doorgeeft, is geen NAT omdat dit interne verkeer, zodat de verbindingsmethode kan worden ingesteld op "Geen SNAT" nodig.

    **Opmerking**: de bron-netwerk in deze regel is een resource in het subnet FrontEnd als er wordt niet meer dan één of een bekende bepaald aantal endwebservers, een resource netwerkobject kan worden gemaakt als u meer informatie over aan de exacte IP-adressen in plaats van het hele Frontend subnet.

>[AZURE.TIP] Deze regel maakt gebruik van de service 'Een' om de toepassing van de steekproef gemakkelijker te installeren en gebruiken, Hierdoor kunnen ook ICMPv4 (ping) in een één regel. Dit is echter niet aanbevolen. De poorten en protocollen ("Services") moeten zijn teruggebracht tot het minimum mogelijke waarmee toepassing bewerking aanvallen over deze grens verkleinen.

<br />

>[AZURE.TIP] Hoewel deze regel ziet u een expliciete dest verwijzing die wordt gebruikt, moet een consistente manier worden gebruikt tijdens de configuratie van de firewall. Het wordt aanbevolen dat het Object met benoemde Network overal in worden gebruikt voor gemakkelijker leesbaarheid en ondersteuningsopties. De expliciete dest hier alleen om weer te geven van een methode alternatieve verwijzing wordt gebruikt en niet het algemeen wordt aanbevolen (met name voor complexe configuraties).

- **Uitgaand op Internet regel**: deze doorgeven regel wordt dat verkeer is toegestaan met een Bronnetwerk om aan de geselecteerde bestemming-netwerken te geven. Deze regel is een standaardregel meestal al op de firewall Barracuda NextGen, maar is uitgeschakeld. Met de rechtermuisknop op deze regel hebben toegang tot de opdracht regel activeren. De regel die u hier kunt zien is welke twee lokale subnetten die zijn gemaakt als verwijzingen in de vereiste sectie van dit document toevoegen aan het bron-kenmerk van deze regel gewijzigd.

    ![Firewall uitgaande regel][14]

- **DNS-regel**: deze doorgeven regel alleen DNS (poort 53) verkeer worden doorgegeven aan de DNS-server is toegestaan. Voor deze omgeving die meeste verkeer van de FrontEnd naar de BackEnd is geblokkeerd, kan deze regel specifiek DNS.

    ![Firewall DNS-regel][15]

    **Opmerking**: In dit scherm opname de verbindingsmethode is opgenomen. Omdat deze regel voor interne IP aan interne IP-adres verkeer is, wordt geen NATing is vereist, wordt dit de verbindingsmethode is ingesteld op "Geen SNAT" voor deze regel keer.

- **Subnet naar Subnet regel**: deze doorgeven regel is een standaardregel dat is geactiveerd en gewijzigd dat een server in de back-end-subnet verbinding maken met een server in de front-end van subnet. Deze regel is alle interne verkeer zodat de verbindingsmethode kan worden ingesteld op Nee SNAT.

    ![Firewall binnen VNet regel][16]

    **Opmerking**: het selectievakje bidirectionele niet is ingeschakeld (en niet is ingecheckt in de meeste regels), dit is van belang zijn voor deze regel in dat deze zorgt ervoor dat deze regel 'één richting', een verbinding kan worden geïnitieerd vanaf het back-end-subnet de front-end van netwerk, maar niet andersom. Als dit selectievakje is ingeschakeld, zou deze regel bidirectionele verkeer, dat uit onze logische diagram niet de keuze inschakelen.

- **Alle verkeer regel weigeren**: dit moet altijd de laatste regel (in prioriteit) en als zodanig als een verkeersstromen niet overeenkomt met de voorafgaand aan de regels van deze door deze regel wordt verwijderd. Dit is een standaardregel en meestal wordt geactiveerd, geen wijzigingen in het algemeen nodig zijn. 

    ![Firewall regel weigeren][17]

>[AZURE.IMPORTANT] Zodra alle bovenstaande regels zijn gemaakt, is het belangrijk om te controleren van de prioriteit van elke regel om ervoor te zorgen verkeer wordt toegestaan of geweigerd naar wens. In dit voorbeeld worden de regels in de volgorde waarin die ze moeten worden weergegeven in het raster Hoofdgegeven van doorstuurregels in de Barracuda Management-Client.

## <a name="rule-activation"></a>Activering van de regel
Met de regelset gewijzigd in de specificatie van het diagram logica, moet de regelset worden geüpload naar de firewall en wordt geactiveerd.

![Firewall regel activering][18]
 
In de rechterbovenhoek van de client management zijn een cluster van knoppen. Klik op de knop 'Wijzigingen verzenden' de gewijzigde regels om naar te verzenden de firewall en klik op de knop 'Activeren'.
 
Met de activering van de firewall-regelset zijn in dit voorbeeld-omgeving opbouwen is voltooid.

## <a name="traffic-scenarios"></a>Verkeer scenario 's
>[AZURE.IMPORTANT] Een belangrijke takeway is te onthouden dat **al** het verkeer via de firewall wordt verzonden. Dus met extern bureaublad naar de server IIS01, hoewel in de Front End-Cloudservice en klik op de front-end van subnet is, toegang tot deze server we RDP moet aan de firewall op poort 8014 en laat de firewall voor het routeren van de aanvraag intern naar de IIS01 RDP-poort. De knop 'Verbinden' van de Azure portal werken niet omdat er geen directe RDP-pad naar IIS01 (als de portal zien kan). Dit houdt in alle verbindingen van internet op de Service beveiliging en een poort, bijvoorbeeld secscv001.cloudapp.net:xxxx.

Voor deze scenario's moet de volgende firewallregels op hun plaats staan:

1.  Firewallbeheer van de
2.  RDP naar IIS01
3.  RDP naar DNS01
4.  RDP naar AppVM01
5.  RDP naar AppVM02
6.  App-verkeer is toegestaan op het Web
7.  App-verkeer naar AppVM01
8.  Uitgaande met Internet
9.  Frontend naar DNS01
10. Binnen Subnet verkeer (back-end naar alleen front-end)
11. Alles weigeren

De regelset werkelijke firewall vele andere regels naast deze waarschijnlijk heeft, moet de regels op een bepaald firewall wordt ook de getallen verschillende prioriteit dan de kleuren die hier wordt vermeld. Deze lijst en de bijbehorende getallen zijn te leveren relevantie te verkrijgen tussen alleen deze 11 regels en de relatieve prioriteit onder deze. Met andere woorden; Klik op de werkelijke firewall kan de "RDP naar IIS01" regel getal 5, maar zo lang maken als het zich onder de regel "Firewall Management" en boven de regel 'RDP-DNS01' deze met de bedoeling deze lijst wilt uitlijnen. De lijst helpt ook de onderstaande scenario's toestaan kort te houden; bijvoorbeeld ' FW regel 9 (DNS) '. Ook kort te houden, de vier RDP-regels worden worden genoemd, "de RDP-regels" wanneer het verkeer scenario is gerelateerd aan RDP.

Ook intrekken dat netwerk beveiligingsgroepen ter plaatse zijn voor binnenkomende internetverkeer op de front-end en back-end-subnetten.

#### <a name="allowed-internet-to-web-server"></a>(Toegestaan) Internet voor de webserver
1.  HTTP-pagina van Internet gebruiker serviceaanvragen van SecSvc001.CloudApp.Net (Internet die tegenover elkaar liggen Cloud Service)
2.  Cloud service stadia verkeer via openen endpoint voor poort 80 aan firewall interface op 10.0.0.4:80
3.  Geen NSG toegewezen aan beveiliging subnet, dat het geval is systeem NSG regels verkeer firewall toestaan
4.  Verkeer raakt interne IP-adres van de firewall (10.0.1.4)
5.  Firewall begint met de verwerking van de regel:
  1.    FW regel 1 (FW Mgmt) niet van toepassing, naar de volgende regel verplaatsen
  2.    FW regels 2 tot en met 5 (RDP regels) niet van toepassing, naar de volgende regel verplaatsen
  3.    FW regel 6 (App: Web) is van toepassing, wordt verkeer is toegestaan, firewall NAT's om te 10.0.1.4 (IIS01)
6.  Het subnet Frontend begint met binnenkomende regel verwerking:
  1.    NSG regel 1 (blok Internet) is niet van toepassing (dit verkeer is NAT zou doen door de firewall, het bronadres is dus nu de firewall op het subnet beveiliging en zichtbaar voor de Frontend subnet NSG moeten "lokaal" verkeer en dus is toegestaan), naar de volgende regel verplaatsen
  2.    Standaard NSG regels dat subnet naar subnet verkeer is toegestaan, wordt verkeer is toegestaan, NSG regel verwerking stoppen
7.  IIS01 luistert naar webverkeer, dit verzoek ontvangt en start de verwerking van de aanvraag
8.  IIS01 pogingen naar een FTP-sessie naar AppVM01 op Backend subnet Start
9.  De route UDR op Frontend subnet zorgt ervoor dat de firewall de volgende hop
10. Geen uitgaande regels op Frontend subnet verkeer is toegestaan
11. Firewall begint met de verwerking van de regel:
  1.    FW regel 1 (FW Mgmt) niet van toepassing, naar de volgende regel verplaatsen
  2.    FW-regel 2 tot en met 5 (RDP regels) niet van toepassing, naar de volgende regel verplaatsen
  3.    FW regel 6 (App: Web) niet van toepassing, verplaatsen naar de volgende regel
  4.    FW regel 7 (App: Backend) is van toepassing, wordt verkeer is toegestaan, firewall stuurt verkeer naar 10.0.2.5 (AppVM01)
12. De Backend-subnet begint met binnenkomende regel verwerking:
  1.    NSG regel 1 (blok Internet) niet van toepassing, naar de volgende regel verplaatsen
  2.    Standaard NSG regels dat subnet naar subnet verkeer is toegestaan, wordt verkeer is toegestaan, NSG regel verwerking stoppen
13.  AppVM01 ontvangt de aanvraag en de sessie start en reageert
14. De route UDR op Backend subnet zorgt ervoor dat de firewall de volgende hop
15. Aangezien er geen uitgaande NSG regels op de Backend-subnet het antwoord is toegestaan
16. Omdat dit verkeer op een bestaande sessie retourneert stuurt de firewall het antwoord terug naar de webserver (IIS01)
17. Frontend subnet begint met binnenkomende regel verwerking:
  1.    NSG regel 1 (blok Internet) niet van toepassing, naar de volgende regel verplaatsen
  2.    Standaard NSG regels dat subnet naar subnet verkeer is toegestaan, wordt verkeer is toegestaan, NSG regel verwerking stoppen
18. De IIS-server de reactie ontvangt, de transactie met AppVM01 en voltooit het HTTP-antwoord bouwen, wordt dit HTTP-antwoord verzonden naar degene
19. Aangezien er geen uitgaande NSG regels op de Frontend subnet het antwoord is toegestaan
20. Het HTTP-antwoord raakt de firewall en omdat dit het antwoord op een bestaande NAT-sessie is geaccepteerd door de firewall
21. De firewall vervolgens stuurt de reactie terug naar de gebruiker Internet
22. Aangezien er geen uitgaande ontvangt NSG regels of UDR hops in het antwoord is toegestaan Frontend subnet en de gebruiker Internet u de pagina met webonderdelen aangevraagd.

#### <a name="allowed-internet-rdp-to-backend"></a>(Toegestaan) Internet RDP naar Backend
1.  De beheerder van de server op internet aanvraagt RDP-sessie met AppVM01 via SecSvc001.CloudApp.Net:8025, waar 8025 het poortnummer gebruiker toegewezen voor de regel van de firewall "RDP-AppVM01 is"
2.  De cloudservice geeft het verkeer via de openen endpoint op poort 8025 aan firewall interface op 10.0.0.4:8025
3.  Geen NSG toegewezen aan beveiliging subnet, dat het geval is systeem NSG regels verkeer firewall toestaan
4.  Firewall begint met de verwerking van de regel:
  1.    FW regel 1 (FW Mgmt) niet van toepassing, naar de volgende regel verplaatsen
  2.    FW regel 2 (RDP IIS) niet van toepassing, naar de volgende regel verplaatsen
  3.    FW regel 3 (RDP DNS01) niet toepassen, naar de volgende regel verplaatsen
  4.    FW regel 4 (RDP AppVM01) is van toepassing, wordt verkeer is toegestaan, firewall NAT's om te 10.0.2.5:3386 (RDP poort op AppVM01)
5.  De Backend-subnet begint met binnenkomende regel verwerking:
  1.    NSG regel 1 (blok Internet) is niet van toepassing (dit verkeer is NAT zou doen door de firewall, het bronadres is dus nu de firewall op het subnet beveiliging en zichtbaar voor de Backend-subnet NSG moeten "lokaal" verkeer en dus is toegestaan), naar de volgende regel verplaatsen
  2.    Standaard NSG regels dat subnet naar subnet verkeer is toegestaan, wordt verkeer is toegestaan, NSG regel verwerking stoppen
6.  AppVM01 luistert voor RDP-verkeer en reageert
7.  Met geen uitgaande NSG regels standaardregels toepassen en afzender verkeer is toegestaan
8.  UDR routes uitgaand verkeer naar de firewall als de volgende hop
9.  Omdat dit verkeer op een bestaande sessie retourneert stuurt de firewall het antwoord terug naar de gebruiker internet
10. RDP-sessie is ingeschakeld
11. AppVM01 gebruikersnaam wachtwoord gevraagd

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Toegestaan) Web Server DNS opzoeken op DNS-server
1.  Web Server, IIS01, een gegevensfeed bij www.data.gov behoeften, maar behoeften voor het oplossen van het adres.
2.  De netwerkconfiguratie voor de lijsten VNet DNS01 (10.0.2.4 op de Backend-subnet) als de primaire DNS-server, in IIS01 wordt de DNS-aanvraag verzonden naar DNS01
3.  UDR routes uitgaand verkeer naar de firewall als de volgende hop
4.  Geen uitgaande NSG regels zijn afhankelijk van het Frontend subnet, is verkeer toegestaan
5.  Firewall begint met de verwerking van de regel:
  1.    FW regel 1 (FW Mgmt) niet van toepassing, naar de volgende regel verplaatsen
  2.    FW-regel 2 tot en met 5 (RDP regels) niet van toepassing, naar de volgende regel verplaatsen
  3.    FW regels 6 en 7 (App regels) niet van toepassing, naar de volgende regel verplaatsen
  4.    FW regel 8 (naar Internet) niet van toepassing, naar de volgende regel verplaatsen
  5.    FW regel 9 (DNS) is van toepassing, wordt verkeer is toegestaan, firewall stuurt verkeer naar 10.0.2.4 (DNS01)
6.  De Backend-subnet begint met binnenkomende regel verwerking:
  1.    NSG regel 1 (blok Internet) niet van toepassing, naar de volgende regel verplaatsen
  2.    Standaard NSG regels dat subnet naar subnet verkeer is toegestaan, wordt verkeer is toegestaan, NSG regel verwerking stoppen
7.  DNS-server ontvangt de aanvraag
8.  DNS-server geen het adres in cache opgeslagen en wordt gevraagd om een hoofdsite DNS-server op internet
9.  UDR routes uitgaand verkeer naar de firewall als de volgende hop
10. Geen uitgaande NSG regels op Backend subnet verkeer is toegestaan
11. Firewall begint met de verwerking van de regel:
  1.    FW regel 1 (FW Mgmt) niet van toepassing, naar de volgende regel verplaatsen
  2.    FW-regel 2 tot en met 5 (RDP regels) niet van toepassing, naar de volgende regel verplaatsen
  3.    FW regels 6 en 7 (App regels) niet van toepassing, naar de volgende regel verplaatsen
  4.     FW regel 8 (naar Internet) is van toepassing, wordt verkeer is toegestaan, sessie is SNAT af aan hoofdmap DNS-server op Internet
12. Internet-DNS-server reageert, omdat deze sessie is gestart vanuit de firewall, het antwoord wordt geaccepteerd door de firewall
13. Als dit een bestaande sessie is, het antwoord op de oorspronkelijke server, DNS01 worden doorgestuurd door de firewall
14. De Backend-subnet begint met binnenkomende regel verwerking:
  1.    NSG regel 1 (blok Internet) niet van toepassing, naar de volgende regel verplaatsen
  2.    Standaard NSG regels dat subnet naar subnet verkeer is toegestaan, wordt verkeer is toegestaan, NSG regel verwerking stoppen
15. De DNS-server ontvangt en slaat het antwoord en klik vervolgens op de eerste aanvraag terug naar IIS01 reageert
16. De route UDR op backend subnet zorgt ervoor dat de firewall de volgende hop
17. Er bestaat geen uitgaande NSG regels op de Backend-subnet, is verkeer toegestaan
18. Dit is een bestaande sessie op de firewall, het antwoord wordt doorgestuurd door de firewall terug naar de IIS-server
19. Frontend subnet begint met binnenkomende regel verwerking:
  1.    Er is geen NSG-regel die van toepassing op inkomend verkeer van de Backend-subnet naar het Frontend subnet, zodat geen van de NSG regels toepassen
  2.    De standaard systeem regel het verkeer tussen subnetten zou dat deze verkeer is toegestaan, zodat het verkeer is toegestaan
20. IIS01 krijgt de reactie van DNS01

#### <a name="allowed-backend-server-to-frontend-server"></a>(Toegestaan) Back-end-server naar Frontend server
1.  Een beheerder aangemeld bij AppVM02 via RDP aanvraagt een bestand rechtstreeks vanuit de server IIS01 via windows Verkenner
2.  De route UDR op Backend subnet zorgt ervoor dat de firewall de volgende hop
3.  Aangezien er geen uitgaande NSG regels op de Backend-subnet het antwoord is toegestaan
4.  Firewall begint met de verwerking van de regel:
  1.    FW regel 1 (FW Mgmt) niet van toepassing, naar de volgende regel verplaatsen
  2.    FW-regel 2 tot en met 5 (RDP regels) niet van toepassing, naar de volgende regel verplaatsen
  3.    FW regels 6 en 7 (App regels) niet van toepassing, naar de volgende regel verplaatsen
  4.    FW regel 8 (naar Internet) niet van toepassing, naar de volgende regel verplaatsen
  5.    FW regel 9 (DNS) niet van toepassing, verplaatsen naar de volgende regel
  6.    FW regel-10 (binnen-Subnet) is van toepassing, wordt verkeer is toegestaan, firewall verkeer worden doorgegeven aan 10.0.1.4 (IIS01)
5.  Frontend subnet begint met binnenkomende regel verwerking:
  1.    NSG regel 1 (blok Internet) niet van toepassing, naar de volgende regel verplaatsen
  2.    Standaard NSG regels dat subnet naar subnet verkeer is toegestaan, wordt verkeer is toegestaan, NSG regel verwerking stoppen
6.  Als er BEGINLETTERS verificatie en autorisatie en IIS01 de aanvraag accepteert en reageert
7.  De route UDR op Frontend subnet zorgt ervoor dat de firewall de volgende hop
8.  Aangezien er geen uitgaande NSG regels op de Frontend subnet het antwoord is toegestaan
9.  Als dit een bestaande sessie op de firewall is dit antwoord is toegestaan en de firewall geeft als resultaat het antwoord op AppVM02
10. Back-end subnet begint met binnenkomende regel verwerking:
  1.    NSG regel 1 (blok Internet) niet van toepassing, naar de volgende regel verplaatsen
  2.    Standaard NSG regels dat subnet naar subnet verkeer is toegestaan, wordt verkeer is toegestaan, NSG regel verwerking stoppen
11. AppVM02 ontvangt over het antwoord

#### <a name="denied-internet-direct-to-web-server"></a>(Geweigerd) Internet rechtstreeks naar de webserver
1.  Internet-gebruiker probeert toegang tot de webserver, IIS01, via de FrontEnd001.CloudApp.Net-service
2.  Aangezien er geen eindpunten openen voor HTTP-verkeer, Hiermee worden niet via de Cloudservice en de server wouldn't bereiken
3.  Als de eindpunten geopend om welke reden waren, wilt de NSG (Internet blok) op het subnet Frontend dit verkeer blokkeren
4.  Ten slotte de route van Frontend subnet UDR zou elk uitgaand verkeer verzenden vanuit IIS01 naar de firewall als de volgende hop en de firewall zou dit zien als asymmetrische verkeer en neerzetten van het uitgaande antwoord dus er minimaal drie onafhankelijke lagen beveiliging tussen de internet- en IIS01 via de cloudservice onbevoegde/ongeoorloofde toegang voorkomen.

#### <a name="denied-internet-to-backend-server"></a>(Geweigerd) Internet naar endserver
1.  Internet-gebruiker probeert te openen van een bestand op AppVM01 via de BackEnd001.CloudApp.Net-service
2.  Aangezien er geen eindpunten geopend om bestandsshare, dit zou de Cloud-Service niet doorgeven en de server wouldn't bereiken
3.  Als de eindpunten geopend om welke reden waren, wilt de NSG (Internet blok) dit verkeer blokkeren
4.  Ten slotte de route UDR zou elk uitgaand verkeer verzenden vanuit AppVM01 naar de firewall als de volgende hop en de firewall zou dit zien als asymmetrische verkeer en neerzetten van het uitgaande antwoord dus er minimaal drie onafhankelijke lagen beveiliging tussen de internet- en AppVM01 via de cloudservice onbevoegde/ongeoorloofde toegang voorkomen.

#### <a name="denied-frontend-server-to-backend-server"></a>(Geweigerd) Frontend server naar Backend-Server
1.  Stel IIS01 is is gehackt en schadelijke code probeert te scannen de Backend subnet-servers wordt uitgevoerd.
2.  De route van Frontend subnet UDR zou elk uitgaand verkeer van IIS01 naar verzenden de firewall als de volgende hop. Dit is niet iets dat kan worden gewijzigd door de beschadigde VM.
3.  De firewall wilt verwerken het verkeer, als de aanvraag is AppVM01 of de DNS-server voor DNS-lookups dat verkeer mogelijk kan worden toegestaan door de firewall (vanwege FW regels 7 en 9). Alle andere verkeer zou worden geblokkeerd door FW regel 11 (allen weigeren).
4.  Als u geavanceerde bedreiging detectie is ingeschakeld voor de firewall (Zie de documentatie van de leverancier voor uw specifieke netwerk toestel geavanceerde mogelijkheden bedreiging, die valt buiten in dit document), even verkeer die door de eenvoudige doorstuurregels in dit document besproken mogen kan worden voorkomen als het verkeer bekend bekende handtekeningen of patronen die een regel voor geavanceerde bedreiging markeert.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Geweigerd) DNS-Internet opzoeken op DNS-server
1.  Internet-gebruiker probeert aan te melden voor het opzoeken van een interne DNS-record op DNS01 via BackEnd001.CloudApp.Net-service 
2.  Aangezien er geen eindpunten geopend om DNS-verkeer, Hiermee worden niet via de Cloudservice en de server wouldn't bereiken
3.  Als de eindpunten geopend om welke reden waren, wordt de NSG-regel (Internet blok) op het subnet Frontend dit verkeer geblokkeerd
4.  Ten slotte de Backend subnet UDR route zou elk uitgaand verkeer verzenden vanuit DNS01 naar de firewall als de volgende hop en de firewall zou dit zien als asymmetrische verkeer en neerzetten van het uitgaande antwoord dus er minimaal drie onafhankelijke lagen beveiliging tussen de internet- en DNS01 via de cloudservice onbevoegde/ongeoorloofde toegang voorkomen.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Geweigerd) Internet op SQL-toegang via Firewall
1.  Internet-gebruiker aanvraagt SQL-gegevens uit SecSvc001.CloudApp.Net (Internet die tegenover elkaar liggen Cloud Service)
2.  Aangezien er geen eindpunten openen voor SQL, dit zou de Cloud-Service niet doorgeven en de firewall wouldn't bereikt
3.  Als SQL-eindpunten geopend om welke reden waren, zou de firewall regel verwerking eerst:
  1.    FW regel 1 (FW Mgmt) niet van toepassing, naar de volgende regel verplaatsen
  2.    FW regels 2 tot en met 5 (RDP regels) niet van toepassing, naar de volgende regel verplaatsen
  3.    FW regel 6 en 7 (regels van toepassing) niet van toepassing, naar de volgende regel verplaatsen
  4.    FW regel 8 (naar Internet) niet van toepassing, naar de volgende regel verplaatsen
  5.    FW regel 9 (DNS) niet van toepassing, verplaatsen naar de volgende regel
  6.    FW regel-10 (binnen-Subnet) niet van toepassing, naar de volgende regel verplaatsen
  7.    FW regel 11 (allen weigeren) is van toepassing, wordt verkeer is geblokkeerd, stoppen, regel verwerking


## <a name="references"></a>Verwijzingen
### <a name="main-script-and-network-config"></a>Belangrijkste Script en netwerkconfiguratie
Sla het volledige Script in een PowerShell-script-bestand. De netwerkconfiguratie opslaan in een bestand met de naam 'NetworkConf2.xml'.
Wijzig de variabelen door de gebruiker gedefinieerde desgewenst. Het script uitvoeren en voert u de bovenstaande-Firewall regel setup instructieset.

#### <a name="full-script"></a>Volledige Script
Dit script wordt op basis van de gebruiker gedefinieerde variabelen:

1.  Verbinding maken met een Azure-abonnement
2.  Maak een nieuwe opslag-account
3.  Een nieuwe VNet en drie subnetten zoals gedefinieerd in het netwerk configuratiebestand maken
4.  Maken van vijf virtuele machines; 1 firewall en 4 windows server VMs
5.  Configureer UDR waaronder:
  1.    Twee nieuwe routetabellen maken
  2.    Routes toevoegen aan de tabellen
  3.    Tabellen koppelen aan de juiste subnetten
6.  IP-doorsturen op de NVA inschakelen
7.  Configureer NSG waaronder:
  1.    Een NSG maken
  2.    Een regel toevoegen
  3.    De NSG binden naar de juiste subnetten

Deze PowerShell-script moet lokaal op worden uitgevoerd dat een internet verbonden PC of server.

>[AZURE.IMPORTANT] Wanneer dit script wordt uitgevoerd, zijn er mogelijk waarschuwingen of andere informatieve berichten die pop-in PowerShell. Alleen foutberichten worden weergegeven in het rood zijn oorzaak van belang.

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwading from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA
    
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.
    
      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4
     
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4
     
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
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 4 - The DNS Server
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
    
    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}
    
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
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }
    
    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan
    
      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"
    
      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal
    
      # Assoicate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName
    
     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
    
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the Frontend and Backback subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
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
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
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
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Bidirectionele DMZ met NVA, NSG en UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Logische weergave van de firewallregels"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Een Object FrontEnd netwerk maken"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Een DNS-Server-Object maken"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Kopie van de standaard RDP regel"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 regel"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Toepassingspictogram omleiden"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Bestemming NAT-pictogram"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Pictogram doorgeven"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Regel voor het beheer van Firewall"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "Firewall RDP regel"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Firewall Web regel"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Firewall AppVM01 regel"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Firewall uitgaande regel"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "Firewall DNS-regel"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Firewall binnen VNet regel"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Firewall regel weigeren"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Firewall regel activering"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
