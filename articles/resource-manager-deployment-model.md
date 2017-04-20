<properties
   pageTitle="Resourcemanager en klassieke implementatie | Microsoft Azure"
   description="Beschrijving van de verschillen tussen het implementatiemodel resourcemanager-en het klassieke (of servicebeheer) implementatiemodel."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-the-state-of-your-resources"></a>Azure resourcemanager versus klassieke implementatie: meer informatie over de implementatiemodellen en de status van uw resources

In dit onderwerp u meer informatie over Azure resourcemanager en klassieke implementatiemodellen, de status van uw bronnen, en waarom uw resources zijn geïmplementeerd met een of de andere. De resourcemanager en klassieke implementatiemodellen vertegenwoordigen twee verschillende manieren implementatie en beheren van uw Azure oplossingen. U ermee werken tot en met twee verschillende API-groepen en de gedistribueerde bronnen belangrijke verschillen kunnen bevatten. De twee modellen niet volledig compatibel zijn met elkaar. In dit onderwerp worden deze verschillen.

Om te vereenvoudigen de implementatie en het beheer van resources, wordt aangeraden dat u Resource Manager voor alle nieuwe resources gebruiken. Microsoft raadt indien mogelijk dat u bestaande resources tot en met resourcemanager Implementeer deze opnieuw.

Als u nog niet eerder naar resourcemanager, wilt u mogelijk moet u rekening houden de terminologie die is gedefinieerd in het [overzicht van de Azure resourcemanager](azure-resource-manager/resource-group-overview.md).

## <a name="history-of-the-deployment-models"></a>Geschiedenis van de implementatiemodellen

Azure aangeboden oorspronkelijk alleen het implementatiemodel klassieke. In dit model bestaat elke resource onafhankelijk; Er is geen manier om verwante resources groeperen. In plaats daarvan moest u handmatig bijhouden welke resources uit de volgende uw oplossing of een toepassing, en vergeet niet dat ze kunt beheren in een gecoördineerde benadering. Als u wilt een oplossing implementeert, moest u elke resource afzonderlijk via de klassieke beheerportal maken of een script maken waarmee alle resources in de juiste volgorde geïmplementeerd. Als u wilt verwijderen van een oplossing, moest u elke resource afzonderlijk verwijderen. U kunt niet gemakkelijk toepassen en clienttoegangsbeleid besturingselement voor verwante resources bijwerken. Tot slot u niet kunt toepassen labels naar bronnen label met voorwaarden die u helpen uw resources controleren en facturering kunt beheren.

Azure geïntroduceerd in 2014 resourcemanager, die het concept van een resourcegroep toegevoegd. Een resourcegroep is een container voor resources die een gemeenschappelijke levenscyclus delen. Het implementatiemodel resourcemanager biedt diverse voordelen:

- U kunt implementeren, beheren en alle services voor uw oplossing als een groep controleren, in plaats van afzonderlijk afhandelen van deze services.
- U kunt net zo vaak de overal in de levenscyclus van de-oplossing implementeert en betrouwbaarheid die uw resources zijn geïmplementeerd in een consistente status.
- U kunt toegangsbeheer toepassen op alle resources in de resourcegroep en beleid automatisch worden toegepast wanneer nieuwe resources worden toegevoegd aan de resourcegroep.
- U kunt labels toepassen op resources logisch ordenen alle resources in uw abonnement.
- U kunt de notatie voor JavaScript-Object (JSON) definiëren de infrastructuur voor uw oplossing. De JSON-bestand is bekend als een sjabloon resourcemanager.
- U kunt de afhankelijkheden tussen resources zodat ze zijn geïmplementeerd in de juiste volgorde definiëren.

Wanneer resourcemanager is toegevoegd, worden alle resources zijn met terugwerkende kracht toegevoegd aan standaardgroepen resource. Als u een resource tot en met klassieke implementatie nu maakt, wordt automatisch de resource gemaakt binnen een standaardgroep voor de resource voor deze service, hoewel u die resourcegroep implementatie niet hebt opgegeven. Echter betekent alleen bestaande binnen een resourcegroep niet dat de resource is geconverteerd naar het model resourcemanager. Bespreken we de verwerking van elke service op de twee implementatiemodellen in het volgende gedeelte. 

