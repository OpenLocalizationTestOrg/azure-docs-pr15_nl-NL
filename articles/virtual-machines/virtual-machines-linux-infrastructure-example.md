<properties
    pageTitle="Voorbeeld infrastructuur scenario | Microsoft Azure"
    description="Meer informatie over de belangrijkste ontwerpen en implementeren richtlijnen voor het implementeren van de infrastructuur van een voorbeeld in Azure wordt aangegeven."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="example-azure-infrastructure-walkthrough"></a>Voorbeeld van Azure-infrastructuur Stapsgewijze instructies

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

In dit artikel worden doorlopen building uit de infrastructuur van een voorbeeld-toepassingen. We Detailstijlen ontwerpen van een infrastructuur voor een eenvoudige online winkel die bij elkaar alle richtlijnen en beslissingen rond naamgevingsconventies, beschikbaarheid sets, virtuele netwerken en netwerktaakverdelers brengt en distribueren van uw virtuele machines (VMs).


## <a name="example-workload"></a>Voorbeeld werkbelasting

Adventure Works Cycle wil maken van een toepassing online store in Azure wordt aangegeven dat bestaat uit:

- Twee nginx servers uitvoeren van de client front in een weblaag
- Twee nginx servers verwerking van gegevens en orders in een toepassingslaag
- Twee MongoDB servers deel van een een laptopgeheugen cluster voor het opslaan van productgegevens en orders in een databaselaag
- Twee Active Directory-domeincontroller voor klant- en leveranciers in een laag verificatie
- Alle servers bevinden zich in twee subnetten:
    - een front subnet voor de webservers 
    - een back-enddatabase subnet voor de toepassingsservers, MongoDB cluster en domeincontrollers

![Diagram van verschillende niveaus voor infrastructuur toepassingen](./media/virtual-machines-common-infrastructure-service-guidelines/example-tiers.png)

Inkomende secure webverkeer moet worden verdeeld over de webservers terwijl klanten in de winkel online zoeken. Volgorde netwerkverkeer in de vorm van HTTP aanvraagt vanaf het web servers moeten worden verdeeld over de toepassingsservers. Daarnaast moet de infrastructuur zijn ontworpen voor maximale beschikbaarheid.

De resulterende ontwerp omvat:

- Een Azure-abonnement en -account
- Een één resourcegroep
- Opslag-accounts
- Een virtueel netwerk met twee subnetten
- Beschikbaarheid wordt ingesteld voor de VMs met een vergelijkbare rol
- Virtuele machines

Alle het bovenstaande volgt u deze naamgevingsconventies:

- Adventure Works Cycle gebruik **[IT werkbelasting]-[locatie]-[Azure resource]** als een voorvoegsel
    - In dit voorbeeld "**azos**" (Azure Online Store) is de naam van de IT-werkbelasting en de locatie "**Gebruik**" (Oost VS 2) is
- Opslag accounts adventureazosusesa**[beschrijving]** gebruiken
    - 'adventure' is toegevoegd aan het voorvoegsel te leveren uniekheid en accountnamen opslagruimte biedt geen ondersteuning voor het gebruik van afbreekstreepjes.
- Virtuele netwerken AZOS-gebruik-VN**[aantal]** gebruiken
- Beschikbaarheid sets gebruiken azos-gebruiken-als-**[rol]**
- VM namen gebruiken azos-gebruiken-vm -**[vmname]**


## <a name="azure-subscriptions-and-accounts"></a>Azure-abonnementen en -accounts

Adventure Works Cycle gebruikt om de facturering voor deze werkbelasting IT hun Enterprise-abonnement met de naam Adventure Works Enterprise-abonnement.


## <a name="storage-accounts"></a>Opslag-accounts

Adventure Works Cycle vastgesteld dat ze twee opslag accounts nodig:

- **adventureazosusesawebapp** voor de standaard opslag van de webservers, toepassingsservers en domeincontrollers en hun gegevensschijven.
- **adventureazosusesadbclust** voor de Premium-opslag van de MongoDB een laptopgeheugen cluster-servers en hun gegevensschijven.


## <a name="virtual-network-and-subnets"></a>Virtuele netwerk en subnetten

Omdat het virtuele netwerk niet nodig heeft voor lopende connectiviteit met het netwerk van de on-premises implementatie Adventure werk maal, besloten ze op een virtueel netwerk alleen de cloud.

Ze een virtueel netwerk alleen de cloud hebt gemaakt met de volgende instellingen met behulp van de Azure portal:

- Naam: AZOS-gebruik-VN01
- Locatie: Oost Amerikaans 2
- Virtuele netwerk-adresruimte: 10.0.0.0/8
- Eerste subnet:
    - Naam: FrontEnd
    - Adresruimte: 10.0.1.0/24
- Tweede subnet:
    - Naam: BackEnd
    - Adresruimte: 10.0.2.0/24


## <a name="availability-sets"></a>Beschikbaarheid van sets

Als u wilt behouden beschikbaarheid van alle vier niveaus van hun online store, besloten Adventure Works Cycle op vier beschikbaarheid sets:

- **azos-gebruik-als-web** voor de webservers
- **azos--als-app gebruiken** voor de toepassingsservers
- **azos gebruiken als db** voor de servers in de MongoDB een laptopgeheugen cluster
- **azos gebruiken als domeincontroller** voor de besturing voor domein


## <a name="virtual-machines"></a>Virtuele machines

Adventure Works Cycle besloten van de volgende namen voor hun VMs Azure:

- **azos-gebruik-vm-web01** voor de eerste webserver
- **azos-gebruik-vm-web02** voor de tweede webserver
- **azos-gebruik-vm-app01** voor de eerste toepassingsserver
- **azos-gebruik-vm-app02** voor de tweede toepassingsserver
- **azos-gebruik-vm-db01** voor de eerste MongoDB-server in het cluster
- **azos-gebruik-vm-db02** voor de tweede MongoDB-server in het cluster
- **azos-gebruik-vm-dc01** voor de eerste domeincontroller
- **azos-gebruik-vm-dc02** voor de tweede domeincontroller

Hier ziet u de resulterende configuratie.

![Infrastructuur definitief toepassingen geïmplementeerd in Azure wordt aangegeven](./media/virtual-machines-common-infrastructure-service-guidelines/example-config.png)

Deze configuratie omvat:

- Een virtueel netwerk voor alleen de cloud gebruikt met twee subnetten (front-end en back-end)
- Twee opslag-accounts
- Vier beschikbaarheid sets, één voor elke laag van de online store
- De virtuele machines voor de vier lagen
- Een externe taakverdeling instellen voor het HTTPS gebaseerde webverkeer van Internet naar de webservers
- Een interne laden verdeeld instellen voor niet-versleutelde webverkeer van de webservers met de toepassingsservers
- Een één resourcegroep


## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 