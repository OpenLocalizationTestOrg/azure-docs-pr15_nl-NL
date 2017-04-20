<properties
   pageTitle="Azure architectuur referentie - IaaS: Uitvoering van een DMZ tussen Azure en Internet | Microsoft Azure"
   description="Het implementeren van een beveiligde hybride netwerkarchitectuur met internettoegang in Azure wordt aangegeven."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
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

# <a name="implementing-a-dmz-between-azure-and-the-internet"></a>Een DMZ tussen Azure en Internet implementeren

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In dit artikel wordt beschreven aanbevolen procedures voor de uitvoering van een beveiligde hybride netwerk die uw on-premises netwerk waarin verkeer van het netwerk internet naar Azure uitbreidt. Deze verwijzing architectuur breidt de architectuur beschreven in het artikel [een DMZ tussen Azure en uw on-premises implementatie-datacenter implementeren][implementing-a-secure-hybrid-network-architecture]. Het wordt aanbevolen u dat document lezen en begrijpen die verwijzing architectuur voordat het lezen van dit document.

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: [Resourcemanager] [ resource-manager-overview] en klassieke. Deze verwijzing architectuur gebruikt resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties. 

Veelvoorkomend gebruik dozen voor deze architectuur opnemen:

- Hybride toepassingen waar werkbelasting uitvoeren gedeeltelijk on-premises implementatie en deels in Azure.

- Azure-infrastructuur waarmee binnenkomende verkeer van on-premises en internet.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram wordt de belangrijke onderdelen in deze architectuur gemarkeerd:

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft Downloadcentrum][visio-download]. Dit diagram is op de pagina "DMZ--openbare".

[! [0]][0]

- **Openbare IP-adres (PIP).** Dit is het IP-adres van de openbare eindpunt. Externe gebruikers die zijn verbonden met Internet kunnen u het systeem openen via dit adres.

- **Virtuele toestel netwerk (NVA).**  NVA is een algemene term waarmee een VM uitvoeren van taken zoals toestaan of weigeren van access als een firewall, optimaliseren WAN-bewerkingen (met inbegrip van Netwerkcompressie), aangepaste routering of andere netwerkfunctionaliteit wordt beschreven.

- **Azure taakverdeling.** Alle verzoeken voor oproepen van internet via deze taakverdeling en zijn verdeeld over NVAs in de openbare DMZ inkomende subnet.

- **Openbare DMZ inkomende subnet.** Dit subnet accepteert aanmeldingsaanvragen van de Azure taakverdeling. Verzoeken voor oproepen worden doorgegeven aan een van de NVAs in de DMZ.

- **Openbare DMZ uitgaande subnet.** Dit subnet doorgegeven aanvragen die worden goedgekeurd door de NVA aan de verdeling van de interne belasting voor de weblaag.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en resourcetypen, zodat deze architectuur verwijzing kan worden deze is ingericht veel verschillende manieren. We hebben een resourcemanager Azure-sjabloon om te kunnen installeren van de architectuur van de verwijzing die deze aanbevelingen volgt opgegeven. Als u wilt maken van de architectuur van uw eigen verwijzing moet u deze aanbevelingen volgt, tenzij u specifieke vereisten die een aanbeveling niet past hebt.

### <a name="nva-recommendations"></a>NVA aanbevelingen

Implementeer één set NVAs voor verkeer die afkomstig zijn op internet en een andere voor verkeer die afkomstig zijn on-premises implementatie. Het is een beveiligingsrisico voor slechts één set NVAs gebruiken voor beide omdat dit ontwerp geen omtrek beveiliging tussen de twee paren van netwerkverkeer biedt. Dit is een voordeel voor het gebruik van deze ontwerp, omdat deze de complexiteit van beveiliging controleregels reduceert en maakt het duidelijkere welke regels komen overeen met elk binnenkomende verzoek om een netwerk. Een reeks NVAs implementeert bijvoorbeeld regels voor het internetverkeer alleen terwijl een andere set NVAs implementeren regels voor alleen on-premises implementatie-verkeer is toegestaan.