## <a name="understand-support-for-the-models"></a>Meer informatie over ondersteuning voor de modellen 

Bij het kiezen welke implementatiemodel wilt gebruiken voor uw resources, zijn er zijn drie scenario's letten:

1. De service ondersteunt resourcemanager en biedt van slechts één type.
2. De service resourcemanager ondersteund, maar bevat twee typen - een voor resourcemanager en een voor classic. In dit scenario geldt alleen voor virtuele machines, opslag-accounts en virtuele netwerken.
3. De service biedt geen ondersteuning voor resourcemanager.

Als u wilt ontdekken of een service resourcemanager ondersteunt, raadpleegt u [resourcemanager providers ondersteund](resource-manager-supported-services.md).

Als de service die u wilt gebruiken geen ondersteuning biedt voor resourcemanager, moet u blijven klassieke implementatie gebruiken.

Als de service resourcemanager en **niet** een virtuele machine, opslag-account of virtuele netwerk ondersteunt, kunt u resourcemanager zonder eventuele problemen.

Voor virtuele machines, opslag-accounts en virtuele netwerken, als de resource is gemaakt via klassieke implementatie, moet u blijven tot en met klassieke bewerkingen worden uitgevoerd. Als de virtuele machine, opslag-account of virtuele netwerk is gemaakt via resourcemanager implementatie, moet u blijven resourcemanager bewerkingen. Dit onderscheid kunt krijgen duidelijker is als uw abonnement een combinatie van resources die zijn gemaakt via resourcemanager en klassieke implementatie bevat. Deze combinatie van resources kan onverwachte resultaten maken, omdat de bronnen bewerkingen niet ondersteunen.

In sommige gevallen, een opdracht resourcemanager informatie over een resource die is gemaakt via klassieke implementatie kunt ophalen, of een administratieve taken zoals het verplaatsen van een klassieke resource naar een andere resourcegroep kunt uitvoeren. Maar deze gevallen moeten de indruk dat het type resourcemanager bewerkingen ondersteunt niet geven. Stel dat u hebt een resourcegroep die een virtuele machine die is gemaakt met klassieke implementatie bevat. Als u de volgende resourcemanager PowerShell-opdracht uitvoeren:

    Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines

De virtuele machine geretourneerd:
    
    Name              : ExampleClassicVM
    ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
    ResourceName      : ExampleClassicVM
    ResourceType      : Microsoft.ClassicCompute/virtualMachines
    ResourceGroupName : ExampleGroup
    Location          : westus
    SubscriptionId    : {guid}

De resourcemanager cmdlet **Get-AzureRmVM** retourneert echter alleen virtuele machines tot en met resourcemanager geïmplementeerd. De volgende opdracht resulteert niet in de virtuele machine gemaakt via klassieke implementatie.

    Get-AzureRmVM -ResourceGroupName ExampleGroup

Alleen resources die zijn gemaakt door resourcemanager ondersteuning tags. U kunt geen labels toepassen op klassieke resources.

## <a name="resource-manager-characteristics"></a>Resourcemanager kenmerken

Om u te helpen begrijpen van de twee modellen, laten we eens bekijkt u de kenmerken van resourcemanager typen:

- Via de [portal van Azure](https://portal.azure.com/)gemaakt.

     ![Azure-portal](./media/resource-manager-deployment-model/portal.png)

     Voor berekeningscluster, opslag en netwerkproblemen resources, hebt u de optie van het gebruik van de Resource Manager of klassieke implementatie. Selecteer **Resourcemanager**.

     ![Resourcemanager-implementatie](./media/resource-manager-deployment-model/select-resource-manager.png)

- Gemaakt met de resourcemanager-versie van de Azure PowerShell-cmdlets. Deze opdrachten hebben de indeling *Werkwoord-AzureRmNoun*.

        New-AzureRmResourceGroupDeployment

- Gemaakt door de [Azure resourcemanager REST API](https://msdn.microsoft.com/library/azure/dn790568.aspx) voor REST bewerkingen.

- Gemaakt via Azure CLI opdrachten uitvoeren in de modus **arm** .

        azure config mode arm
        azure group deployment create 

- Het resourcetype is niet inbegrepen **(klassieke)** in de naam. De volgende afbeelding ziet u het type als **opslag-account**.

    ![in de browser](./media/resource-manager-deployment-model/resource-manager-type.png)

## <a name="classic-deployment-characteristics"></a>Klassieke implementatie kenmerken

U kunt het implementatiemodel klassieke ook bekend als het model servicebeheer.

Resources die zijn gemaakt in het implementatiemodel klassieke delen de volgende kenmerken:

- Gemaakt via de [klassieke portal](https://manage.windowsazure.com)

     ![Klassieke portal](./media/resource-manager-deployment-model/classic-portal.png)

     Of de Azure-portal en u **klassieke** implementatie (voor berekeningscluster, opslag en netwerken) opgeven.

     ![Klassieke implementatie](./media/resource-manager-deployment-model/select-classic.png)

- Gemaakt door de servicebeheer-versie van de Azure PowerShell-cmdlets. Deze opdrachtnamen hebben de indeling *Werkwoord-AzureNoun*.

        New-AzureVM 

- Gemaakt door de [Service Management REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) voor REST bewerkingen.
- Gemaakt via Azure CLI opdrachten uitgevoerd in de modus **asm** .

        azure config mode asm
        azure vm create 

- Het resourcetype bevat **(klassieke)** in de naam. De volgende afbeelding ziet u het type als **opslag-account (klassieke)**.

    ![klassieke type](./media/resource-manager-deployment-model/classic-type.png)

U kunt de Azure-portal gebruiken voor het beheren van resources die zijn gemaakt door klassieke implementatie.

## <a name="changes-for-compute-network-and-storage"></a>Wijzigingen voor berekeningscluster-, netwerk- en -opslag

In het volgende diagram wordt berekeningscluster-, netwerk- en resources die zijn geïmplementeerd via resourcemanager weergegeven.

![Resourcemanager architectuur](./media/virtual-machines-azure-resource-manager-architecture/arm_arch3.png)

Houd rekening met de volgende relaties tussen de bronnen:

- Alle resources bestaan binnen een resourcegroep.
- De virtuele machine, is afhankelijk van een specifieke opslag-account dat is gedefinieerd in de opslagruimte resource-provider voor de opslag van de schijven in blobopslag (vereist).
- De virtuele machine verwijst naar een specifieke NIC gedefinieerd in de resource netwerkprovider (vereist) en een beschikbaarheid instellen in de provider van de resource berekeningscluster gedefinieerde (optioneel).
- De NIC wordt verwezen naar de VM toegewezen IP-adres (vereist), het subnet van het virtuele netwerk voor de virtuele machine (vereist) en aan een beveiligingsgroep netwerk (optioneel).
- Het subnet binnen een virtueel netwerk wordt verwezen naar een beveiligingsgroep netwerk (optioneel).
- Het exemplaar van de verdeling van laden verwijst naar de groep backend IP-adressen die opnemen van de formule voor het berekenen van een virtuele machine (optioneel) en verwijst naar een laden verdeling openbaar of privé IP-adres (optioneel).

Dit zijn de onderdelen en hun onderlinge relatie voor klassieke implementatie:

![klassieke architectuur](./media/virtual-machines-azure-resource-manager-architecture/arm_arch1.png)

De klassieke oplossing voor het hosten van een virtuele machine bevat:

- Een vereiste cloudservice die als een container fungeert voor host voor virtuele machines (berekenen). Virtuele machines worden automatisch ingevuld met een netwerkadapter (NIC) en een IP-adres door Azure toegewezen. Daarnaast bevat de cloudservice een exemplaar van de verdeling van externe laden, een openbare IP-adres en Standaardeindpunten wilt toestaan dat externe bureaublad en externe PowerShell-verkeer is toegestaan voor virtuele machines op basis van Windows en Secure Shell (SSH)-verkeer is toegestaan voor Linux gebaseerde virtuele machines.
- Een vereiste opslag-account waarin de VHD's voor een virtuele machine, waaronder het besturingssysteem, tijdelijke en aanvullende gegevensschijven (opslag) opgeslagen.
- Een optioneel virtueel netwerk die als een extra container, waarin u kunt de structuur van een subnet maken en het subnet waarop de virtuele machine bevindt aanwijzen (netwerk).

De volgende tabel worden de wijzigingen in de interactie van berekeningscluster, het netwerk en opslag resource-providers beschreven:

 Item | Klassieke | Resourcemanager
 ---|---|---
| Cloudservice voor virtuele Machines |  Cloudservice is een container voor het vasthouden van de virtuele machines die beschikbaarheid van het platform en taakverdeling vereist. | Cloudservice is niet langer een object dat is vereist voor het maken van een virtuele Machine met het nieuwe model. |
| Virtuele netwerken | Een virtueel netwerk is optioneel voor de virtuele machine. Als opgenomen, kan niet het virtuele netwerk met resourcemanager worden geïmplementeerd. | Virtuele machine moet een virtueel netwerk dat is geïmplementeerd met bronbeheer. |
| Opslag-Accounts | Het virtuele machine moet een opslag-account waarin de VHD's voor het besturingssysteem, tijdelijke en aanvullende gegevensschijven opgeslagen. | Het virtuele machine moet een opslag-account voor de opslag van de schijven in blobopslag. |
| Beschikbaarheid van Sets | Beschikbaarheid aan het platform is aangegeven door de dezelfde "AvailabilitySetName" configureren op de virtuele Machines. Het maximum aantal foutenstructuuranalyse domeinen is 2. | Beschikbaarheid van de Set is een resource die worden aangeboden door Microsoft.Compute-Provider. Virtuele Machines waarvoor beschikbaarheid moeten worden opgenomen in de beschikbaarheid van de groep. Het maximum aantal foutenstructuuranalyse domeinen is nu 3. |
| Affiniteit groepen | Affiniteit groepen zijn vereist voor het maken van virtuele netwerken. Door de introductie van regionale virtuele netwerken, die was echter niet vereist meer. |Het concept affiniteit groepen wordt om te vereenvoudigen, bestaat niet in de API's die worden aangeboden door Azure resourcemanager. |
| Taakverdeling    | Het maken van een Cloudservice biedt een impliciete taakverdeling voor de virtuele Machines geïmplementeerd. | De taakverdeling is een resource die worden aangeboden door de provider Microsoft.Network. De primaire netwerk-interface van de virtuele Machines dat moet worden verdeeld moet worden verwezen naar de taakverdeling. Netwerktaakverdelers zijn interne en externe gebruikers. Een exemplaar van de verdeling van laden verwijst naar de groep backend IP-adressen die opnemen van de formule voor het berekenen van een virtuele machine (optioneel) en verwijst naar een laden de verdeling van openbaar of privé IP-adres (optioneel). [Meer informatie.](../articles/resource-groups-networking.md) |
|Virtuele IP-adres | Als een VM wordt toegevoegd aan een cloudservice, krijg Cloudservices van een standaard-VIP (virtuele IP-adres). Het virtuele IP-adres is het adres dat is gekoppeld aan de impliciete taakverdeling.    | Openbare IP-adres is een resource die worden aangeboden door de provider Microsoft.Network. Openbare IP-adres kan zijn statisch (gereserveerd) of dynamisch. Dynamische openbare IP-adressen kan worden toegewezen aan een taakverdeling. Openbare IP-adressen kan worden beveiligd met beveiligingsgroepen. |
|Gereserveerde IP-adres|   U kunt een IP-adres in Azure reserveren en koppelen aan een Cloudservice om ervoor te zorgen dat het IP-adres Plaktoetsen is.   | Openbare IP-adres kan worden gemaakt in de modus voor "Statische" en biedt dezelfde mogelijkheid als een 'gereserveerde IP-adres. Statische openbare IP-adressen kan alleen worden toegewezen aan een taakverdeling nu. |
|Openbare IP-adres (PIP) per VM | Openbare IP-adressen kunnen ook worden gekoppeld aan een VM rechtstreeks. | Openbare IP-adres is een resource die worden aangeboden door de provider Microsoft.Network. Openbare IP-adres kan zijn statisch (gereserveerd) of dynamisch. Alleen dynamische openbare IP-adressen kunnen echter aan een netwerk-Interface om een openbare IP-adres per VM nu worden toegewezen. |
|Eindpunten| Invoer eindpunten nodig op virtuele machines zijn geopend Connectivity voor bepaalde poorten zijn geconfigureerd. Een van de algemene manieren verbinden met virtuele machines uitgevoerd door het instellen van de invoer eindpunten. | Regels voor binnenkomende NAT kan worden geconfigureerd op netwerktaakverdelers naar dezelfde mogelijkheid van het inschakelen van de eindpunten op specifieke poorten om verbinding te maken om de VMs te bereiken. |
|DNS-naam| Een cloudservice krijgt een impliciete uniek DNS-naam. Bijvoorbeeld: `mycoffeeshop.cloudapp.net`. | DNS-namen zijn optioneel parameters die kunnen worden opgegeven voor een resource openbare IP-adres. De FQDN-naam heeft de volgende indeling - `<domainlabel>.<region>.cloudapp.azure.com`. |
|Netwerkinterfaces | Primaire en secundaire netwerk-Interface en de eigenschappen zijn gedefinieerd als netwerkconfiguratie van een virtuele machine. | Netwerkinterface is een resource die worden aangeboden door Microsoft.Network-Provider. De levenscyclus van het netwerk is niet gekoppeld aan een virtuele Machine. Deze verwijst naar de VM toegewezen IP-adres (vereist), het subnet van het virtuele netwerk voor de virtuele machine (vereist) en aan een beveiligingsgroep netwerk (optioneel). |

Zie voor meer informatie over het verbinden van virtuele netwerken van andere implementatiemodellen, [verbinding maken met virtuele netwerken van andere implementatiemodellen in de portal](./vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-to-resource-manager"></a>Migreren van klassiek naar resourcemanager

Als u wilt migreren van uw bronnen van klassieke implementatie naar resourcemanager implementatie, Zie:

1. [Technische uitgebreide kennismaking op platform ondersteund migratie van klassiek naar Azure resourcemanager](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
2. [Platform ondersteund migratie van IaaS resources uit klassieke naar Azure resourcemanager](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
3. [Migreren IaaS resources van klassiek naar Azure resourcemanager met behulp van Azure PowerShell](./virtual-machines/virtual-machines-windows-ps-migration-classic-resource-manager.md)
4. [Migreren IaaS resources van klassiek naar Azure resourcemanager met behulp van Azure CLI](./virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Kan ik een virtuele machine Azure Resource Manager gebruiken om te implementeren in een virtueel netwerk of opslag-account gemaakt met behulp van klassieke implementatie maken?**

Dit wordt niet ondersteund. U kunt Azure resourcemanager niet gebruiken voor de implementatie van een virtuele machine in een virtueel netwerk die is gemaakt met klassieke implementatie.

**Kan ik een virtuele machine met bronbeheer Azure van de afbeelding van een gebruiker die is gemaakt met de Azure-Service API's maken?**

Dit wordt niet ondersteund. U kunt echter de VHD-bestanden van een opslag-account dat is gemaakt met de Service API's kopiëren en deze toevoegen aan een nieuwe account via Azure resourcemanager die.

**Wat is de invloed op het quotum voor mijn abonnement?**

De quota voor de virtuele machines, virtuele netwerken en opslag-accounts die zijn gemaakt door de resourcemanager van Azure zijn gescheiden van andere quota. Elk abonnement krijgt quota voor de resources die met de nieuwe API's maken. U vindt meer informatie over de quota voor extra [hier](../articles/azure-subscription-service-limits.md).

**Kan ik mijn geautomatiseerde scripts gebruiken voor het inrichten van virtuele machines, virtuele netwerken en opslag accounts via de resourcemanager-API's blijven?**

Alle automatisering en scripts die u hebt gemaakt blijven functioneren voor de bestaande virtuele machines, virtuele netwerken die zijn gemaakt met het beheer van Azure-Service-modus. De scripts wel moeten worden bijgewerkt zodat het nieuwe schema voor het maken van dezelfde resources tot en met de resourcemanager-modus.

**Waar vind ik voorbeelden van Azure resourcemanager sjablonen?**

Een uitgebreide reeks starter sjablonen vindt u op [Azure resourcemanager QuickStart sjablonen](https://azure.microsoft.com/documentation/templates/).

## <a name="next-steps"></a>Volgende stappen

- Als u wilt bij het maken van de sjabloon die een virtuele machine definieert begeleidt, Zie opslag-account en virtueel netwerk, [resourcemanager sjabloon Stapsgewijze instructies](resource-manager-template-walkthrough.md).
- De opdrachten voor het implementeren van een sjabloon, raadpleegt u [een toepassing met Azure resourcemanager sjabloon Deploy](resource-group-template-deploy.md).