Een laag 7 opnemen NVA toepassing verbindingen op het niveau van de NVA beëindigen en onderhouden van affiniteit bij de backend-lagen. Dit zorgt ervoor symmetric connectivity waarin antwoord verkeer van de backend-lagen tot en met de NVA als resultaat.  

### <a name="public-load-balancer-recommendations"></a>Openbare laden de verdeling van aanbevelingen ###

Als u wilt behouden schaalbaarheid en beschikbaarheid, implementeren de openbare DMZ inkomende NVAs in een [beschikbaarheid instellen] [ availability-set] en gebruikt u een [internet die tegenover elkaar liggen taakverdeling] [ load-balancer] internetaanvragen over de NVAs in de set beschikbaarheid verdelen.  

Configureer de verdeling van de belasting om aanvragen alleen op de poorten die nodig zijn voor internetverkeer te accepteren. Binnenkomende HTTP-verzoeken voor poort 80 en binnenkomende HTTPS aanvragen aan poort 443 bijvoorbeeld beperken.

## <a name="scalability-considerations"></a>Aandachtspunten voor de schaalbaarheid

De infrastructuur van uw presentaties ontwerpen met een internetverbinding van taakverdeling vóór het binnenkomende openbare DMZ subnet vanaf het begin. Zelfs als uw initally architectuur vereist is voor een enkele NVA, zijn deze gemakkelijker aan de nieuwe schaal naar meerdere NVAs in de toekomst als internet die tegenover elkaar liggen taakverdeling al wordt geïmplementeerd.

## <a name="availability-considerations"></a>Aandachtspunten voor de beschikbaarheid

Internet die tegenover elkaar liggen taakverdeling is vereist voor elke NVA in de openbare DMZ inkomende subnet willen implementeren van een [test voor status][lb-probe]. Een test voor status die niet meer reageert op dit eindpunt wordt beschouwd als niet beschikbaar is en de taakverdeling leidt aanvragen voor andere NVAs in bepaalde beschikbaarheid. Houd er rekening mee dat als alle NVAs niet reageert, uw toepassing mislukt, zodat het is belangrijk dat u hebt geconfigureerd voor waarschuwingen DevOps wanneer het aantal exemplaren van orde NVA kleiner is dan een drempelwaarde voor controle.

## <a name="manageability-considerations"></a>Aandachtspunten voor de beheerbaarheid

De functionaliteit voor controle en beheer voor de binnenkomende openbare DMZ NVA van reageren op vergaderverzoeken in het vak sprong in het management subnet alleen beperken. Zoals is beschreven in de [uitvoering van een DMZ tussen Azure en uw on-premises implementatie-datacenter] [ implementing-a-secure-hybrid-network-architecture] document, een route één netwerk van de on-premises netwerk via de gateway naar het vak sprong definiëren in het subnet management beperken van toegang.

Als gateway-connectiviteit uit uw on-premises netwerk met Azure niet actief is, kunt u het vak sprong nog steeds bereiken door te implementeren van een PIP, toevoegen aan het vak van de lijnsprong en externe in van internet.

## <a name="security-considerations"></a>Veiligheidsoverwegingen

Deze verwijzing architectuur implementeert meerdere niveaus van beveiliging:

- Internet die tegenover elkaar liggen taakverdeling wordt u omgeleid zodat aanvragen voor het NVAs in de binnenkomende openbare DMZ alleen subnet en alleen op de poorten die nodig zijn voor de toepassing.

- De NSG regels voor het binnenkomende en uitgaande openbare DMZ-subnet wordt voorkomen dat de NVAs misbruik aanvragen die buiten de regels NSG valt blokkeren.

- De configuratie van NAT routering voor de NVAs wordt u omgeleid zodat verzoeken voor oproepen op poort 80 en poort 443 naar de verdeling van de web laag belasting, maar aanvragen voor alle andere poorten, worden genegeerd.

Houd er rekening mee dat u alle verzoeken voor oproepen op alle poorten zich moet aanmelden. Regelmatige controle van de logboeken, aandacht op aanvragen die buiten de verwachte parameters valt terwijl deze indringers pogingen kunnen duiden.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Gebruik van NSGs om te blokkeren/keer verkeer tussen lagen

Elk van de lagen web en business gegevens beperken verkeer ertussen NSGs gebruiken. Dat wil zeggen de laag bedrijven gebruikt een NSG blokkeren van al het verkeer die niet afkomstig uit de weblaag zijn en de gegevenslaag een NSG gebruikt voor het blokkeren van al het verkeer die niet afkomstig uit de laag bedrijven zijn. Als er een vereiste dat de regels NSG bredere toegang tot deze niveaus uitvouwen, wegen deze vereisten is voldaan ten opzichte van de beveiligingsrisico's. Elke nieuwe binnenkomende pad staat voor een verkoopkans per ongeluk of purposeful gegevens zijn lekken of toepassing beschadigd.

## <a name="solution-deployment"></a>Implementatie van oplossingen

Een implementatie van een verwijzing architectuur dat deze aanbevelingen implementeert is beschikbaar op Github. De architectuur van deze verwijzing bevat een virtueel netwerk (VNet), netwerk-beveiligingsgroep (NSG), taakverdeling en twee virtuele machines (VMs).

De architectuur verwijzing kan worden geïmplementeerd met Windows- of Linux VMs aan de hand van de volgende instructies uit: 

1. Klik met de rechtermuisknop op de knop onderstaande en selecteer een van beide 'koppeling openen in nieuw tabblad"of 'Koppeling openen in nieuw venster':  
[![Implementeren naar Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2FvirtualNetwork.azuredeploy.json)

2. Nadat de koppeling is geopend in de portal van Azure, moet u waarden opgeven voor een deel van de instellingen: 
    - De naam van de **resourcegroep** is al gedefinieerd in het parameterbestand, dus **Nieuw** te selecteren en voer `ra-public-dmz-network-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
    - Selecteer het **Type besturingssysteem** uit de vervolgkeuzelijst vak, **windows** of **linux**.
    - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
    - Klik op de knop **aanschaffen** .

3. Wacht totdat de implementatie om te voltooien.

4. Klik met de rechtermuisknop op de knop onderstaande en selecteer een van beide 'koppeling openen in nieuw tabblad"of 'Koppeling openen in nieuw venster':  
[![Implementeren naar Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fworkload.azuredeploy.json)

5. Nadat de koppeling is geopend in de portal van Azure, moet u waarden opgeven voor een deel van de instellingen: 
    - De naam van de **resourcegroep** is al gedefinieerd in het parameterbestand, dus **Nieuw** te selecteren en voer `ra-public-dmz-wl-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
    - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
    - Klik op de knop **aanschaffen** .

6. Wacht totdat de implementatie om te voltooien.

7. Klik met de rechtermuisknop op de knop onderstaande en selecteer een van beide 'koppeling openen in nieuw tabblad"of 'Koppeling openen in nieuw venster':  
[![Implementeren naar Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fsecurity.azuredeploy.json)

8. Nadat de koppeling is geopend in de portal van Azure, moet u waarden opgeven voor een deel van de instellingen: 
    - De naam van de **resourcegroep** is al gedefinieerd in het parameterbestand, dus selecteer **Bestaande gebruiken** en voer `ra-public-dmz-network-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
    - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
    - Klik op de knop **aanschaffen** .

9. Wacht totdat de implementatie om te voltooien.

10. De parameterbestanden zijn hard gecodeerde beheerder-gebruikersnaam en wachtwoord voor alle VMs en het wordt ten zeerste aanbevolen dat u direct beide wijzigen. Voor elke VM in de implementatie, selecteert u deze in de portal van Azure en klik vervolgens op in het **wachtwoord opnieuw** in het blad **ondersteuning + probleemoplossing** . Selecteer **wachtwoord opnieuw** in het vak van de vervolgkeuzelijst **modus** en selecteer vervolgens een nieuwe **gebruikersnaam** en **wachtwoord**. Klik op de knop **bijwerken** om te voorkomen.


<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[iptables]: https://help.ubuntu.com/community/IptablesHowTo
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md
[load-balancer]: ../load-balancer/load-balancer-internet-overview.md
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[0]: ./media/blueprints/hybrid-network-secure-vnet-dmz.png "Hybride netwerkarchitectuur Secure"
